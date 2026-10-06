# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives**

- Eval mode: the candidate plan's diagnosis or stated cause, read against the package's repro-evidence block and relevant issue context.
- Live mode: the diagnosis in `plan.md` and `comment.md`, read against the student's own posted Unit 2 reproduction comment on the issue.
- Use reproduced observations, outputs, successful controls, and failure signals as grounding evidence. Do not automatically treat the issue author's proposed explanation as reproduced fact.

**What good looks like**

The stated cause explains the behavior that was actually reproduced and does not contradict observations in the reproduction evidence. An inference may be acceptable when the observations support it, but a plan that simply repeats an unverified suspected cause is not grounded.

## Scope

**Where it lives**

- Eval mode: the candidate plan's scope statement, files or areas to change, exclusions, and implementation description, read against the issue context and repo facts.
- Live mode: the same parts of `plan.md`, checked against the actual issue and relevant repository structure or contribution guidance.

**What good looks like**

The plan proposes one bounded change tied to the reproduced problem. The affected files or areas and practical limits are clear enough to distinguish the intended fix from unrelated cleanup, redesign, or drive-by refactoring.

## Executability

**Where it lives**

- Eval mode: the candidate plan's files or areas, implementation approach, and order or description of work, read alongside the grounded diagnosis.
- Live mode: the corresponding implementation section of `plan.md` and any necessary repository context permitted by the skill.

**What good looks like**

A contributor can identify where to begin and what behavior to change without needing the author's private reasoning. The approach addresses the cause supported by the reproduction evidence rather than merely suppressing the visible symptom. Exact code, line numbers, or a complete patch are not required.

## Test plan

**Where it lives**

- Eval mode: the candidate plan's test plan read against the steps, inputs, outputs, and failure signal in the repro-evidence block.
- Live mode: the test plan in `plan.md` read against the student's posted Unit 2 reproduction steps and output.

**What good looks like**

The verification re-runs the original reproduction or an equivalent check through the real affected code and names an observable result that would differ before and after the fix. A generic statement such as "run tests" is not decisive unless those identified tests directly demonstrate the reproduced bug is gone.

## Honesty

**Where it lives**

- Eval mode: the candidate plan's diagnosis, assumptions, risks, unknowns, and deviations, compared with the issue and repro evidence.
- Live mode: the corresponding claims in `plan.md` and `comment.md`; after a build, also inspect the `## Deviations` entry in the updated plan.

**What good looks like**

Claims stay within what the evidence establishes or clearly label reasonable inference and material uncertainty. Unknowns that could change implementation or verification are not presented as settled facts. No special risks section is required when there is no material uncertainty to report.

## Comms

**Where it lives**

- Eval mode: the candidate plan comment, issue/thread highlights, repo-facts block, contribution policy, templates, and any stated disclosure requirements in the package.
- Live mode: `comment.md` read against the GitHub issue thread and the repository's relevant contributing instructions, templates, or policy files.
- In Path Review live mode, apply the classroom house rules from `scope.md`, including that another student's plan does not block this student's own plan.

**What good looks like**

The comment accurately summarizes the student's own diagnosis, scope, approach, and verification plan; it does not contradict the fuller plan; it responds to relevant maintainer requests; and it follows stated repository conventions. If the evidence contains no special requirement, the student is not required to invent one.
