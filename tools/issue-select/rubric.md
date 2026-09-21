# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Repo facts: last 5 default-branch commits, maintainer first-response sample, and maintainer activity in the comment thread | Pass if there is evidence of human maintainer activity within 90 days of the bundle capture date, either through a human-authored default-branch commit, a maintainer response to a recent issue, or a maintainer comment in this issue. Bot-only activity does not count. | required |
| repo-active | Repo facts: archived flag, latest release, and last push to any branch | Pass if the repo is not archived and has either a release or push within 180 days of the bundle capture date. | required |
| scope-fit | Issue body and comment thread | Pass if the issue describes one specific bug, feature, documentation task, or other coherent outcome, and provides enough information to understand the desired result or a concrete next step. A task may touch multiple related files, be old, involve technically difficult code, or name multiple possible causes and still pass if the overall goal is bounded and actionable. Fail if it is an umbrella/tracking/megaissue or codebase-wide campaign, primarily requires an unresolved design or product decision, lacks enough specification to know the desired behavior, or the thread shows prolonged unresolved design debate. Do not fail solely because there was a previous closed or abandoned PR. | required |
| unclaimed | Repo facts: assignees and linked PRs; comment thread for claim comments or mentioned PRs | Pass only if there is no current assignee, no open PR implementing the issue, and no comment showing that someone is currently working on it. Closed or abandoned PRs do not count as an active claim. | required |
| contribution-policy | Repo facts: contribution policy | Pass if the repo either says nothing about AI-assisted contributions or allows them with conditions such as disclosure, testing, understanding, or human review. Fail if the policy explicitly bans AI-generated or AI-assisted contributions. | required |

## Verdict rule

Accept only if every required check passes.

Treat `unclear` on any required check as a fail.

Preferred checks, if added later, may be used to rank accepted issues but never change the accept/reject verdict.
