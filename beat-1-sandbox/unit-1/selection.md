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

All three URLs are in the scoped repo (codepath/pathreview-ai301-fa26-s1). Graded each independently against rubric.md, then ranked the accepted ones by the fit profile.

Shared repo evidence

- Maintainer: Aburke225 is author_association: COLLABORATOR and pushed 3 default-branch commits on 2026-09-16 (f89c06f, 2b8855f, 32240e0) — 4 days before today. Well past the 2-activities-in-60-days bar.
- Repo: isArchived: false, pushedAt: 2026-09-16. No releases exist, but push activity is inside 6 months.
- AI policy: docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md, the four issue templates, and README contain no AI restriction. Silence passes. (CONTRIBUTING does impose green CI and xfail-marker removal — terms, not a ban.)
- Availability: the repo has zero pull requests of any state, and all three issues have zero comments, zero assignees, and no cross-references in their timelines. Nothing blocks any of them. The Path Review house rule on classmate claims never even comes into play.

Ranked read-out — all three accepted

1. #32 — DELETE /profiles/{profile_id} leaves embeddings in the vector store. Best fit: it spans the API route, the service layer, and the RAG vector store, so it is backend logic + data processing + an ML-adjacent component that needs no ML knowledge — three of your stated preferences and two growth areas in one bounded fix.

2. #67 — Review creation does not verify profile ownership. Clean backend authorization fix in review_service.py, with the correct behavior already pinned by the existing get_review()/list_reviews() scoping. Slightly narrower than #32 (mostly one service file), so second.

3. #1 — Duplicate embeddings on re-ingest. Real debugging/performance work in the ingestion pipeline, but the issue body already names the exact root cause (db_session.query("IngestedSource") as a string, masked by a broad except), leaving the least to figure out.

Per-check grades:

┌──────────────────────┬──────┬──────┬──────┐
│        Check         │ #32  │ #67  │  #1  │
├──────────────────────┼──────┼──────┼──────┤
│ maintainer-alive     │ pass │ pass │ pass │
├──────────────────────┼──────┼──────┼──────┤
│ repo-in-use          │ pass │ pass │ pass │
├──────────────────────┼──────┼──────┼──────┤
│ scope-and-spec       │ pass │ pass │ pass │
├──────────────────────┼──────┼──────┼──────┤
│ available-to-work-on │ pass │ pass │ pass │
├──────────────────────┼──────┼──────┼──────┤
│ ai-policy-compatible │ pass │ pass │ pass │
└──────────────────────┴──────┴──────┴──────┘

One note on where the rubric and the issues sit a little oddlyed effort: 4–6 hours" / tier-2 label rather than agood-first-issue label, and the rubric has no difficulty-tier check, so tier plays no part in these verdicts. If you want tier to matter, that belongs in
rubric.md, not in this run.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence"hored 3 default-branch commits on 2026-09-16 (f89c06f, 2b8855f, 32240e0), within 60 days."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "is6-09-16, within 6 months."},
      {"name": "scope-and-spec", "grade": "pass", "evidence": "One bounded outcome: 'Clearing those chunks should be part of the deletion' across 3 named
related files; no open design question."},
      {"name": "available-to-work-on", "grade": "pass", "evidence": "assignees: []; 0 comments; repo has zero PRs of any state; timeline shows only label
and rename events."},
      {"name": "ai-policy-compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, PR template, issue templates and README state no AI
restriction."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/67",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225 authored 3 default-branch commits on 2026-09-16, within 60
days."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived: false; pushedAt 2026-09-16, within 6 months."},
      {"name": "scope-and-spec", "grade": "pass", "evidence": eate_review through Profile.user_id, 'consistent withget_review() and list_reviews()'."},
      {"name": "available-to-work-on", "grade": "pass", "evidents; no PRs exist in the repo; timeline shows only two labelevents."},
      {"name": "ai-policy-compatible", "grade": "pass", "evideONTRIBUTING, PR template, issue templates or README."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225 authored 3 default-branch commits on 2026-09-16, within 60
days."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived: false; pushedAt 2026-09-16, within 6 months."},
      {"name": "scope-and-spec", "grade": "pass", "evidence": d: _check_skip() 'passes the string \"IngestedSource\" todb_session.query() instead of the model class'."},
      {"name": "available-to-work-on", "grade": "pass", "evidents; no PRs exist in the repo; timeline shows only four labelevents."},
      {"name": "ai-policy-compatible", "grade": "pass", "evideONTRIBUTING, PR template, issue templates or README."}
    ],
    "verdict": "accept"
  }
]

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

2. The verdict correctly identified that the repository is active, the expected behavior is clear, and the issue is currently available to work on. Even though #32 ranked slightly higher by my fit profile, #67 feels like a better balance between being challenging enough to learn from and still being narrow enough for a first contribution.

3. I expect claiming #67 to be straightforward because it currently has no assignee, no comments, and no open PR showing that someone else is working on it.
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
