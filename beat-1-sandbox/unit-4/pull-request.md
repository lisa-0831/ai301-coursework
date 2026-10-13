# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/182

**Branch**

`fix/62-health-redis-settings`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Initial full run (all 20 scored packages): 14/20 agreement.
2. Intermediate targeted run: 7/8 agreement. This run covered only eight selected packages,
   not all twenty, so it checked whether specific fixes worked rather than measuring overall
   agreement.
3. Final full run (all 20 scored packages): 18/20 agreement, recorded in `eval-run.txt` as:

```
agreement: 18/20 scored items  (bar: 18/20: PASS)
```

**Package analysis**

I chose `pkg-11`. Its gold label is `accept`, but my rubric returned `reject`. The final run
records it as:

```
pkg-11  clear-accept    accept  reject   NO     failed: claim-accuracy
```

Because `pkg-11` is in the `clear-accept` category, the expected outcome was that all seven
required checks pass. The eval log reports `failed: claim-accuracy` for it; I am not
assuming that line lists every per-check result. My reading is that the grader applied `claim-accuracy`
too strictly: it most likely treated one of the PR's claims as unsupported or contradicted
when the rubric would have allowed it, for example by recording it as unverified or
accepting it as a disclosed limitation. I have not inspected `pkg-11`'s underlying PR
package or the grader's per-check reasoning, so I can't name the exact claim that triggered
the failure. This is an interpretation of the recorded result, not a confirmed diagnosis.

**Check rationale**

The `claim-accuracy` pass condition in `tools/pr-precheck/rubric.md` reads exactly:

> Pass if the description accurately represents what was changed, tested, omitted, or left uncertain. A behavioral claim is supported when the diff implements it and printed output, a specific reproduction record, or an issue-specific regression test assertion within a stated run shows it. A specific secondary claim (a named suite or lint run with its scope or counts, generated files unchanged) that nothing contradicts and that is not the sole support for the central fix is recorded as unverified but does not fail this check. A disclosed shortfall is not automatically a failure. Fail if the PR claims a fix, test, or behavior that the diff or evidence contradicts or leaves entirely unsupported, claims completeness or unchanged behavior that the artifacts contradict, or hides a material contradiction between its description and the artifacts.

The rule separates central behavioral claims from secondary test claims because they carry
different risk. A claim that the fix changes behavior is the core of the PR, so it needs
concrete support: the diff has to implement it, and printed output, a reproduction record,
or a regression test assertion has to show it. Secondary claims such as "lint passed" or
"the full suite ran with N tests" are usually not backed by raw logs in a PR package. If
every such claim had to be proven, nearly every honest PR would fail. So a secondary claim
that nothing contradicts, and that isn't the only support for the fix, is noted as
unverified instead of being failed.

Transparent limitations should not lead to automatic rejection because disclosure is what
a reviewer wants. A PR that says "I could not run the integration suite locally" is giving
an accurate account of what was tested. Rejecting it would reward PRs that stay quiet about
gaps over PRs that report them. The check fails on contradiction, unsupported central claims,
or hidden mismatches, not on honest uncertainty.

**Trade-offs**

This check trades some recall on good PRs for precision on bad ones. In the final run, the
skill classified every package in the four reject categories correctly:

```
categories: clear-accept 5/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3
```

The cost shows up in `clear-accept`, where two good packages were rejected, both on the
same check:

```
pkg-11  clear-accept    accept  reject   NO     failed: claim-accuracy
pkg-16  clear-accept    accept  reject   NO     failed: claim-accuracy
```

That is the observed result: all four reject categories were classified correctly, and
`pkg-11` and `pkg-16` were falsely rejected with `claim-accuracy` as the reported failing
check. Why the grader decided that way is not shown by these results. One possible
explanation is that when it is unsure whether a claim is supported, it leans toward failing
rather than recording the claim as unverified, but I have not verified this, and I am not
claiming it is what produced the correct rejections. I accept this miss: wrongly rejecting
a good PR only costs a second look, while wrongly accepting a PR with unsupported claims
would let it go out.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
