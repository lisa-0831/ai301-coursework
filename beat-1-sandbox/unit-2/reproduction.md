# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

lisa-0831

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5816885188

Picking this up. The `/health` Redis probe is reading `settings.redis_host` and `settings.redis_port`, while `Settings` exposes `redis_url`. I’m going to reproduce the reported 503 and `AttributeError` locally, then post a repro report with my environment, steps, and observed output before making any code changes.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5849797309

I reproduced the Redis health-check failure on commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`.

Environment:
- macOS: 15.5
- Architecture: arm64
- Python: 3.12.2
- Docker: 29.2.0
- Docker Compose: v5.5.1

Steps:

1. Followed the repository's initial setup in `docs/SETUP.md`:

```bash
cp .env.example .env
docker compose up -d
make setup
```

Then started the application:

```bash
make run
```

2. Confirmed that Redis itself was reachable:

```bash
docker compose exec redis redis-cli ping
```

Output:

```text
PONG
```

3. Called the health endpoint:

```bash
curl -s -o /tmp/health.json -w "HTTP %{http_code}\n" http://localhost:8000/health
cat /tmp/health.json
```

Observed:

```text
HTTP 503
```

The response reports Redis as unhealthy:

```json
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-26T20:31:48.565709"}}
```

At the same time, the foreground API output from the terminal running `make run` shows the Redis health check failing with:

```text
redis_health_check_failed
error="'Settings' object has no attribute 'redis_host'"
```

This reproduces the reported Redis health-check bug. Redis itself is reachable, as shown by the `PONG` response, but `/health` reports Redis as unhealthy because the probe tries to read `settings.redis_host` and `settings.redis_port`, while `Settings` exposes `redis_url`.

The same `/health` request also triggered a separate PostgreSQL health-check failure:

```text
postgres_health_check_failed
error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
```

Because of that separate failure, the overall HTTP 503 cannot be attributed only to the Redis bug. However, the Redis portion of issue #62 is independently reproduced by the successful Redis `PONG`, the `"redis": "unhealthy"` health response, and the `redis_host` `AttributeError` in the API log.

## Eval iterations

**Run history**

- Smoke test with `--limit 3`: 3/3 agreement.
- First full evaluation run: 18/20 agreement.
- Second full evaluation run: 17/20 agreement.
- Targeted rerun with `--only pkg-05,pkg-09,pkg-10`: 3/3 agreement.
- Final full evaluation run saved to `eval-run.txt`: 19/20 agreement.

**Package analysis**

I looked closely at `pkg-05`. After revising the rubric, my skill accepted the package, which matched the gold label. The package gave enough information for a stranger to construct an equivalent reproduction attempt, but it did not require an exact byte-for-byte copy of the original input. My earlier version of `steps-rerunnable` was too strict about reproducing the exact input, so it incorrectly rejected this case. I revised the check to focus on whether the report states the essential conditions needed to construct an equivalent attempt without guessing.

**Check rationale**

My current `steps-rerunnable` check says:

> Pass if a stranger could follow the stated steps and construct an equivalent attempt at the same trigger without guessing an essential condition. Exact byte-for-byte input is not required when the report states the relevant properties needed to recreate an equivalent input.

I revised this check after `pkg-05` exposed that my earlier wording was too strict. A reproduction report does not always need the exact original input if it clearly describes the properties that matter to triggering the behavior. The revised check still requires the essential conditions to be explicit, but it does not reject a valid report only because the reproducer constructs an equivalent input.

**Trade-offs**

The trade-off is that making `steps-rerunnable` accept equivalent inputs is more permissive than requiring the exact original input. That could accept a report whose constructed input differs in an unimportant way from the reporter's original artifact. I re-ran `pkg-05`, `pkg-09`, and `pkg-10` with `--only` after the rubric changes, and all three matched their gold labels. The final full run improved to 19/20, so the change fixed those false rejections without broadly loosening the rubric across the evaluation set.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
