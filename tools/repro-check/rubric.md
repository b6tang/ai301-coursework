# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | Use the Environment sources in `references/evidence-guide.md`: the repro report's environment record read against the issue context and relevant repo-facts. | Pass if the package records the environment details needed to place the reproduction, including the tested version and relevant platform information, and the tested conditions match the issue's target or any relevant difference is explicitly called out. | required |
| steps-followable | Use the Steps sources in `references/evidence-guide.md`: the repro report's reproduction steps read against the issue context and relevant setup requirements. |Pass if the steps give a clear starting point, include every action needed to trigger the issue, follow the issue's command, input, or trigger when provided, explain any differences, and state the expected and actual results. | required |
| behavior-evidenced | Use the Behavior shown sources in `references/evidence-guide.md`: the repro report's output excerpts, logs, screenshots, or other artifacts read against the behavior described in the issue. | Pass if the artifacts bear directly on the issue's target behavior and make the observed result checkable. A reproduced result can pass when the reported behavior is shown. An evidenced cannot-reproduce can also pass when the target behavior does not occur, or when a necessary trigger or precondition cannot be achieved, as long as that limitation is explicitly identified and supported by the artifacts. Fail if the artifacts show a different failure instead of the issue's reported behavior, and the package claims that the issue was reproduced. | required |
| outcome-honest | Use the Honesty sources in `references/evidence-guide.md`: the repro report's stated outcome read against its environment, steps, and supporting artifacts. | Pass if the stated outcome does not claim more than the evidence supports. An evidenced successful reproduction or an evidenced cannot-reproduce can pass; a confident conclusion that conflicts with or exceeds the evidence fails. | required |
| repo-conventions | Use the Comms sources in `references/evidence-guide.md`: the claim and repro comments read against the issue context, repo-facts block, contribution policy, templates, and any stated disclosure requirements. | Pass if the comments are specific and accurate, follow the repository's stated communication, template, and disclosure requirements, and do not substitute generic boilerplate or unsupported claims for issue-specific information. | required |


## Verdict rule

Accept only if every required check passes. Reject if any required check fails or is unclear.