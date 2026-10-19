# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

1. In live mode, read `scope.md` first and confirm the issue is inside the scoped source; note any house rules. In eval mode, do not use `scope.md`.

2. Read `rubric.md` and `references/evidence-guide.md`. Note every check, the evidence each check names, its pass condition, its weight, and the verdict rule.

3. Read the whole PR package before grading any check.
   - Read the issue context first, including repository requirements and relevant thread direction. In eval mode, use the issue context and repo-facts block in the bundle. In live mode, use the issue thread, repository PR template, CONTRIBUTING instructions, and stated policy.
   - Read the repro evidence next, including its steps, inputs, observed behavior, and supporting artifacts.
   - Read the accepted plan after the repro evidence, including its scope, boundary, named files or areas, test plan, and recorded deviation notes.
   - Read the candidate PR's commit list and complete diff next. In live mode, use the branch's commit list and branch diff.
   - Read the candidate PR's test evidence next, including before-and-after observations and repository-check outcomes. In live mode, use the captured test outputs and artifacts.
   - Read the candidate PR title and description last. In live mode, use the draft title and description.

4. While reading, note the issue constraints, reproduced behavior, planned scope, recorded deviations, actual changed files and hunks, test outcomes, description claims, and applicable repository or maintainer requirements.

Read the plan before the diff so the intended boundary is established before comparing it with the actual changes. Read the description after the diff and test evidence so its claims can be checked against what the PR delivers and what the evidence supports.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

1. For each rubric check, gather exactly the evidence named in that check, using `references/evidence-guide.md` to locate it.

2. For `plan-fidelity`, gather the plan's scope, boundary, named files or areas, test plan, and recorded deviations. Compare the material changed files and hunks with the planned work and deviation notes, then compare the description's claims with what the diff actually delivers. Record any extra work, omitted work, disclosed limitation, or contradiction.

3. For `test-evidence`, gather the plan's test plan and the reproduction's steps, inputs, and observed failure. Compare them with the PR's before-and-after evidence and stated expected-after result. Record which relevant behavior was exercised, what happened, and which repository checks were required, run, and supported by identifiable check commands or names and their specific recorded outcomes. Compare the description's verification claims with those recorded outcomes.

4. For `diff-quality`, gather the unified diff and commit list. Identify the intended fix and record any unrelated edits or drive-by changes. Also record debug leftovers, dead code, commented-out blocks, or formatting churn that materially obscures the change.

5. For `standards-and-comms`, gather the applicable PR-template asks, contributing instructions, stated policies, AI-use disclosure requirements, and explicit maintainer direction. Compare each applicable requirement with the corresponding content in the PR title and description, and record any missing or ignored ask.

6. In live mode, gather working-copy, branch, test-output, issue-side, and repository evidence from the locations named by the evidence guide. In eval mode, use only the package bundle and quote the relevant package text.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade the checks in the order they appear in `rubric.md`.

2. For each check, apply only that check's stated evidence and pass/fail condition.

3. Grade the check:
   - `pass` when the gathered evidence satisfies the pass condition.
   - `fail` when the gathered evidence satisfies a stated fail condition.
   - `unclear` when evidence needed to decide the check is genuinely absent or insufficient.

4. Record one short evidence fact or quote that explains the grade. Do not use `unclear` merely because the evidence was not gathered yet.

5. After the package has been read and the evidence gathered, do not re-read the entire package for every check. Return only to the source locations named for that check when the recorded evidence is insufficient or conflicting.

6. In live mode, after grading the rubric checks, read `voice-guide.md` and check the draft PR title and description against it. Report any broken voice rule, but do not change the rubric verdict unless a rubric check itself uses that evidence.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Wait until every rubric check has a grade.

2. Apply the verdict rule in `rubric.md` exactly as written.

3. With the current rubric, return `accept` only if every required check passes. Return `reject` if any required check fails or is unclear. Preferred checks do not change the verdict.

4. If the verdict is `reject`, use the first required check in rubric order graded `fail` or `unclear` as the deciding check.

5. For a deciding `fail`, quote the shortest relevant submission text or diff excerpt that shows why the check failed. For a deciding `unclear`, state what evidence is missing and quote nearby submission text only when it helps show why the available evidence is insufficient.

6. Produce the final result using the output format defined in `SKILL.md`. The final verdict is only `accept` or `reject`; there is no third verdict.
