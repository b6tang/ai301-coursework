# Contribution: Facet counts on page 2 of a table leave out rows from page 1, and facet links keep `_next`

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** baowent
**GitHub Username:** b6tang
**Issue:** https://github.com/simonw/datasette/issues/3021
**Pull Request:** https://github.com/simonw/datasette/pull/3026

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and tested an issue-scout search strategy**

I created a search strategy using GitHub CLI queries for Python, Java, and C++ projects, with metadata-based ranking filters. My first scan evaluated five issues and accepted one, but I had concerns about its implementation complexity and the repository's support for external contributors. I then graded three issues from the Issue Selection site, and all were rejected because the repository prohibits LLM-sourced contributions. These results showed that repository contribution policies and active PRs must be checked before choosing an issue.

**Unit 6: Installed a voice guard and created repository memory for simonw/datasette**

I installed the user-level voice guard and wrote a voice guide that required my claim to name a concrete first step, avoid promising a fix, and distinguish confirmed observations from explanations still under investigation. My first rule required checkable implementation scope before reproduction, which made a short claim unnecessarily detailed, so I revised it to require only a concrete investigation step and a time for the next update. I then used the guard to check my claim for Datasette #3021 before posting it. I also created and locally excluded `CLAUDE.local.md`, where I recorded the scoped facet-pagination path, repository conventions, and exact test commands.

**Unit 7: Validated the Datasette fix with the ship-gate**

I installed the ship-gate and filled in `ship-manifest.md` with Datasette’s required Black and Ruff checks, while recording the full pytest suites separately with the reason they did not run cleanly on Windows. On `fix/3021-facet-pagination`, the gate passed both repository checks, and `pr-precheck` accepted the plan fidelity, test evidence, diff quality, and PR communication. Windows temporary-file cleanup failures made the local full-suite results difficult to interpret. I therefore kept the focused regression results separate and reran the full suites in WSL, where they completed without failures.

**Unit 8: Used the PR-monitor to choose a no-reply wait**

I installed the PR-monitor and wrote a playbook covering actionable review responses, waiting thresholds, nudge etiquette, and walk-away criteria. The monitor read PR #3026 as open and mergeable with one passing check, no maintainer decision, and a praise-only review from a non-maintainer. It drafted a no-reply wait note, which I reviewed and kept local because there was no actionable feedback and the 14-day nudge threshold had not been reached.

---

## Why I Chose This Issue

After another contributor opened a PR for an issue I had already claimed, I searched again using the GitHub Issues Search API. I searched for recent, unassigned Python and C++ issues from the previous 90 days, found Datasette #3021, and passed it to issue-scout for direct grading. Issue-scout returned an `accept` verdict: all five required checks passed, and it rated the difficulty as moderate.

This issue fits me because it is a focused Python bug involving backend request handling, SQL, and pagination. I wanted to learn how Datasette calculates facets across pages and how to turn a live reproduction into regression tests. Issue-scout was useful for grading a candidate I had already found, but its built-in scan had not surfaced this issue, and its rubric could not determine whether a repository treats a claim comment as reserving the work. My previous experience showed that another contributor can still open a PR after a claim, so next time I would check the repository's actual claiming convention and move from reproduction to implementation sooner.


Claim comment: https://github.com/simonw/datasette/issues/3021#issuecomment-6541316576

**Toolkit**

- Tool: issue-scout; I repeatedly ran fresh scans while revising its search strategy, then used live direct mode to grade [Datasette #3021](https://github.com/simonw/datasette/issues/3021) after finding it through a separate GitHub Issues Search API query.

- What it produced: The fresh scans produced several candidates that passed the rubric, but none was a good issue for me to claim. In direct mode, issue-scout accepted this issue with all required checks passing and reported a moderate difficulty level.

- Difficulties: The one-per-repository filter reduced repeated AI-policy rejections, but issue-scout could still accept active repositories with little evidence that they merge outside contributions.

- What changed: I applied the Unit 6 rubric patch, revised the queries and ranking filters, tested contribution-readiness labels, added a one-candidate-per-repository filter, and increased the grading count from 5 to 10. When those changes still did not produce a suitable issue, I used a broader GitHub API search and passed #3021 to issue-scout in direct mode.

---

## Understanding the Issue

**Problem Description**

Facet counts change after moving to another table page because pagination conditions are included in the facet query. Facet value links also retain `_next`, which skips valid matching rows after a value is selected.

**Expected Behavior**

Facet counts should describe the complete filtered result set on every page, and selecting a facet value should show all matching rows.

**Current Behavior**

On page 2, the `pk1` facet omits earlier values and counts `d` as 4 instead of 26. Selecting `d` keeps `_next` and displays only four matching rows.

**Affected Components**

- `datasette/views/table.py` constructs the SQL used for table rows and facets.
- `datasette/views/table_extras.py` passes that SQL and its parameters to facet classes.
- `datasette/facets.py` builds value links for column, array, and date facets.
- `tests/test_facets.py` contains the regression coverage.

**Toolkit**

- Tool: `plan-check`; I used it to grade `plan.md` and `plan-comment.md` against the issue.
- What it produced: It returned an accept verdict with all six checks passing and confirmed the SQL and facet-link paths described in the plan.
- Difficulties: None blocked the plan; it was accepted on the first run, although the grader offered optional clarifications.
- What changed: Nothing in response to `plan-check`.
---

## Reproduction Process

**Environment Setup**

I reproduced the issue on the official Datasette demo on 2026-10-31. This avoided depending on changes in my local working tree and provided a publicly accessible reproduction environment.

- **Public demo:** https://latest.datasette.io
- **Python:** 3.11.0
- **Datasette:** 1.0a41
- **SQLite:** 3.34.1
- **Commit:** `a8e13cc489b41f2f9a5aa691f5eb1bedf99a0e0b`
- **Local client:** Windows 11 with PowerShell

I confirmed the deployed version before reproducing the issue:
```powershell
Invoke-RestMethod 'https://latest.datasette.io/-/versions.json' |
    ConvertTo-Json -Depth 8
```

The main setup challenge was making sure the public demo still matched the version and commit named in the report. The version endpoint confirmed the same environment, so I could reproduce the behavior without installing a separate local Datasette instance or changing tracked files.

**Steps to Reproduce**

1. Open the `sortable` fixture table: https://latest.datasette.io/fixtures/sortable

2. Select `pk1` under **Suggested facets**. On page 1, the table reports 201
   rows and the facet box shows:
   ```text
   a=26, b=26, c=26, d=26, e=26, f=26, g=26, h=19
   ```

3. Click **Next page**. The resulting URL contains:

   ```text
   _facet=pk1&_next=d%2Cv
   ```

   The page heading still reports 201 rows, but the `pk1` facet box now shows:

   ```text
   e=26, f=26, g=26, h=19, d=4
   ```

   Values `a`, `b`, and `c` are missing, and `d` has changed from 26 to 4.

4. Click `d` in the page 2 facet box. The resulting URL retains both the
   pagination cursor and the new filter:

   ```text
   _facet=pk1&_next=d%2Cv&pk1=d
   ```

   The heading reports 26 matching rows, while the facet box and table show
   only the four matching rows after the cursor.

**Reproduction Evidence**

- **Commit showing reproduction:** None. I reproduced the bug on the official demo, which reported upstream commit [`a8e13cc489b41f2f9a5aa691f5eb1bedf99a0e0b`](https://github.com/simonw/datasette/commit/a8e13cc489b41f2f9a5aa691f5eb1bedf99a0e0b). I did not change any tracked files during reproduction.
- **Screenshots/logs:**  I recorded the browser observations below. The issue also includes a [BugReels recording](https://bugreels.com/sessions/bb921c22-5a15-4c80-a0a7-488f4baa3013/recording?t=66) showing the same reported behavior. My manual reproduction observations in official Datasette demo were:

  ```text
  `pk1` check: page 1
  heading: 201 rows
  pk1 facets: a=26, b=26, c=26, d=26, e=26, f=26, g=26, h=19
  URL: https://latest.datasette.io/fixtures/sortable?_facet=pk1#facet-pk1

  `pk1` check: page 2 - after clicking `Next page`
  heading: 201 rows
  pk1 facets: e=26, f=26, g=26, h=19, d=4
  missing: a, b, c
  URL: https://latest.datasette.io/fixtures/sortable?_facet=pk1&_next=d%2Cv

   `pk1` check: after selecting d on page 2
  heading: 26 rows where pk1 = "d"
  facet: d=4
  table rows shown: 4
  URL: https://latest.datasette.io/fixtures/sortable?_facet=pk1&_next=d%2Cv&pk1=d

  additional `pk2` check:
  page 1: a-s=8 each, t-z=7 each
  page 2 after d,v: most values=4, t/u/v=3
  page 3 after h,r: s=1 only
  heading on every page: 201 rows
  ```

- **My findings:**  I reproduced page-dependent facet counts with both `pk1` and `pk2`, showing that the problem was not limited to one column.

  In the `pk1` case, page 2 omitted `a`, `b`, and `c`, and counted `d` as 4 instead of 26. Selecting `d` displayed only 4 of the 26 matching rows.

  Separately, in the `pk2` case, the counts dropped from 7–8 per value on page 1 to 3–4 on page 2, and page 3 showed only `s=1`, while the heading remained at 201 rows. Reproducing the behavior with two columns showed that it was not limited to `pk1`; its implementation cause still required source investigation.

**Toolkit**

- Tool: repro-check; I used it in live mode to grade my posted claim together with the completed reproduction report for Datasette #3021.
- What it produced: It returned an accept verdict with all five checks passing and verified the recorded environment and behavior through the public demo's JSON endpoints.
- Difficulties: The issue already included a detailed reproduction and video, so I needed to distinguish my observations from the reporter's evidence and avoid treating the reported cause as something proven by my reproduction.
- What changed: No change was required to obtain the accepted verdict.

---

## Solution Approach

**Analysis**

`table_view_data()` creates `from_sql` and `from_sql_params` as snapshots of the filtered query before the `_next` cursor conditions are added. However, `sql_no_order_no_limit`, which was passed to the facets, was also built from
the post-pagination `where_clause`. `table_extras.py` likewise passed the later parameter set to the facet classes. As a result, facet counts depended
on the current page. Facet value links were also built from the current page URL without removing `_next`. The same pattern was used by the built-in column, array, and date facets.

**Proposed Solution**

Reuse the existing pre-pagination `from_sql` and `from_sql_params` before `_next` is applied. When building a link that selects or clears a facet value, remove `_next` but keep all other query arguments, including filters, sorting, and repeated arguments. Leave the paginated row query and whole-facet removal links unchanged.

**Implementation Plan**

1. Add regression tests in `tests/test_facets.py` for page-independent counts and value links without `_next`.
2. Update `table.py` and `table_extras.py` to pass `from_sql` and `from_sql_params` to facets.
3. Update the column, array, and date link paths in `facets.py`, then run the focused, related, formatting, lint, and full-suite checks.

The plan did not originally specify the order of the link-helper calls. Before applying that change, I found that removing a selected value first could
produce a path with no query string. A subsequent helper call could then fall
back to the original request arguments and restore the selected value. I therefore implemented the selected-value path by removing `_next` first and the selected value second, and added a guard test for that case.

**Toolkit**

- Tool: Claude Code; I used it to inspect the relevant code paths and draft
  the plan, then to generate the implementation changes after the plan was
  accepted.
- What it produced: It produced the draft changes across the four affected source and test files, which I reviewed and revised.
- Difficulties: The correct ordering of the two facet-link helper calls was not obvious because an empty intermediate query could cause the original arguments to be restored.
- What changed: I reversed the removal order and added a guard test for deselecting a metadata-configured facet value when it is the only query argument.

---

## Testing Strategy

I first ran the new focused tests while the production code was still unchanged. The ten issue-regression cases failed, while the existing-behavior guard test passed. This gave me a baseline showing both the reported bug and the behavior that the fix must not break.

After the implementation, all eleven focused cases passed. They verify that facet counts remain stable across plain, filtered, and sorted pagination; selecting `d` removes `_next` and returns all 26 matching rows; and column, array, and date facet links handle both selected and unselected values while preserving repeated arguments.

I also repeated the original browser steps against the fixed branch in WSL. The `pk1` counts remained `a–g=26` and `h=19` on pages 1, 2, and 3. Selecting `d` from page 2 removed `_next` and displayed all 26 rows from `d,a` through `d,z`, instead of only the four rows after the old cursor.

Black and Ruff passed. The complete WSL non-serial suite finished with 2,804 passed, 85 skipped, 6 xfailed, and 15 xpassed; the serial suite passed all 40 tests. On Windows, some related and full-suite tests encountered `WinError 32` temporary-file cleanup failures both before and after the change, so I did not describe those Windows runs as clean passes.

**Toolkit**

- Tool: `pytest`, the local Datasette server, and the ship-gate; I used them for the failing baseline, automated verification, manual browser check, and final repository checks.
- What it produced: The focused result changed from ten failures plus one passing guard case to eleven passing cases, the browser reproduction passed after the fix, and the Linux full suites completed without failures.
- Difficulties: I spent significant time deciding which cases covered the affected paths without adding arbitrary combinations, and Windows temporary-file cleanup errors made the full-suite output difficult to interpret.
- What changed: I settled on ten issue-regression cases: three count scenarios, one regression case reproducing the reported page-2 `d` link, and six selected/unselected cases across column, array, and date facets. I also added an eleventh guard test for the link-helper ordering and used WSL for a clean full-suite result.

---

## Implementation Notes

**2026-10-31**

I added the regression tests before changing the production code. Ten issue-regression cases failed, while the metadata-facet guard case passed. My initial approach considered rebuilding the facet SQL, but source review showed that `from_sql` and `from_sql_params` already contained the required pre-pagination query, so I reused those snapshots instead.

While implementing the facet-link change, I found that removing a selected value before `_next` could produce a path with no query string. The next helper call could then restore the original request arguments. I reversed that order and added an eleventh guard test. After the fix, all eleven focused cases passed.

**2026-11-02**

I reran the full test suites in WSL after Windows temporary-file cleanup errors made the local full-suite results difficult to interpret. The WSL non-serial suite completed with 2,804 passed, 85 skipped, 6 xfailed, and 15 xpassed, and all 40 serial tests passed. I also repeated the browser reproduction against the fixed branch: facet counts remained complete on later pages, and selecting `d` returned all 26 matching rows without `_next`.

**Toolkit**

- Tool: Claude Code; I used it for code navigation and initial drafts of the implementation and tests.
- What it produced: It produced draft changes in `table.py`, `table_extras.py`, `facets.py`, and `tests/test_facets.py`, which I reviewed against the issue and source code.
- Difficulties: The first approach did not take advantage of the existing pre-pagination SQL snapshots, and the selected-value link path had a non-obvious argument-restoration edge case.
- What changed: I reused `from_sql` and `from_sql_params`, removed `_next` before removing a selected value, and added a guard test for the helper-call ordering.

---

## Pull Request

**PR Link:** https://github.com/simonw/datasette/pull/3026

**PR Description:** The PR explains why pagination conditions affected facet counts and links, then describes the change to use the existing pre-pagination SQL and remove `_next` from column, array, and date facet value links. It includes the focused regression results, Black and Ruff checks, the Windows cleanup limitation, clean WSL full-suite results, and an AI-assistance disclosure. It uses `Fixes #3021` to link the issue.

I also linked a manually triggered GitHub Actions run on my fork. Its Test workflow passed on Python 3.11 through 3.15.

**Maintainer Feedback:**

- 2026-11-03: None yet. The PR remains open, and no Datasette maintainer has requested changes.

**Status:** Awaiting review

**Toolkit**

- Tool: `ship-gate`; I ran it manually on the final branch with `plan.md`, `test_evidence.md`, and `pr_draft.md` before opening the PR.
- What it produced: It passed Black and Ruff, then returned an accept verdict from `pr-precheck` for plan fidelity, test evidence, diff quality, and repository standards.
- Difficulties: Windows cleanup failures meant that I could not honestly describe the original local full-suite run as passing, and the gate noted that one explanation of the changed test totals was too broad.
- What changed: I separated the focused before-and-after evidence from the Windows failures, recorded the clean WSL results, and kept the limitation explicit in the PR description.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how Datasette implements keyset pagination with `_next` and how pagination conditions can accidentally leak into related SQL queries. I also learned to keep SQL and parameter snapshots together, preserve repeated query arguments when modifying URLs, and use focused regression tests to separate count errors from link behavior.

**Challenges Overcome**

The main challenge was tracing why the table heading remained correct while facet counts changed between pages. Comparing the count, row, and facet query paths showed that they were using SQL captured at different stages. I also found an edge case where calling the link helpers in the wrong order could restore removed arguments. Finally, Windows temporary-file cleanup failures made the full test results difficult to interpret, so I used focused before-and-after evidence and reran the complete suites in WSL.

**What I'd Do Differently Next Time**

After reproducing the issue, I would spend more time checking existing query snapshots and helper behavior before finalizing the implementation plan. That would have revealed earlier that Datasette already stored the pre-pagination SQL and that the link-helper call order required special care. I would also move the full-suite testing to WSL sooner once the Windows cleanup failures appeared.
---

## Resources Used

- [Datasette issue #3021](https://github.com/simonw/datasette/issues/3021) provided the original reproduction steps, expected behavior, and affected code paths, which I verified against the public demo and source.
- [Datasette contributing guide](https://github.com/simonw/datasette/blob/main/docs/contributing.rst) provided the repository’s development, testing, and formatting commands.
- [Datasette public demo](https://latest.datasette.io/fixtures/sortable) provided the fixture data used for the initial browser reproduction.
