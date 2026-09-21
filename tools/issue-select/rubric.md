# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Time reference

For relative time windows, use the bundle's `captured` date as the reference date in eval mode, and the current date in live mode.

## Checks

| Check                | Evidence                                                                                                                                                                                        | Pass condition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Weight   |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| maintainer-alive     | Last 5 default-branch commits and their authors/merge provenance; maintainer first-response sample; Owner/Member/Collaborator activity in the issue thread.                                     | Pass if there are at least 2 qualifying maintainer-side activities within the last 60 days. Qualifying activity includes a default-branch commit by an Owner/Member/Collaborator, a human-authored PR reviewed or merged by the project, or an issue/PR response from an Owner/Member/Collaborator.                                                                                                                                                                                                                               | required |
| repo-in-use          | Archived flag, latest release date, and recent default-branch commit/push dates.                                                                                                                | Fail if the repository is archived. Otherwise pass if either the latest release or repository push activity occurred within the last 6 months.                                                                                                                                                                                                                                                                                                                                                                 | required |
| scope-and-spec       | Issue body and comment thread; linked/prior PR attempts and later changes mentioned in Repo facts or the thread.                                                                                | Fail for umbrella/tracking issues, pure support questions, unresolved design debates, or work explicitly requiring large-scale redesign. A long-running discussion with repeated abandoned or closed implementation attempts and no clearly settled expected behavior also fails. A brief issue body or missing reproduction steps does not fail by itself.
Otherwise, pass if the issue asks for one coherent, bounded piece of work and the expected behavior is settled enough to implement without making an unresolved product/design decision. A bounded piece of work may touch multiple related files/components or involve multiple technical substeps, as long as they all serve one clearly defined outcome.                                                                                     | required |
| available-to-work-on | Current assignees; linked PRs and their states; claim comments and dates; later progress, abandonment, and maintainer responses. When sidebar metadata and the thread conflict, use the thread. | Fail if there is a current assignee, an open PR actively implementing the issue, an active claim, or clear evidence that the requested work has already been completed or resolved.<br><br>A claim within 60 days counts as active unless explicitly abandoned. A claim older than 60 days blocks only when supported by continuing evidence such as an open PR, later progress, a current assignment, or maintainer confirmation. Closed-unmerged PRs and explicitly abandoned claims do not block the issue. | required |
| ai-policy-compatible | Contribution policy, dedicated AI-policy files, contributor instructions, and relevant templates or equivalent policy evidence.                                                                 | Fail only if the project explicitly prohibits the AI-assisted contribution workflow required for this course. Pass if no AI restriction is stated, or if AI use is allowed subject to disclosure, testing, personal understanding, or human-review requirements.                                                                                                                                                                                                                                               | required |

## Verdict rule

Accept only if every required check passes. 
Any required check graded `fail` or `unclear` causes `reject`.