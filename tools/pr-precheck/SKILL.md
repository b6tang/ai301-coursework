---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

Grade exactly one PR package per run to answer one question: is this ready to submit?

A PR package contains the candidate PR's title, description, commits, diff, and test evidence. Compare these artifacts with the plan the PR claims to implement and the issue that plan belongs to.

Answer by executing `procedure.md`, applying the checks and verdict rule in `rubric.md`, and locating evidence with `references/evidence-guide.md`.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

Use one of the following modes:

- **Live mode**: read `plan.md`, including its recorded deviation notes, from the working-copy location provided by the user. Read the branch's complete diff by running `git diff main...HEAD` (three dots) from the working copy, and read the branch's commit list. Read the draft PR title and description and the captured test evidence from the locations provided by the user. Read the issue and relevant thread context from GitHub. Take the reproduction evidence from the posted reproduction report or corresponding reference materials supplied for the PR. Gather the scoped repository's PR template, contributing instructions, and other applicable stated policies at their actual repository locations. The plan and reproduction evidence may be authored by the user or supplied as reference materials, but must correspond to the issue and branch being graded; apply the same checks regardless of their source.

- **Eval mode**: use the supplied package bundle containing the issue context, repo-facts block, plan-context block with the accepted plan and repro evidence, and candidate PR. Treat the bundle as the whole world for evidence. Do not fetch GitHub pages, working-copy files, or other external facts. Grade every rubric check and apply the full verdict rule.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` in this tool directory before anything else. Confirm that the issue and candidate PR target the scoped repository, and apply its house rules when interpreting the evidence.

Refuse to grade work outside the scoped repository. If the `Repo:` line still contains an unfilled placeholder, stop without grading and tell the user to fill it with the intended repository according to the instructions in `scope.md`. Never guess a scope.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, read `voice-guide.md` at the point specified by `procedure.md`, after grading the rubric checks. Check the draft PR title and description against the user's rules.

Report any broken rule in the summary, quoting the rule and identifying the draft wording that breaks it. A voice-rule violation alone does not change the verdict; let it affect a grade only if a rubric check explicitly reads that evidence.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

Read `rubric.md` for the checks, named evidence, pass/fail conditions, weights, and verdict rule. Read `references/evidence-guide.md` for where each evidence family lives and what good evidence looks like.

Execute `procedure.md` as written. Follow its read order, evidence-gathering steps, check-execution steps, and verdict-assembly steps. If `procedure.md` does not specify how to perform a necessary step, report the missing instruction instead of inventing one.

If `rubric.md` has no filled checks or `procedure.md` has no written steps, stop without grading and state which component is empty. Template comments and empty table rows do not count as authored content. Do not invent checks or operating steps to replace an empty component.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

Return only `accept` (ready to submit) or `reject` (hold), as determined by the rubric's verdict rule. Do not return a score, a third verdict, or an acceptance with reservations.

Provide a short per-check summary before the final JSON block. For a rejection, include the deciding-check evidence selected by the procedure. Report any procedure-gap or live-mode voice notes before the block. Put reservations in the relevant check evidence lines.

End every completed grading reply with a valid fenced JSON block using exactly the schema below. Include every rubric check in rubric order, with its grade and one evidence line. Preserve the field order and do not add fields. Write nothing after the block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

- Evidence first: name the deciding fact or quote for every grade. Do not use an impression such as "looks fine" as evidence.
- Grade the thing, not the polish: apply the checks to the actual PR artifacts, plan, issue, and stated requirements. Do not substitute description length, commit count, or presentation quality for the rubric's outcome conditions.
- Let the rubric decide: if the evidence satisfies a stated condition, apply that condition even if the result feels wrong. Report any tension in the summary; do not change the rule during the run.
- Let the procedure decide how: follow its written steps and report gaps instead of silently improvising around them.
- Treat `unclear` as the rubric's verdict rule directs. If the rule is silent, count `unclear` as a failure for verdict assembly: an unverifiable claim does not establish readiness.
