# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The candidate PR's changed files and hunks, read against the plan-context scope, boundary, test plan, recorded deviation notes, and the PR description's claims. | Pass if every material change is within the plan's stated scope and boundary, or is tied to an explicit deviation note, and the PR description accurately describes what the diff delivers. An honestly disclosed limitation, deferred edge, or recorded deviation may still pass. Fail if the diff silently does more or less than the plan, or if the description claims work or fidelity that the diff contradicts. | required |
| test-evidence | The candidate PR's test-evidence section, read against the plan-context test plan and repro evidence, plus the visible outcome of repository-required checks named in the package. | Pass if the evidence shows the relevant behavior was checked, states the expected-after result, shows before/after reproduction results, and makes the outcome visible. Repository-required checks must be run when the package or plan requires them, with the check commands or names and their specific outcomes stated. A generic "tests pass" statement without identifiable checks and outcomes is insufficient. An honest cannot-reproduce, limitation, or failed check may pass only when it is accurately reported and does not claim unsupported success. Fail if the evidence proves no observable behavior, omits required repository checks, or presents an unsupported success claim as verified. | required |
| diff-quality | The candidate PR's unified diff and commit list, read for the actual fix, unrelated hunks, debug leftovers, dead code, commented-out blocks, and formatting churn. | Pass if the intended change is identifiable, no unrelated cleanup or drive-by edits ride along, and distracting material does not materially obscure review. Fail if unrelated edits are included, even when disclosed, or if debris or formatting churn bury the intended change. A change needed to implement the fix is not unrelated merely because it required a recorded plan deviation. | required |
| standards-and-comms | The repo-facts block's PR template, CONTRIBUTING instructions, stated contribution and AI-use policies, explicit maintainer or thread requests, and the candidate PR title and description. | Pass if every applicable repository-stated requirement and explicit maintainer request is addressed with specific, truthful content, including the required AI-use disclosure. Fail if a required template or disclosure ask is ignored, missing, or replaced with generic boilerplate. The description's factual agreement with the diff is judged by `plan-fidelity`. | required |

## Verdict rule

Accept only if every required check passes.

Reject if any required check fails or is unclear.
Preferred checks never change the verdict.
