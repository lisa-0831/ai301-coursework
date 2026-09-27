# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | Read the repro report's environment record and the repo-facts block. | Pass if the report identifies the environment needed to understand what was tested, including the project version or commit and the relevant OS/runtime/tool version when applicable. A maintainer should not need to ask which version or environment produced the result. | required |
| steps-rerunnable | Read the reproduction steps in the repro report, including commands, inputs, setup, or configuration they reference. | Pass if a stranger could follow the stated steps and construct an equivalent attempt at the same trigger without guessing an essential condition. Exact byte-for-byte input is not required when the report states the relevant properties needed to recreate an equivalent input. | required |
| behavior-matches-issue | Compare the issue's described behavior with the repro report's stated result and its actual output or artifact. | If the report claims a successful reproduction, pass only if the artifact shows the same target behavior or failure described by the issue. If the report explicitly says it could not reproduce, pass when the artifact honestly shows the attempted target did not occur and the report scopes that result to the environment and attempt actually tested. A report that claims success while showing a different error, failure mode, or merely related symptom fails. | required |
| outcome-honest | Compare the report's stated conclusion with its steps, observed output, and artifact. | Pass if the conclusion accurately describes what the evidence shows. A successful reproduction must be supported by evidence of the target behavior. An evidenced cannot-reproduce result also passes if it is stated honestly and the attempted reproduction is documented. | required |
| repo-conventions | Read the repo-facts block for contribution, communication, and AI-use rules, then check the claim comment and repro report against those rules. | Pass if the comments respect the repository's stated conventions. If the repository requires a disclosure or other specific communication requirement, the package must satisfy it. | required |

## Verdict rule

Accept (ready) if every required check passes.

Reject (hold) if any required check fails or is unclear.

Preferred checks, if added later, never change the verdict.
