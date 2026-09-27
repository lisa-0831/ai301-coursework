# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives**
- In an eval package, check the repo-facts block and the environment record in the repro report.
- In a live package, check the repository's documentation or version information together with the environment stated in the student's repro comment.

**What good looks like**
The report identifies the project version or commit that was tested and any OS, runtime, or tool version that is relevant to the issue. The information should be specific enough that a maintainer knows the circumstances under which the result occurred without having to ask a follow-up question. Do not infer environment details that are not actually recorded.

## Steps

**Where it lives**
- In an eval package, read the reproduction steps and commands in the repro report, along with any setup or configuration described in the repo-facts block.
- In a live package, read the steps and commands in the student's repro comment and compare them with the repository's documented setup when needed.

**What good looks like**
A stranger can start from the stated setup and follow the steps to make an equivalent attempt at the same trigger. Essential commands, inputs, configuration, and setup conditions are stated rather than implied. Exact file contents are not necessary when the report describes the relevant properties clearly enough for someone to construct an equivalent input.

## Behavior shown

**Where it lives**
- Read the issue description to identify the target behavior.
- Then inspect the repro report's observed output and artifacts, such as terminal output, logs, error messages, or screenshots.
- Compare the artifact itself with the behavior described in the issue.

**What good looks like**
For a claimed successful reproduction, the observed evidence shows the same target behavior or failure mode described by the issue. A different error or merely related symptom does not count.

For an explicit cannot-reproduce result, the evidence should show a credible attempt at the target behavior and what actually happened instead. The report should identify relevant differences or limits of the attempted environment rather than presenting the negative result as proof that the issue does not exist.

## Honesty

**Where it lives**
- Compare the conclusion stated in the repro report with the reproduction steps, observed output, and artifacts.
- For a cannot-reproduce result, inspect the attempted steps and evidence showing what actually happened.

**What good looks like**
The report claims only what its evidence supports. A successful reproduction is backed by an artifact showing the target behavior. A cannot-reproduce result is also valid when the attempt is documented and the conclusion accurately states that the target behavior did not occur. Confidence or polished wording cannot substitute for evidence.

## Comms

**Where it lives**
- Compare the claim comment with the specific issue being claimed.
- Compare the claim and repro comments with the repo-facts block, contribution documentation, issue templates, and any stated AI-use or disclosure requirements.
- Look at the actual wording of the comments for issue-specific details rather than generic boilerplate.

**What good looks like**
The claim identifies the issue-specific behavior and says what the contributor will investigate next without promising a fix or a deadline. The repro report communicates the relevant evidence directly and specifically. The comments follow repository-specific contribution and disclosure rules and do not make claims that go beyond the evidence.
