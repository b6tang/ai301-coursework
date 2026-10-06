# Procedure: how this skill grades a plan package

## Read order

1. In live mode, read `scope.md` first and confirm the issue is inside the scoped source; note any house rules. In eval mode, do not use `scope.md`.

2. Read `rubric.md` and `references/evidence-guide.md`. Note every check, the evidence each check names, its weight, and the verdict rule.

3. Read the whole plan package before grading any check.
   - Read the issue context first, including repo facts and relevant thread context.
   - Read the repro evidence next.
   - Read the candidate plan after the repro evidence.
   - Read the candidate plan comment last.

4. While reading, note the issue constraints, the reproduced behavior, the plan's diagnosis and proposed change, and what the plan comment promises.

Read the repro evidence before the plan so the reproduced behavior is established before judging whether the plan's diagnosis and approach are grounded in it. Read the comment after the plan so its promises can be checked against the work the plan actually contains.

## Evidence gathering

1. For each rubric check, gather exactly the evidence named in that check, using `references/evidence-guide.md` to locate it.

2. For `fix-rationale`, gather the plan's diagnosis and proposed approach, then compare them with the repro evidence's steps, inputs, observed behavior, and relevant artifacts.

3. For `scope-bounded`, gather the plan's in-scope work, out-of-scope work, and named files or areas, then compare them with the issue and repro evidence.

4. For `execution-ready`, gather the named files or areas, implementation approach, and order of work from the plan.

5. For `uncertainty-honest`, gather the stated risks, assumptions, unknowns, and deviations, then compare factual claims with what the issue and repro evidence actually establish.

6. For `verification-plan`, gather the proposed verification steps and expected result, then compare them with the reproduced steps, inputs, and failure.

7. For `comment-aligned`, gather the candidate plan comment and compare it with the candidate plan, relevant issue or thread direction, and applicable repo-facts requirements.

8. In live mode, gather issue-side and repository evidence from the locations named by the evidence guide. In eval mode, use only the package bundle and quote the relevant package text.

## Check execution

1. Grade the checks in the order they appear in `rubric.md`.

2. For each check, apply only that check's stated evidence and pass/fail condition.

3. Grade the check:
   - `pass` when the gathered evidence satisfies the pass condition.
   - `fail` when the gathered evidence satisfies a stated fail condition.
   - `unclear` when evidence needed to decide the check is genuinely absent or insufficient.

4. Record one short evidence fact or quote that explains the grade. Do not use `unclear` merely because the evidence was not gathered yet.

5. After the package has been read and the evidence gathered, do not re-read the entire package for every check. Return only to the source locations named for that check when the recorded evidence is insufficient or conflicting.

6. In live mode, after grading the rubric checks, read `voice-guide.md` and check the draft plan comment against it. Report any broken voice rule, but do not change the rubric verdict unless a rubric check itself uses that evidence.

## Verdict assembly

1. Wait until every rubric check has a grade.

2. Apply the verdict rule in `rubric.md` exactly as written.

3. With the current rubric, return `accept` only if every required check passes. Return `reject` if any required check fails or is unclear. Preferred checks do not change the verdict.

4. If the verdict is `reject`, use the first required check in rubric order graded `fail` or `unclear` as the deciding check.

5. For a deciding `fail`, quote the shortest relevant submission text that shows why the check failed. For a deciding `unclear`, state what evidence is missing and quote nearby submission text only when it helps show why the available evidence is insufficient.

6. Produce the final result using the output format defined in `SKILL.md`. The final verdict is only `accept` or `reject`; there is no third verdict.