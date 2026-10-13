---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Grade exactly one PR package to answer one question: is this pull request ready to submit?

A PR package contains the candidate PR title and description, its commits and diff, test evidence, and the plan it claims to implement, read against the corresponding issue and repository standards.

Evaluate the actual changes and their evidence, not the contributor's confidence or writing style. Do not grade unrelated issues, propose new features, or substitute your own expectations for the written rubric.

## Inputs and modes

Select exactly one mode based on the user's request.

**Live mode:** Grade the student's own draft PR before it is opened.

- Read `plan.md`, including any `## Deviations` section.
- From the repository working copy, obtain the committed branch changes using `git diff main...HEAD` (three dots). Use `git diff main...HEAD --stat` to identify every changed file. Do not silently treat uncommitted work as part of the PR.
- Read the draft PR title and description from `pr_draft.md`.
- Read actual test commands and their printed results from `test_evidence.md`.
- Read the issue's GitHub thread, including its problem statement, reproduction details, constraints, and relevant discussion.
- Read `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` from the target repository.
- Compare the diff with the plan, tests with the plan's test strategy, and PR claims with the diff and test output.
- For a house-chain student, use the instructor-provided house issue, house plan, house reproduction pack, and working branch instead of the student's original issue chain. Apply the same checks.

**Eval mode:** Grade exactly one frozen practice submission.

- Read only the supplied package bundle, including its issue context, plan context, candidate PR, diff, test evidence, and stated repository standards.
- Treat the bundle as the entire evidence universe. Do not fetch GitHub data, inspect local repository files, or supplement missing information with outside knowledge.
- Evaluate every rubric check and apply the complete verdict rule, even when an early check fails.

## The scope seam (live mode only)

Read `scope.md` before gathering any other live-mode evidence. Confirm that the PR targets the repository named on its `Repo:` line and obeys its house rules.

If the repository still has a placeholder, stop without grading and instruct the student to update the `Repo:` line. Never infer or invent a repository.

Refuse to grade work outside the scoped repository. Enforce applicable house rules, including the student's own fork and branch, one bounded PR per issue per student, and use of the repository's PR template.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

Read `voice-guide.md` in live mode. Check both the draft PR title and description against its writing rules.

If the draft breaks a rule, identify the specific rule, quote or locate the offending text, and suggest a concrete correction in the readable summary.

The voice guide does not independently change the final verdict unless `rubric.md` contains an applicable check.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read and use all three grading components:

1. `rubric.md` defines the named checks, their evidence requirements, pass conditions, weights (`required` or `preferred`), and the final verdict rule.
2. `references/evidence-guide.md` identifies where each evidence type appears in an eval package or live PR and what adequate evidence looks like.
3. `procedure.md` specifies the execution workflow under Read order, Evidence gathering, Check execution, and Verdict assembly.

Execute `procedure.md` as written, rather than inventing an alternative review process.

If the procedure omits a necessary operation, report the gap explicitly rather than silently improvising around it. Do not add improvised checks or unstated rejection criteria.

If `rubric.md` contains no substantive checks or `procedure.md` contains no substantive steps, refuse to grade and explain which component needs to be completed. Template comments alone do not count as content.

## Verdict and output

The final verdict is binary: `accept` means ready to submit, and `reject` means hold for corrections. There is no third verdict or numerical score.

Grade each check as `pass`, `fail`, or `unclear`. Combine those grades using the verdict rule in `rubric.md`.

Before the machine-readable result, provide a concise per-check summary identifying the deciding evidence, missing proof, and actionable corrections. In live mode, include relevant voice-guide observations.

End every completed grading response with the following fenced JSON block, using the exact schema and key order shown. Replace placeholders with the actual item, check names, grades, evidence, and verdict. Output nothing after this block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first:** Every check must cite a specific fact, changed file, diff line, plan requirement, or actual command output. A bare statement that tests passed is not proof.
- **Grade the artifact, not the polish:** A concise but complete PR can pass. A polished description cannot excuse unplanned changes, missing tests, or unsupported claims.
- **Compare rather than assume:** Check each changed file against the plan, each planned test against actual evidence, and each PR claim against the diff and output.
- **Respect repository standards:** Apply documented PR-template and contributing requirements, but do not manufacture standards not present in the evidence.
- **The rubric decides:** Apply each check's stated pass condition and weight exactly, even when intuition suggests a different outcome.
- **The procedure decides how:** Follow the authored procedure, expose procedural gaps, and do not silently invent steps.
- **Unclear is not a free pass:** Follow the rubric's rule for `unclear`; if it is silent, treat unverifiable required claims as failures when determining the final verdict.
- **Remain within scope:** Grade one PR package at a time, use only permitted sources in the selected mode, and never substitute unrelated evidence.
