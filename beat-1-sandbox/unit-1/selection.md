# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62

**Verdict output**

Re-graded #62 against fresh evidence (nothing changed since the batch run — still one comment, no assignee, no PRs in the repo).

- maintainer-active — pass: Aburke225 (COLLABORATOR) has human-authored default-branch commits through 2026-09-16T21:42:18Z and closing comments on repo issues at 2026-09-16T21:51:34Z, 5 days before today.
- repo-active — pass: archived: false, pushed_at: 2026-09-16T21:50:20Z. No releases exist, but the push is well inside 180 days.
- scope-fit — pass: one specific bug, one file, with reproduction steps and the fix already named (redis_url instead of redis_host/redis_port).
- unclaimed — pass: no assignee, zero PRs in the entire repo, no linked PRs on the timeline. The sole comment is classmate skonda29's claim, which the Path Review house rule in scope.md says does not block — claim it anyway, since credit attaches to the PR you open.
- contribution-policy — pass: docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md, and the repo root carry no AI-use statement; no AI_POLICY.md. Silence passes.

Every required check passes, so the verdict is accept. Fit-wise this is the strongest of the three you sent: a backend Python bug in api/routes/health.py with a concrete repro (GET /health → 503 with AttributeError: redis_host in the log) and a bounded fix, which matches your preference for reproducible behavior over architectural work.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
  "checks": [
    {
      "name": "maintainer-active",
      "grade": "pass",
      "evidence": "COLLABORATOR Aburke225 authored default-branch commits through 2026-09-16T21:42:18Z and commented on repo issues at 2026-09-16T21:51:34Z — within 90 days of today (2026-09-21)."
    },
    {
      "name": "repo-active",
      "grade": "pass",
      "evidence": "archived=false; pushed_at=2026-09-16T21:50:20Z, 5 days before today (no releases, but push is within 180 days)."
    },
    {
      "name": "scope-fit",
      "grade": "pass",
      "evidence": "One bounded bug with repro steps and a named fix: \"Settings in core/config.py doesn't define either field; it carries a single redis_url, which the probe should use instead.\""
    },
    {
      "name": "unclaimed",
      "grade": "pass",
      "evidence": "assignees=[]; repo has 0 PRs total and no linked PRs on the timeline; the only comment is classmate skonda29's claim (author_association NONE), which the Path Review house rule says does not block."
    },
    {
      "name": "contribution-policy",
      "grade": "pass",
      "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no AI-use statement; no AI_POLICY.md or AGENTS.md in the repo — silence passes."
    }
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

agreement: 2/3 scored items

agreement: 1/1 scored items

agreement: 17/20 scored items (bar: 18/20: below the bar)

agreement: 3/3 scored items

agreement: 19/20 scored items (bar: 18/20: PASS)

agreement: 19/20 scored items (bar: 18/20: PASS)

**Issue analysis**

`issue-15` — my rubric returned `accept`, while the gold label was `reject`.

The rubric passed the issue because the repository evidence looked healthy and the task appeared bounded. The bundle says `"last push to any branch: 2026-08-04"` and `"latest release: 12.1 (2026-06-26)"`, so the repo passed my activity checks. It also says `"assignees: none; linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)"`, so my `unclaimed` check passed.

For scope, the issue describes a concrete change: `"if message starts with a bot mention then we should separate that out into a command field and put the rest of the message in the text field."` Under my current `scope-fit` rule, that looked like one coherent and actionable goal, so the skill accepted it.

The result missed the risk visible in the long history of repeated claims and abandoned attempts. For example, the bot says `"you have been unassigned from this issue because you have not updated this issue or any referenced pull requests for over 14 days."` That pattern appears multiple times in the thread, which suggests more hidden difficulty than my rubric captured.

**Check rationale**

`scope-fit`:

> | scope-fit | Issue body and comment thread | Pass if the issue describes one specific bug, feature, documentation task, or other coherent outcome, and provides enough information to understand the desired result or a concrete next step. A task may touch multiple related files, be old, involve technically difficult code, or name multiple possible causes and still pass if the overall goal is bounded and actionable. Fail if it is an umbrella/tracking/megaissue or codebase-wide campaign, primarily requires an unresolved design or product decision, lacks enough specification to know the desired behavior, or the thread shows prolonged unresolved design debate. Do not fail solely because there was a previous closed or abandoned PR. | required |

I made this check focus on whether the task is bounded and actionable rather than simply whether it is small or easy. My earlier version was too strict and rejected issues such as `issue-01`, `issue-09`, and `issue-19` even though they had a clear outcome. I revised it so that multiple files, older issues, or technically difficult work can still pass when the desired result is specific enough to act on.

**Trade-offs**

The trade-off is that this broader `scope-fit` check can miss hidden difficulty that only becomes clear from a long issue history. In the final run, `issue-15` was accepted by my rubric even though its gold label was `reject`. The issue looked bounded on the surface, but its thread showed repeated claims, abandoned attempts, and unresolved history over several years. I accepted this trade-off because tightening the check too much had previously caused clear, actionable issues such as `issue-01`, `issue-09`, and `issue-19` to be rejected.

---

## Selection rationale

**Selection rationale**

1. Issue #62 fits my interests because it is a small backend Python bug with clear reproduction steps and a concrete fix direction. I am more interested in backend debugging than a documentation-only task, and the scope looks manageable within the time available.

2. The verdict correctly identified that the repository and maintainers are active, the issue is bounded and reproducible, there is no blocking assignee or PR, and the contribution policy does not prevent me from working on it. Beyond the rubric, I also considered whether I would actually enjoy debugging this type of backend issue and whether the fix would give me useful experience navigating an unfamiliar codebase.

3. I expect claiming it to be slightly awkward because another student has already commented that they would like to work on it. However, the Path Review course rules say that another student's claim does not block the issue, so I can still claim it for the course.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
