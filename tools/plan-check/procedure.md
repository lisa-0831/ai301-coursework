# Procedure: how this skill grades a plan package

## Read order

1. Determine whether the run is eval mode or live mode. In live mode, first apply `scope.md` as required by `SKILL.md`; in eval mode, treat the supplied package as the entire evidence set and do not fetch outside information.
2. Read the issue context before the candidate plan. Record the reported problem, expected behavior, affected area, and any explicit maintainer constraints without yet deciding whether the candidate plan is correct.
3. Read the reproduction evidence next. Record the exact reproduced inputs or steps, the observable failure, relevant successful controls or comparison behavior, and any facts that narrow the cause.
4. Read the repo-facts or repository-conventions evidence and record only requirements relevant to the proposed change or plan comment.
5. Read the candidate plan after the issue and reproduction evidence. Record its diagnosis, proposed scope, files or areas, implementation approach, test plan, and any stated risks, assumptions, unknowns, or deviations.
6. Read the candidate plan comment last. Record what it promises to change and test, and compare those claims with the full candidate plan and any relevant thread or repository instructions.
7. Do not grade any rubric check until the preceding evidence has been gathered. Reading the reproduction before judging the plan prevents a plausible-sounding diagnosis from becoming the assumed truth.

## Evidence gathering

1. For `diagnosis-grounded`, pair each material cause claimed by the plan with the reproduction observation that supports or contradicts it. Do not treat the issue author's suspected cause as reproduced evidence unless the reproduction independently supports it.
2. For `scope-bounded`, record the files or areas the plan says it will change, what behavior those changes target, any exclusions, and any proposed work that appears unrelated to the reproduced failure.
3. For `approach-executable`, record the concrete implementation actions the plan proposes and the files or components where they occur. Then compare those actions with the supported diagnosis to determine whether they address the supported cause rather than only the symptom.
4. For `test-proves-fix`, record the original reproduction inputs and failure signal, then record the plan's proposed verification command, action, or check and its expected observable result. Note whether the check runs through the real affected behavior or an equivalent real-code path.
5. For `uncertainty-honest`, list claims in the plan that go beyond directly observed reproduction facts and check whether they are supported, reasonably inferred, or explicitly identified as assumptions or unknowns. Also record any stated risks or deviations that materially affect the build.
6. For `comms-and-conventions`, compare the candidate comment with the plan, issue/thread highlights, and repository facts. Record relevant maintainer requests, contribution instructions, template requirements, and disclosure rules. Do not invent a convention when the supplied evidence states none.
7. Use the locations and definitions in `references/evidence-guide.md` for every evidence family. If the required evidence is genuinely absent from the allowed package, record that absence rather than searching elsewhere or filling it in from assumptions.

## Check execution

1. Grade the checks in this order: `diagnosis-grounded`, `scope-bounded`, `approach-executable`, `test-proves-fix`, `uncertainty-honest`, then `comms-and-conventions`.
2. For each check, use only the evidence gathered for that check and apply its pass condition exactly as written in `rubric.md`.
3. Grade `pass` when the available evidence satisfies the pass condition, and cite the specific fact or comparison that decided it.
4. Grade `fail` when the available evidence shows that the pass condition is not satisfied, and cite the contradictory, missing-critical, or inadequate fact that decided it.
5. Grade `unclear` only when the allowed evidence is genuinely insufficient to decide whether the pass condition is satisfied. Do not use `unclear` merely because a plan is short; a terse plan can pass when the necessary evidence is present.
6. Do not silently repair a weak plan by supplying implementation details, assumptions, tests, or rationale that the candidate did not provide or quote.
7. Once the evidence needed for a check has been recorded, do not re-read the whole package unless two recorded facts conflict. Resolve such a conflict from the original allowed evidence and record which fact controls the grade.

## Verdict assembly

1. After all checks have a grade, apply the verdict rule from `rubric.md` without adding additional unstated criteria.
2. Produce `accept` only when every required check is `pass`.
3. Produce `reject` if any required check is `fail` or `unclear`.
4. For each check, output one concise evidence line containing the fact, comparison, or absence that actually decided that grade rather than a generic statement such as "looks good."
5. If several required checks fail, report all of them; do not stop after the first failure.
6. In live mode, separately report any voice-guide violation required by `SKILL.md`, but do not let it change the verdict unless a rubric check also makes that violation verdict-relevant.
7. Finish with the valid fenced JSON block required by `SKILL.md`, using the exact check names from `rubric.md` and the final `accept` or `reject` verdict.
