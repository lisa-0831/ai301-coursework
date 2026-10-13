# Evidence guide: where evidence lives in a PR package

This guide is the map `procedure.md` points to under Evidence gathering.
For each of the four evidence families it says where the evidence lives
in an eval package, where it lives in a live PR, and what observable
evidence satisfies the rubric checks that read it. The families map to
the rubric's seven checks like this:

| Family (harness category) | Rubric checks it feeds |
|---|---|
| Plan fidelity (silent-drift) | `plan-fidelity`, `issue-alignment`, the fidelity half of `claim-accuracy` |
| Test evidence (not-tested) | `test-evidence`, `test-coverage`, the testing half of `claim-accuracy` |
| Diff quality (unreviewable) | `diff-quality` |
| Standards and comms (standards-wall) | `repo-standards` |

A package that fails none of the four families is a clear-accept.

**Ground rules for every family**

- **Eval mode: the bundle is the whole world.** Use only the package's
  own sections: the header (source, captured date), `## Repo facts`,
  `## Issue`, `## Thread highlights`, `## Plan context`, and
  `## Candidate PR` with its `### Title`, `### Description`,
  `### Commits`, `### Diff`, and `### Test evidence` subsections. Do not
  open the `source` link, the upstream repo, or any local file, and do
  not fill gaps from outside knowledge of the project. Evidence missing
  from the bundle is recorded as missing; it never counts in the PR's
  favor.
- **Live mode: committed artifacts only.** Use `plan.md` (including
  `## Deviations`), `git diff main...HEAD --stat` and
  `git diff main...HEAD` from the working copy, `pr_draft.md`,
  `test_evidence.md`, the issue's GitHub thread, and the target repo's
  `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.
  Uncommitted work is not part of the PR.
- **Descriptions are claims, not proof.** The PR title, description,
  checklists, and prose in the test-evidence section are assertions to
  verify against the diff, the plan, and printed output. A specific
  reproduction record is weighed only under the combination rule in
  the Test evidence family below.
- **Disclosure counts.** A limitation, deviation, skipped check, or
  failing check that is stated plainly with its actual outcome is not a
  failure by itself. Judge whether the remaining evidence still meets
  the check's pass condition.

## Plan fidelity (harness category: silent-drift)

**Where it lives in an eval package**

- *What was promised:* `## Plan context`. Read the plan's steps (the
  implementation approach), its `Files:` list, its `Not in scope:`
  boundary, its `Test plan:`, and any recorded deviation. The repro
  evidence paragraph at the top of the plan context states the failure
  the plan was built on.
- *What the issue asked for:* `## Issue` (problem, expected behavior,
  reproduction) and `## Thread highlights` (maintainer constraints or
  direction).
- *What actually changed:* `### Diff` (every `--- a/` / `+++ b/` file
  header and every hunk) and `### Commits` (commit subjects show intent
  and whether unrelated work rides along).
- *What the PR says changed:* `### Title` and `### Description`.

**Where it lives in a live PR**

- *Promised:* `plan.md`: its approach, planned files or areas,
  exclusions, test strategy, and the `## Deviations` section.
- *Issue:* the issue's GitHub thread, including reproduction details and
  any maintainer comments that narrow or redirect the fix.
- *Changed:* `git diff main...HEAD --stat` for the complete changed-file
  list, `git diff main...HEAD` for content, and `git log main..HEAD` for
  commit subjects.
- *Claimed:* the title and description in `pr_draft.md`.

**What satisfies the checks**

Build a two-column comparison: each planned step and file on one side,
each changed file and substantive hunk on the other.

- `plan-fidelity` passes when every changed file is on the plan's file
  list, is covered by a deviation note that explains why, or is a small
  supporting change whose link to the plan is evident from the diff
  itself (for example, a test file next to the planned source file).
  Every material planned step is visible in the diff or explicitly
  noted as dropped or changed. Nothing in the diff touches what the plan
  put out of scope.
- Silent drift in the "more" direction: a changed file, behavior, or
  refactor that the plan does not mention and no deviation note
  explains. Silent drift in the "less" direction: a planned step or file
  with no corresponding change and no note. A deviation note does not
  excuse unrelated scope; it only re-ties a mismatch that serves the
  plan's goal.
- `issue-alignment` passes when the changed code path is the one the
  issue and repro implicate, and the change plausibly produces the
  issue's expected behavior. It fails when the diff adds a separate
  feature or redesign, or fixes something other than the reported
  failure. It does not require proof of perfection.
- The fidelity half of `claim-accuracy`: every claim in the title and
  description about what changed, what stayed unchanged, or that the PR
  "implements the plan exactly" must match the diff. A description that
  claims more than the diff delivers, or omits a material change the
  diff makes, is drift and fails here, not under standards and comms.

Example of a match (calib-01): the plan lists `Files: themes.gitconfig`
and one removal step with other themes out of scope; the diff touches
only `themes.gitconfig`, removing exactly the shadowed `#434C5E` line;
the description claims that removal and nothing more.

## Test evidence (harness category: not-tested)

**Where it lives in an eval package**

- *The proof offered:* `### Test evidence` under `## Candidate PR`:
  commands, their printed output, before/after results, and any
  disclosed failures or skips.
- *Tests added or changed:* test files in `### Diff`.
- *What should have been proven:* the `Test plan:` in `## Plan context`
  and the repro evidence there (the before state and the exact repro
  command); the reproduction steps in `## Issue`.
- *What the PR claims about testing:* the testing statements in
  `### Description`.

**Where it lives in a live PR**

- *Proof:* `test_evidence.md` (captured commands and their real
  output).
- *Tests changed:* test files in `git diff main...HEAD`.
- *Promised:* the test strategy in `plan.md` and the reproduction steps
  from the issue thread or the repro pack.
- *Claims:* the testing section and checklist in `pr_draft.md`.

**What satisfies the checks**

First sort everything in the test-evidence section into three piles.
Keep them separate: a comment or prose line never moves into the first
pile, however specific it is.

- *Printed output:* a command line (typically `$ ...`) followed by
  text that the command printed: a result line, an error, a test
  runner's summary with counts, a diff of output. This is direct
  evidence. Illustrative before/after examples in `### Description`
  (often requested by a PR template) are claims, not evidence. A
  transcript in the test-evidence section stays printed output even if
  it repeats or is cross-referenced by such an example. Verify the
  example against that transcript, with the before-state checked
  against the plan's repro evidence. It does not need its own
  verbatim run.
- *Specific reproduction records:* the author's written record of
  re-running the repro, including results written as shell comments
  (`# before: ...`), annotated transcripts, and keystroke or UI-action
  flows. This is not printed output. A record is specific only when it
  (1) names the issue's exact trigger (the command, input, keystrokes,
  or UI actions from the issue or plan repro, not a substitute case),
  and (2) states a concrete, checkable result that tells the states
  apart: the exact error or message text, a value, an exit code, which
  files are listed, which view or toast appears. For GUI and TUI
  behavior that prints nothing to a terminal, this action-and-visible-
  result record is the native form of evidence. A screenshot or
  capture that is referenced but not in the bundle adds nothing by
  itself; the written record is what gets graded.
- *Unsupported assertions:* generic success prose ("works now",
  "colors work", "verified locally", "renders unchanged", "all tests
  pass", "verified on my machine"); placeholders (`<output here>`,
  `TODO`); checklist ticks; and records that lack a named trigger or a
  concrete result. These are claims.

Tests added in the diff are a fourth input, read from `### Diff`, not
from the evidence section. An *issue-specific regression test* sets up
the issue's trigger and asserts the expected after-state (it usually
cites the issue). Test source shows what was written, not that it ran
or passed.

**The combination rule.** The central fix is verified when either:

- printed output shows the after-state of the issue's trigger (the
  repro re-run, or the regression test's own runner output); or
- a specific reproduction record shows that after-state, AND an
  issue-specific regression test in the diff asserts the same
  behavior, AND a stated test run names that test or has a scope that
  contains it (for example, "`cargo test -p <crate>` passes" where the
  test lives in that crate, or "passes, including the new case").

The two legs cover each other's gaps: the record shows the real
trigger behaving correctly, and the regression test pins that same
behavior in the repo's own suite, where a reviewer or CI can re-run it.
Under this rule a complete raw test log is not required.

Neither leg is enough alone. A regression test plus "tests pass", with
no specific record and no printed run, verifies nothing about the
issue. A specific record with no corroborating test and no printed
output rests on the author's word. And generic success prose never
counts, with or without added tests.

Then apply the checks:

- `test-evidence` passes when the combination rule is met for the
  central fix. Failed, partial, or unavailable checks pass this check
  when the actual outcome is shown or honestly stated with the reason
  (for example, "suite not run: needs a GPU; ran the unit test for the
  changed module, output below"), and a reviewer can still assess the
  change. It fails when generic claims, placeholders, test source alone,
  or invented success stand in for verification, or when the
  description claims a result the evidence contradicts.
- `test-coverage` passes when the issue's essential behavior is
  exercised on the issue's own trigger, and its after-state is
  established under the combination rule. The before state may come
  from the PR's own evidence or from the plan context's repro evidence.
  Each material case in the plan's test plan is either shown, encoded
  in a regression test assertion, replaced by an equivalent check, or
  explicitly disclosed as a limitation. It fails when a critical
  promised case is silently missing, when only a control case or an
  unchanged path is exercised, or when the only test run does not touch
  the affected behavior (for example, an unrelated suite passing, or an
  added test that asserts something other than the fix).
- The testing half of `claim-accuracy`: each claim about the central
  behavior must be backed by printed output, a specific reproduction
  record, or a regression test assertion within a stated run, or stated
  as a limitation. A secondary claim reported specifically (a named
  suite with its count, "fmt and clippy clean", "`go generate` produces
  no diff") is recorded as unverified; it fails the check only when
  something in the package contradicts it or when it is the only
  support offered for the central fix. A bare "tests pass" standing in
  for verification of the fix is an unsupported claim and fails.

Decisive evidence names the observable behavior, states the expected
after-state, and shows or specifically records the result that matches
it. A secondary smoke check reported only in prose does not establish
the central behavior, but it does not sink a package whose central
repro is verified. Example (calib-01): the configparser repro is re-run
and prints `parsed OK`; the before-state error is shown in the plan
context's repro evidence; the `--show-syntax-themes` smoke check is a
prose claim on a secondary case.

Patterns, each judged by the rules above:

- *Annotated transcript plus regression test (verified).* The repro
  commands appear, with before and after results written as comments
  quoting the exact error and the exact new behavior, and the diff adds
  a test that sets up the issue's input and asserts the fixed result,
  covered by a stated run of its suite.
- *TUI action flow plus integration test (verified).* "press `s` ->
  toast '<exact text>' -> no prompt; file still listed", and the diff
  adds an integration test that presses the same key and expects the
  same toast.
- *Generic success plus added test (not verified).* "Tested locally:
  works now. `cargo test` passes." There is no trigger, no concrete
  result, and no before-state, so the added test is uncorroborated.
- *Specific record on the wrong case (coverage fails).* A precise
  record or real output for a control case, a different input, or an
  unchanged path, while the plan's repro trigger is never re-run.

## Diff quality (harness category: unreviewable)

**Where it lives in an eval package**

- `### Diff`: the complete set of file headers and hunks, including
  every `+` and `-` line.
- `### Commits`: subjects that reveal unrelated work ("wip", "fix lint
  everywhere", "bump deps") or accidental commits.
- `## Repo facts`: any stated expectation about PR size or focus (for
  example, "PRs stay small").

**Where it lives in a live PR**

- `git diff main...HEAD --stat` for the full changed-file list,
  including files that should not be there (lockfiles, build output,
  editor or OS files, local config).
- `git diff main...HEAD` for line-level content.
- `git log main..HEAD` for commit subjects.
- `docs/CONTRIBUTING.md` for stated focus or size expectations.

**What satisfies the checks**

`diff-quality` passes when a reviewer can see the fix and nothing
material obscures it. Check each changed line for these debris tells:

- debug output (`print`, `console.log`, `dbg!`, temporary logging);
- commented-out code blocks, dead code, or leftover experiments;
- accidental files (artifacts, lockfile churn unrelated to the change,
  `.DS_Store`, local configs, secrets);
- unrelated refactors, renames, or drive-by edits in code the fix does
  not need;
- mass formatting or whitespace churn that buries the substantive lines.

Do not fail a diff for harmless formatting on lines it already touches,
or for necessary supporting edits (a test, an import, a doc line for the
changed behavior) merely because they add lines. Size alone is not a
failure; obscured intent is. Example (calib-01): a one-line removal in
the single planned file, no other hunks.

## Standards and comms (harness category: standards-wall)

**Where it lives in an eval package**

- *The repo's asks:* `## Repo facts`, specifically the
  `pull requests:` line (PR template present or not and its required
  sections, contribution asks such as "reference the issue") and the
  `contribution policy` line (CONTRIBUTING.md asks and any AI-use
  policy or disclosure requirement).
- *Explicit maintainer direction:* `## Thread highlights`.
- *How the PR answers them:* `### Title` and `### Description`.

**Where it lives in a live PR**

- *Asks:* `.github/PULL_REQUEST_TEMPLATE.md` (its section headings and
  checklist), `docs/CONTRIBUTING.md` (issue-linking, size, commit, and
  AI-use rules), and maintainer comments in the issue thread.
- *Answers:* the title and description in `pr_draft.md`.
- *Scope house rules:* `scope.md` (own fork and branch, one bounded PR
  per issue, use of the repo's PR template), read first in live mode.

**What satisfies the checks**

List only the requirements the sources actually state, then match each
to the draft:

- Every required template section is present with real content
  specific to this change, or with an N/A plus a short reason where the
  section does not apply. A section left as template boilerplate,
  emptied, or deleted fails that ask.
- A required issue reference is present (for example, `#2211`,
  `Fixes #2211`).
- An AI-use disclosure is present when the stated policy asks for one.
  When no policy asks, its absence is not a failure and its presence is
  harmless.
- Explicit maintainer direction in the thread is followed or engaged
  with in the description.

Do not invent requirements. If the repo facts say there is no PR
template, do not require template sections; if they say there is no AI
policy, do not require a disclosure; do not fail on cosmetic
differences from a template. Whether the description's claims match the
diff is plan fidelity, above, not this family. Example (calib-01): the
repo facts state no template, ask for small PRs that reference the
issue, and state no AI policy; the description references `#2211`, so
the only stated asks are met.
