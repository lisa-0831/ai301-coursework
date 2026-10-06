# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The candidate plan's stated cause or diagnosis, read against the repro-evidence block in eval mode or the student's posted repro evidence in live mode. | Pass if the diagnosis is supported by the reproduced observations and does not contradict them. The plan may infer a cause, but the inference must explain the observed failure rather than ignore evidence that points elsewhere. | required |
| scope-bounded | The plan's scope, files or areas to change, and any stated exclusions, read against the issue context and repo facts. | Pass if the proposed work is one bounded change aimed at the reproduced problem, with enough limits to distinguish the fix from unrelated cleanup or redesign. It must not introduce changes that are unnecessary for the issue without explaining why they are required. | required |
| approach-executable | The plan's files or areas and implementation approach, read against the diagnosis, issue context, and repro evidence. | Pass if a contributor unfamiliar with the author's private reasoning could begin implementing the fix from the plan, and the proposed change addresses the cause supported by the evidence rather than only masking the observed symptom. Exact line numbers or code are not required. | required |
| test-proves-fix | The candidate plan's test plan, read against the repro evidence's steps, inputs, failure signal, and observable output. | Pass if the planned test would distinguish the broken behavior from the fixed behavior by re-running the reproduction or an equivalent check through the real affected code, and it states an observable expected result after the fix. | required |
| uncertainty-honest | The plan's diagnosis, risks, unknowns, assumptions, and deviations, read against the issue and repro evidence. | Pass if factual claims do not exceed what the available evidence supports and any material uncertainty that could change the implementation or verification is identified rather than presented as certain. The plan does not need to invent risks when none are apparent. | required |
| comms-and-conventions | The candidate plan comment read against the candidate plan, issue/thread highlights, repo-facts block, contribution instructions, templates, and any stated disclosure requirements. | Pass if the comment accurately represents the plan, responds to relevant maintainer or thread constraints, and does not violate stated repository contribution conventions. If the repo states no special requirement, absence of an invented requirement does not fail this check. | required |

## Verdict rule

Accept only if every required check is `pass`.

A `fail` on any required check produces `reject`.

An `unclear` on any required check also produces `reject`, because a plan that cannot be verified from the available package is not ready to post and build from.

If a future rubric row is marked `preferred`, its grade may be reported but never changes the final verdict.
