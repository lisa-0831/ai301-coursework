# Procedure: how this skill grades a PR package

## Read order

1. Determine whether the request is live mode or eval mode. In live mode, read `scope.md` first, verify the target repository, and refuse out-of-scope work or an unfilled repository placeholder. In eval mode, use only the supplied practice package without fetching external information.

2. Read the issue context before evaluating the PR. Record the reported problem, expected behavior, reproduction steps, affected components, and any explicit maintainer constraints.

3. Read the plan, including its intended implementation, planned files or areas, test strategy, exclusions, and any documented deviations. Record what the contributor originally committed to doing.

4. Read repository requirements. In live mode, inspect `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and relevant issue discussion. In eval mode, use only the repository facts and standards supplied in the package.

5. Inspect the actual changes. In live mode, run `git diff main...HEAD --stat` and `git diff main...HEAD` from the repository working copy to inspect committed branch changes. In eval mode, read the complete supplied diff and changed-file information. Record every modified file and substantive change.

6. Read the test evidence, including before/after reproduction results, test commands, printed outputs, failures, skipped checks, and disclosed limitations. Compare these with the plan's verification strategy.

7. Read the PR title and description last. Record the summary, issue reference, claimed changes, testing claims, checklist assertions, reviewer notes, and any AI-use disclosure.

8. In live mode, also read `voice-guide.md` and evaluate the draft title and description against its rules. Do not use it in eval mode.

9. Do not assign rubric grades until the evidence has been gathered. Treat descriptions and contributor claims as assertions requiring verification rather than as proof.

## Evidence gathering

1. For `plan-fidelity`, construct a comparison between the plan's intended files and actions and every substantive modification in the diff. Include documented deviations. Identify unplanned work, missing planned changes, and contradictions.

2. For `issue-alignment`, compare the reported failure and expected behavior with the implementation shown in the diff. Record how the change addresses the affected code path and whether any unrelated feature or redesign has been introduced.

3. For `test-evidence`, sort the test-evidence section into the three piles defined in `references/evidence-guide.md`: printed command output, specific reproduction records, and unsupported assertions. Never relabel a comment, test source, or prose summary as printed output; a reproduction record is its own category and counts only when it is specific and corroborated as the guide describes. For each regression test added in the diff, record which trigger it exercises, what it asserts, and whether a stated test run covers it. Record failures and whether their causes are explained.

4. For `test-coverage`, pair each material verification case in the plan with corresponding printed output, a specific reproduction record, a regression test whose assertions encode that case, or an explicitly disclosed limitation. Check that the evidence uses the issue's own trigger and that the central reported behavior is exercised, not merely a control case, an unchanged path, or an unrelated test suite.

5. For `diff-quality`, inspect the complete changed-file list and diff for debug output, commented-out code, unrelated refactors, accidental artifacts, and other material review obstacles. Distinguish necessary supporting edits from unrelated changes.

6. For `claim-accuracy`, extract substantive assertions from the PR title, summary, changes, testing section, and reviewer notes. Classify each as central (the fix, the issue behavior, scope or fidelity, completeness) or secondary (a named suite, lint, format, or generation run reported with its scope). Match each assertion to diff evidence, printed output, a specific reproduction record, a regression test assertion, the plan, or an explicit disclosure. Record contradicted claims, central claims with no support, and secondary claims that remain unverified.

7. For `repo-standards`, identify the repository's applicable template sections, contribution rules, issue-linking requirements, and disclosure requirements. Compare each with the actual draft. Treat a reasoned N/A as acceptable when the section does not apply, and do not invent requirements.

8. Use `references/evidence-guide.md` to locate and interpret the evidence for every check. If evidence is absent from the permitted sources, explicitly record its absence rather than inferring a favorable result.

## Check execution

1. Evaluate all checks in this exact order: `plan-fidelity`, `issue-alignment`, `test-evidence`, `test-coverage`, `diff-quality`, `claim-accuracy`, and `repo-standards`.

2. Apply each check's specific pass condition from `rubric.md`. Do not substitute a personal standard or add requirements not stated in the rubric.

3. Assign `pass` when the evidence satisfies the stated pass condition. Name the concrete comparison, file, command output, or fact supporting the grade.

4. Assign `fail` when the evidence contradicts the pass condition or establishes a material, undisclosed omission. Identify exactly what failed and why.

5. Assign `unclear` only when the available evidence cannot establish either outcome. Record what is missing. Do not use unclear merely because the PR is brief.

6. For test-related checks, distinguish an unsuccessful test result from missing or misleading evidence. An honestly disclosed failure is not automatically grounds for rejection; apply the specific rubric pass condition and assess whether the remaining evidence supports review.

7. For plan deviations, compare actual modifications with any documented explanation. A disclosed deviation is not automatically acceptable if it adds unrelated scope, and a small necessary supporting change is not automatically a failure.

8. Do not assume a checklist claim is true without inspecting the relevant diff or output. Do not report a test as passing on the strength of the author's statement alone; report it as stated and judge whether the combined evidence (printed output, a specific reproduction record, and issue-specific regression tests) still meets the pass condition. Do not require complete raw test logs when that combination suffices, and do not accept a package because tests were added or claimed to pass when no specific verification of the central behavior exists.

9. Complete every rubric check, including after the first failure. If two recorded facts conflict, resolve the conflict using only permitted source evidence and explain which observation controls the grade.

## Verdict assembly

1. Collect one `pass`, `fail`, or `unclear` grade and one concrete evidence statement for each rubric check.

2. Apply the verdict rule from `rubric.md` exactly: return `accept` only when every required check passes. Return `reject` if any required check fails or is unclear.

3. Preferred checks, if any are added later, provide feedback but do not affect the binary verdict.

4. Before the JSON output, provide a concise readable summary listing the check outcomes and actionable corrections for failing or unclear checks. Include all material failures rather than stopping at the first.

5. In live mode, separately report violations of `voice-guide.md`. Do not let voice-guide feedback independently change the verdict unless the rubric makes it relevant.

6. Use the PR URL in live mode or the supplied package identifier in eval mode for the `item` field.

7. Finish with the exact fenced JSON schema specified in `SKILL.md`. Include all check names exactly as written in `rubric.md`, their grades, one-line evidence, and the final `accept` or `reject` verdict.

8. Output nothing after the final fenced JSON block.
