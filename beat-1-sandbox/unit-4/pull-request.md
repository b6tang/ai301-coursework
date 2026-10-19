# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request opened against the Path Review repo and of the evaluation runs that produced `eval-run.txt`.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/225

**Branch**

`fix/67-review-profile-ownership`

## Eval iterations

**Run history**

First full run:

> agreement: 13/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)

Targeted rerun of the seven clear-accept packages:

> agreement: 7/7 scored items

Final full run:

> agreement: 20/20 scored items  (bar: 18/20: PASS)

**Package analysis**

I compared all seven packages where my first run disagreed with the expected verdict. Every disagreement was a `clear-accept` package rejected only by `test-evidence`, which showed that the problem was systematic rather than specific to one package. I selected `pkg-08` as a representative example.

The first run reported:

```text
pkg-08  clear-accept  accept  reject  NO  failed: test-evidence
```

My rubric therefore returned `reject`, while the gold label was `accept`. Its test-evidence explanation said:

> "Integration test `stash_untracked_only_errors` passes; `go test ./...` passes; `go generate ./...` produces no cheatsheet diff." states the checks passed with no commands or printed output shown.

However, the package itself included observable before/after behavior, the expected-after result, a control scenario, and identifiable repository checks with specific outcomes:

> Expected-after per the plan: the toast instead of the prompt, file untouched. Control: with a tracked modification staged, `s` still prompts and creates the stash. Integration test `stash_untracked_only_errors` passes; `go test ./...` passes; `go generate ./...` produces no cheatsheet diff.

The first result therefore treated the absence of printed output as disqualifying even though the package identified each required check and its outcome. After I revised `test-evidence` so that named checks with specific outcomes could satisfy the rubric without requiring one transcript format, the final result for `pkg-08` was:

```text
pkg-08  clear-accept  accept  accept  yes
```

Its final test-evidence explanation said:

> Shows before/after repro, expected-after, a tracked-change control, and names the integration test, `go test ./...`, and `go generate ./...` with outcomes.

**Check rationale**

The current `test-evidence` check reads:

> | test-evidence | The candidate PR's test-evidence section, read against the plan-context test plan and repro evidence, plus the visible outcome of repository-required checks named in the package. | Pass if the evidence shows the relevant behavior was checked, states the expected-after result, shows before/after reproduction results, and makes the outcome visible. Repository-required checks must be run when the package or plan requires them, with the check commands or names and their specific outcomes stated. A generic "tests pass" statement without identifiable checks and outcomes is insufficient. An honest cannot-reproduce, limitation, or failed check may pass only when it is accurately reported and does not claim unsupported success. Fail if the evidence proves no observable behavior, omits required repository checks, or presents an unsupported success claim as verified. | required |

My first full run produced:

> agreement: 13/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)

It also reported:

> categories: clear-accept 0/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3

All seven disagreements failed `test-evidence`. In each disagreement, the explanation relied in whole or in part on the absence of printed command output, even though the package identified repository checks and reported specific outcomes alongside its behavioral evidence. This made the presentation format determine the verdict too broadly.

I revised the check to follow the rubric file's instruction to judge the outcome rather than the shape of the write-up. The current wording requires observable before/after behavior, the expected-after result, identifiable required checks, and their specific outcomes. It does not treat the absence of a complete terminal transcript as an automatic failure.

The revision does not make a bare success claim sufficient. The check still fails generic statements such as "tests pass," omitted required checks, evidence that proves no observable behavior, and unsupported claims of success.

**Trade-offs**

The revised `test-evidence` check gives up using a complete terminal transcript as the only acceptable way to substantiate every repository-check outcome. A named check with a specific reported outcome can satisfy that part of the check when the rest of the evidence establishes the required behavior. The trade-off is that the rubric may not detect a specific but inaccurate test summary when the available evidence contains nothing that contradicts it.

The check still requires observable before/after behavior, the expected-after result, and every repository check required by the package or plan. It continues to reject generic success claims, missing required checks, and evidence that does not demonstrate the relevant behavior.

The targeted rerun of the seven packages affected by the revision reported:

> agreement: 7/7 scored items

I then ran the complete set again to check whether the revision changed packages from the four failure categories. The confirming full run reported:

> categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3

> agreement: 20/20 scored items  (bar: 18/20: PASS)

The full run therefore showed that the revision corrected the seven false rejections without changing the expected rejections in the `not-tested`, `silent-drift`, `standards-wall`, or `unreviewable` categories.

---

Related paths: `eval-run.txt` in this directory; the skill's files in `tools/pr-precheck/`.
