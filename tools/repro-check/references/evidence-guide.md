# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives:
In an eval package, look at the environment record in the repro report and compare it with the issue context and any relevant environment requirements in the repo-facts block. 
In live mode, look at the issue thread, the repository's bug template or setup documentation, and the environment recorded in the student's repro draft.

What good looks like:
It records the version, OS, and other environment details the repository or issue requires. The tested version matches the issue's target, or any relevant difference is explicitly called out.

## Steps

Where it lives:
In an eval package, look at the reproduction steps in the repro report and compare them with the issue context and any relevant setup requirements in the repo-facts block. 
In live mode, use the student's draft repro report or comment together with the issue thread and the repository's README, CONTRIBUTING, or other setup documentation.

What good looks like:
The steps identify a clear starting state and every material action needed to reach the trigger, so a stranger can re-run the reproduction without guessing an unstated step. When the issue gives a specific command, input, or trigger, the steps use it or explicitly call out any difference, and the expected and actual results are stated.

## Behavior shown

Where it lives:
In an eval package, look at the output excerpts, logs, screenshots, or other artifacts in the repro report and compare them with the behavior described in the issue context. 
In live mode, use the artifacts produced by the student's reproduction run together with the issue thread and the draft repro report or comment.

What good looks like:
The artifact directly supports the result of the reproduction attempt. It may show the reported behavior, show that the target behavior did not occur under the tested conditions, or show that a necessary trigger or precondition could not be achieved. Any such limitation is stated explicitly. Evidence of a different or adjacent failure is not evidence of the issue's target behavior.

## Honesty

Where it lives:
In an eval package, compare the outcome stated in the repro report with its environment, steps, and supporting artifacts. 
In live mode, compare the conclusion in the student's draft repro comment or report with the evidence produced by the actual reproduction run.

What good looks like:
The stated outcome says no more than the evidence supports. A successful reproduction is supported by matching evidence, while an honest cannot-reproduce is acceptable when the recorded environment, steps, and artifacts support that conclusion; a confident claim that conflicts with the evidence does not pass.

## Comms

Where it lives:
In an eval package, read the claim comment and repro comment against the issue context and the repository's stated contribution requirements in the repo-facts block. 
In live mode, compare the student's draft comments with the issue thread, CONTRIBUTING or other contributor documentation, issue or PR templates, and any stated AI-use disclosure requirements.

What good looks like:
The comments are specific to the issue, accurately describe what the student intends to investigate or what the evidence shows, and follow the repository's stated communication, template, and disclosure requirements. They do not substitute generic boilerplate for issue-specific information or make claims that the available evidence does not support.
