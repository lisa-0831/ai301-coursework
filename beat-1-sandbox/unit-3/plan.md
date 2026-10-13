# Plan for issue #62

## Diagnosis

The Redis portion of `/health` is failing because the health probe constructs its Redis client from configuration fields that do not exist.

The reproduction showed that Redis itself is reachable:

> `docker compose exec redis redis-cli ping`
>
> `PONG`

But the `/health` response still reported:

> `"redis":"unhealthy"`

and the API log showed:

> `redis_health_check_failed`
>
> `error="'Settings' object has no attribute 'redis_host'"`

In `api/routes/health.py`, the probe reads `settings.redis_host` and `settings.redis_port`, while `core/config.py` exposes a single `settings.redis_url`. The failure therefore occurs before the probe can actually ping Redis.

The reproduction also showed a separate PostgreSQL health-check error. Because that is independent of issue #62, fixing the Redis probe may not make the overall `/health` request return HTTP 200.

## Scope

I will update the Redis health-check client construction so it uses the existing `settings.redis_url` configuration instead of the nonexistent `redis_host` and `redis_port` fields.

I will also remove the `attr-defined` type-check suppression for `api.routes.health` from `pyproject.toml`, because the repository's contribution guidance identifies that suppression as belonging to issue #62 and requires it to be removed when the issue is fixed.

I will not change:

- the PostgreSQL health check or its separate `SELECT 1` failure;
- the Vector DB health check;
- the structure of the `/health` response;
- application-wide Redis configuration;
- the `call-overload` or `index` suppressions associated with other health-route issues;
- unrelated error handling or logging.

## Files to change

- `api/routes/health.py`
- `pyproject.toml`

`core/config.py` should not need a change because it already defines the Redis connection setting the application is expected to use:

- `redis_url`

## Approach

1. Update the Redis section of `api/routes/health.py`.
2. Replace the explicit `redis.Redis(host=..., port=..., db=...)` construction with a Redis client created from `settings.redis_url`.
3. Preserve the existing `decode_responses=True` behavior.
4. Drop the explicit `db=0` argument because the database selection is already encoded in `redis_url`, whose current default ends in `/0`.
5. Keep the existing `ping()`, healthy/unhealthy status updates, and logging behavior unchanged.
6. In `pyproject.toml`, remove only the `attr-defined` suppression associated with `api.routes.health` / issue #62, leaving the suppressions associated with the other health-route issues unchanged.
7. Review the diff to confirm that the implementation and suppression cleanup are limited to issue #62.

## Test plan

I will re-run the Unit 2 reproduction against the changed code.

First, confirm Redis is still independently reachable:

```bash
docker compose exec redis redis-cli ping
```

Expected result:

```text
PONG
```

Then run the application:

```bash
make run
```

and in another terminal call the health endpoint:

```bash
curl -s -o /tmp/health.json -w "HTTP %{http_code}\n" http://localhost:8000/health
cat /tmp/health.json
```

For issue #62, the decisive expected results are:

- the Redis dependency is reported as `"healthy"`;
- the API no longer logs `"'Settings' object has no attribute 'redis_host'"`;
- the Redis health check can reach the configured Redis instance through `settings.redis_url`.

I will not require the overall endpoint to return HTTP 200 as proof of this fix, because the Unit 2 reproduction also found an independent PostgreSQL health-check failure that can still make the overall endpoint return HTTP 503.

I will also run the repository's relevant static/type checks to confirm that removing the `attr-defined` suppression does not expose an unresolved issue #62 type error.

I will save the before-and-after command output for the assignment evidence.

## Risks and unknowns

The main known limitation is that the unrelated PostgreSQL health-check failure may keep the overall endpoint unhealthy after the Redis bug is fixed. I will distinguish that existing failure from the Redis result when verifying the change.

The repository currently has no other application usages of `redis_url` that establish a separate Redis-client construction convention, so the implementation will use the Redis library's URL-based client construction directly rather than introducing new configuration fields.

## Deviations

The implementation followed the planned code changes. I updated the Redis health probe to use `settings.redis_url` via `redis.Redis.from_url()`, removed the issue #62 `attr-defined` mypy suppression, and left the unrelated PostgreSQL health-check behavior unchanged.

The runtime verification matched the expected Redis-specific result: Redis returned `PONG`, `/health` reported `"redis":"healthy"`, and the API logged `redis_health_check_passed`. The endpoint still returned HTTP 503 because the separate PostgreSQL health check remains unhealthy.

The repository-wide `make typecheck` could not complete because the repository config targets Python 3.11 while the installed NumPy stubs contain syntax requiring Python 3.12. This failure occurred in `numpy/__init__.pyi` before my change was checked. As a focused verification, I ran `.venv/bin/mypy api/routes/health.py --python-version 3.12`, which completed with `Success: no issues found in 1 source file`.

### Deviation: regression tests added after the `pr-precheck` review

The original plan relied on the manual reproduction alone and did not include an automated test. After the live `pr-precheck` review found this regression-coverage gap, I added a third file that was not in "Files to change":

- `tests/unit/test_health.py` (new; test-only, no application code changed)

It has two `@pytest.mark.unit` / `@pytest.mark.asyncio` tests. Each one calls the real `health_check` coroutine directly with a mocked async database session whose `execute()` succeeds, and patches `redis.Redis.from_url` so no real Redis server is contacted:

- `test_redis_healthy_uses_redis_url` asserts that `from_url` is called once with `settings.redis_url` and `decode_responses=True`, that the mock client's `ping()` is called, and that `dependencies.redis == "healthy"`.
- `test_redis_ping_failure_returns_503` makes `ping()` raise `redis.exceptions.ConnectionError`, then asserts the same `from_url` call, that `ping()` is called, and that an `HTTPException` with `status_code == 503` and `detail["dependencies"]["redis"] == "unhealthy"` is raised.

Both tests pass on this branch and both fail against the pre-fix commit `2f4e82f`. `make test-unit` now reports `377 passed, 53 xfailed, 5 warnings`. See `test_evidence.md` for the results.

These tests do not exercise the HTTP route or FastAPI dependency injection, and they mock PostgreSQL as healthy. They therefore do not show that the real `/health` endpoint returns HTTP 200, which it still does not, because of the separate PostgreSQL issue that this plan leaves out of scope.

### Clarification: the two `--python-version 3.12` mypy records

Two separate mypy diagnostics were recorded, and they should not be confused:

1. **Earlier, one-file check** (the one described above): `.venv/bin/mypy api/routes/health.py --python-version 3.12` → `Success: no issues found in 1 source file`. This result is recorded only in this plan; no log file was saved for it.
2. **Later, 76-file check** (recorded in `test_evidence.md` §4.5): a mypy run with `--python-version 3.12` whose output was `Success: no issues found in 76 source files`. Its exact command line was not recorded.

Neither diagnostic replaces `make typecheck`, which still fails because of the NumPy stub / `python_version = "3.11"` mismatch.
