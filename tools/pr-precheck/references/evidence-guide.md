# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

Where it lives:

In an eval package, read the `plan-context` block's scope, boundary, test plan, and recorded deviation notes against the candidate PR's changed files, unified diff, commit list, and description. Read the description's claims against what the diff actually delivers.

In live mode, read `plan.md`, its scope and boundary, deviation notes, the branch diff from `git diff main...HEAD`, the commit list, and the draft PR title and description.

What good looks like:

Every material changed file and hunk falls inside the plan's stated boundary or is tied to an explicit deviation note. The description accurately states what the diff delivers; an honestly disclosed limitation, deferred edge, or recorded deviation is acceptable, but a claim that the diff contradicts is silent drift.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

Where it lives:

In an eval package, read the candidate PR's `test-evidence` section against the `plan-context` test plan and the reproduction steps, inputs, observed behavior, and artifacts in the `plan-context` repro evidence. Read the visible outcomes of repository checks named by the plan or repo facts.

In live mode, read the captured before-and-after output, screenshots, logs, or other test artifacts from the working copy against the reproduction steps and `plan.md` test plan. Also read the visible outcomes of the repository's required test, lint, build, or other checks.

What good looks like:

The evidence identifies the observable behavior being checked, states the expected-after result, shows before/after reproduction results, and identifies required repository checks by command or name and states their specific outcomes, rather than offering a generic "tests pass" claim. An honest cannot-reproduce result, limitation, or failed check can be valid evidence when it is accurately reported and the PR does not claim unsupported success.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

Where it lives:

In an eval package, read the candidate PR's unified diff and commit list, including the actual fix and all surrounding hunks.

In live mode, read the branch diff from `git diff main...HEAD` and the branch's commit list.

What good looks like:

The intended fix is identifiable, with no unrelated cleanup or drive-by edits riding along, even when disclosed. Debug leftovers, dead code, commented-out blocks, formatting churn, and drive-by edits are evidence of an unreviewable diff when they bury the fix or prevent a reviewer from reliably determining what changed and why.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

Where it lives:

In an eval package, read the `repo-facts` block's PR template asks, `CONTRIBUTING` instructions, stated contribution and AI-use policies, and explicit maintainer or thread requests. Then read the candidate PR's title and description, including the completed template sections and AI-use disclosure.

In live mode, read the repository's PR template and contributing instructions at their actual repository locations, other stated policy files, and relevant issue or review-thread requests. Then read the draft PR title and description.

What good looks like:

Every applicable repository-stated requirement and explicit maintainer request has specific, truthful content, including the required AI-use disclosure. Boilerplate or an ignored template, policy, disclosure, or maintainer request is not compliant. Whether the description's claims match the diff belongs to Plan fidelity above, not this family.
