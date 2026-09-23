# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/67

**Verdict output**

All three candidates are in the scoped repo (`codepath/pathreview-ai301-fa26-s1`). Graded each independently against `rubric.md`, then ranked the accepted issues by the fit profile.

Shared repo-level evidence:

- Repo is not archived (`isArchived: false`) and was last pushed on `2026-09-16T21:48:27Z`, 6 days before the reference date. No releases exist.
- `Aburke225`, whose `author_association` on these issues is `COLLABORATOR`, authored three default-branch commits on 2026-09-16 (`f89c06f`, `2b8855f`, `32240e0`).
- No AI restriction is stated in `docs/CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md`, or the README.
- The repository's only pull request, #74, closes #60, so none of these three candidates has a linked PR, assignee, or comment.

Per the Path Review house rule, a classmate claim would not block any of these issues anyway; none currently has one.

Ranked read-out — all three accepted

1. #67 — Review creation does not verify profile ownership. Top fit: a backend authorization bug in `core/services/review_service.py`, with the intended behavior already demonstrated by `get_review()` and `list_reviews()` scoping through `Profile.user_id`. This matches the fit profile's strongest areas: APIs, backend logic, and debugging. The issue body is truncated mid-code-block, but the rubric explicitly says a brief body does not fail scope by itself, and the expected behavior is settled.

2. #32 — `DELETE /profiles/{profile_id}` leaves embeddings in the vector store. This spans `api/routes/profiles.py`, `core/services/profile_service.py`, and `rag/retriever/vector_store.py`, combining API work, backend data-lifecycle logic, and an ML-adjacent component without requiring ML depth. It is still one bounded outcome under the rubric.

3. #1 — Duplicate embeddings generated when re-ingesting the same repository. This is a debugging and performance issue involving a string passed to `db_session.query()` instead of the model class and swallowed by a broad `except`. It matches data-processing and debugging preferences, but has no API surface, making it the weakest fit of the three.

Per-check grades:

#67
- maintainer-alive: pass — `Aburke225` (`author_association: COLLABORATOR`) authored 3 default-branch commits on 2026-09-16, 6 days before 2026-09-22.
- repo-in-use: pass — `isArchived=false`; `pushedAt 2026-09-16T21:48:27Z`, within 6 months.
- scope-and-spec: pass — One bounded outcome: `core/services/review_service.py` ignores the ownership argument; expected behavior is settled by existing `get_review()`/`list_reviews()` scoping through `Profile.user_id`.
- available-to-work-on: pass — `assignees: []`; `comments: []`; timeline shows only two labeled events; PR #74 closes #60, not this issue.
- ai-policy-compatible: pass — No AI restriction found in `docs/CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md`, or README.

#32
- maintainer-alive: pass — Collaborator `Aburke225` made 3 default-branch commits on 2026-09-16 and renamed the issue the same day.
- repo-in-use: pass — `isArchived=false`; last push was 2026-09-16.
- scope-and-spec: pass — "Clearing those chunks should be part of the deletion" is one defined outcome across three named files, with no unresolved design decision.
- available-to-work-on: pass — `assignees: []`; no comments; no linked or mentioned PR.
- ai-policy-compatible: pass — No AI restriction in CONTRIBUTING, PR template, or README.

#1
- maintainer-alive: pass — Collaborator `Aburke225` authored commits `f89c06f`, `2b8855f`, and `32240e0` on 2026-09-16, plus a rename event on the issue that day.
- repo-in-use: pass — `isArchived=false`; `pushedAt 2026-09-16T21:48:27Z`, within 6 months.
- scope-and-spec: pass — `_check_skip()` passes the string `"IngestedSource"` to `db_session.query()` instead of the model class; this is one settled defect in two named files.
- available-to-work-on: pass — `assignees: []`; `comments: []`; no PR references the issue.
- ai-policy-compatible: pass — No stated AI restriction in the repo's contributor docs or templates.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/67",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Aburke225 (author_association COLLABORATOR on this issue) authored 3 default-branch commits on 2026-09-16, 6 days before 2026-09-22."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16T21:48:27Z, well within 6 months (no releases exist, but push activity satisfies the 'either' condition)."},
      {"name": "scope-and-spec", "grade": "pass", "evidence": "One bounded outcome: 'core/services/review_service.py ignores that argument'; expected behavior settled by existing get_review()/list_reviews() scoping through Profile.user_id."},
      {"name": "available-to-work-on", "grade": "pass", "evidence": "assignees: []; comments: []; timeline shows only two 'labeled' events; repo's sole open PR #74 closes #60, not this issue."},
      {"name": "ai-policy-compatible", "grade": "pass", "evidence": "No AI restriction found in docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md, or README; silence passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/32",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225 made 3 default-branch commits on 2026-09-16 and renamed this issue on 2026-09-16T21:49:37Z — 2+ qualifying activities inside 60 days."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; last push 2026-09-16, 6 days before the reference date."},
      {"name": "scope-and-spec", "grade": "pass", "evidence": "'Clearing those chunks should be part of the deletion' — one defined outcome across three named files; no unresolved design decision."},
      {"name": "available-to-work-on", "grade": "pass", "evidence": "assignees: []; no comments; timeline holds only label and rename events; no linked or mentioned PR."},
      {"name": "ai-policy-compatible", "grade": "pass", "evidence": "No AI-policy file and no AI restriction in CONTRIBUTING, PR template, or README."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225: commits f89c06f, 2b8855f, 32240e0 all dated 2026-09-16, plus a rename on this issue the same day."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16T21:48:27Z is within the last 6 months."},
      {"name": "scope-and-spec", "grade": "pass", "evidence": "'_check_skip() ... passes the string \"IngestedSource\" to db_session.query() instead of the model class' — a single settled defect in two named files."},
      {"name": "available-to-work-on", "grade": "pass", "evidence": "assignees: []; comments: []; timeline is four 'labeled' events only; no PR references this issue."},
      {"name": "ai-policy-compatible", "grade": "pass", "evidence": "No stated AI restriction anywhere in the repo's contributor docs or templates."}
    ],
    "verdict": "accept"
  }
]
```
---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

```text
python3 eval/run_eval.py --rubric "$HOME\.claude\skills\issue-select\rubric.md" --only issue-02,issue-03,issue-05,issue-12
agreement: 4/4 scored items

python3 eval/run_eval.py --rubric "$HOME\.claude\skills\issue-select\rubric.md" --only issue-01,issue-04,issue-06,issue-09,issue-14
agreement: 4/5 scored items

python3 eval/run_eval.py --rubric "$HOME\.claude\skills\issue-select\rubric.md" --only issue-01
agreement: 1/1 scored items

python3 eval/run_eval.py --rubric "$HOME\.claude\skills\issue-select\rubric.md" --save-run "..\ai301-coursework\beat-1-sandbox\unit-1\eval-run.txt"
agreement: 17/20 scored items  (bar: 18/20: below the bar)

python3 eval/run_eval.py --rubric "$HOME\.claude\skills\issue-select\rubric.md" --only issue-01,issue-15,issue-19
agreement: 2/3 scored items

python3 eval/run_eval.py --rubric "$HOME\.claude\skills\issue-select\rubric.md" --only issue-15
agreement: 1/1 scored items

python3 eval/run_eval.py --rubric "$HOME\.claude\skills\issue-select\rubric.md" --save-run "..\ai301-coursework\beat-1-sandbox\unit-1\eval-run.txt"
agreement: 19/20 scored items  (bar: 18/20: PASS)
```

**Issue analysis**

`issue-15`

Rubric decision: `reject`  
Gold label: `reject`

Final eval result:

> issue-15  reject  reject   yes

The gold label describes this issue as:

> "years of design debate and two abandoned PRs behind a friendly label"

My `scope-and-spec` check rejects a long-running discussion when there have been repeated abandoned or closed implementation attempts and the expected behavior is still not clearly settled. That is what caused my rubric to reject this issue. Because `scope-and-spec` is a required check, failing it makes the final verdict `reject`.

**Check rationale**

`scope-and-spec`

> Fail for umbrella/tracking issues, pure support questions, unresolved design debates, or work explicitly requiring large-scale redesign. A long-running discussion with repeated abandoned or closed implementation attempts and no clearly settled expected behavior also fails. A brief issue body or missing reproduction steps does not fail by itself.
>
> Otherwise, pass if the issue asks for one coherent, bounded piece of work and the expected behavior is settled enough to implement without making an unresolved product/design decision. A bounded piece of work may touch multiple related files/components or involve multiple technical substeps, as long as they all serve one clearly defined outcome.

I designed this check to separate a bounded implementation task from an issue that still requires major product or design decisions. During eval, I found that issue length, missing reproduction steps, or touching multiple files did not necessarily make an issue too broad. What mattered more was whether the issue had one clear outcome and whether the expected behavior was settled enough to start implementing.

**Trade-offs**

```
issue-19  accept  accept   yes
issue-19  accept  reject   NO     failed: scope-and-spec
```

I tightened `scope-and-spec` to reject cases like `issue-15`, which had been open for years, had repeated abandoned or closed implementation attempts, and still contained some uncertainty about the expected behavior. That made the check more cautious about issues whose scope may not be fully settled. However, `issue-19` shows the cost of that choice: it was accepted in my targeted run but rejected for `scope-and-spec` in the final full eval, even though its gold label is `accept`. In `issue-19`, the desired outcome is clear; the issue lists multiple technical causes and implementation suggestions rather than an unresolved product decision. The trade-off is that a stricter scope check can sometimes interpret implementation flexibility as unsettled scope and reject a valid bounded issue.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. #67 fits my interests because it is a backend authorization and service-logic bug rather than a documentation or test-only task. I also wanted to try a tier-2 issue instead of another very small tier-1 fix. The scope still looks bounded enough to be manageable within the course timeline.

2. The verdict correctly identified that the repository is active, the expected behavior is clear, and the issue is currently available to work on. The verdict also ranked #67 highest by my fit profile, which matches my preference for backend authorization and service-logic work while keeping the scope bounded enough for a first contribution.

3. I expect claiming #67 to be straightforward because it currently has no assignee, no comments, and no open PR showing that someone else is working on it.
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
