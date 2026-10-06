# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

b6tang

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/67#issuecomment-6009829998
I traced the review creation path and found that `POST /reviews` passes both `profile_id` and the authenticated `user_id` into `create_review`, but `create_review` currently creates the review without checking that the profile belongs to that user.

My plan is to add that ownership check in the review service, keep the existing same-user creation path unchanged, and add regression coverage for both same-user and cross-user profile IDs. I’ll also re-run the cross-user `POST /reviews` case after the change to confirm it is rejected while same-user review creation still works.

---

## Your branch

**Branch**

fix/67-review-profile-ownership

**Evidence**

Before (Unit 2 reproduction):

Authenticated as:
`user1@example.com`

Target profile owner:
`user2@example.com`

Request:
`POST /reviews`

```json
{
  "profile_id": "90e79b04-93cb-475d-b454-5af97c5076ed"
}
```

Output:

```text
HTTP 200
```

```json
{
  "id": "c3e84755-4f03-45ea-ada6-dc5ba0134e1a",
  "profile_id": "90e79b04-93cb-475d-b454-5af97c5076ed",
  "status": "pending",
  "sections": null,
  "overall_score": null,
  "error_message": null,
  "created_at": "2026-09-29T19:44:21.994160Z",
  "updated_at": "2026-09-29T19:44:21.994164Z"
}
```

After (same cross-user reproduction against the built change):

Authenticated as:
`user1@example.com`

Target profile owner:
`user2@example.com`

Target profile ID:
`28098d8e-3bcb-4e61-af0c-c48bb87a9d84`

Request:
`POST /reviews`

```json
{
  "profile_id": "28098d8e-3bcb-4e61-af0c-c48bb87a9d84"
}
```

Output:

```text
HTTP 404
```

```json
{
  "detail": "Profile not found"
}
```

Same-user control:

Authenticated as:
`user1@example.com`

Request:
`POST /reviews`

```json
{
  "profile_id": "387f4d1d-9fb7-4f4d-88ba-73ecbe6d0aec"
}
```

Output:

```text
HTTP 200
```

```json
{
  "id": "89f736af-7d8b-45c6-b006-650a175d96d7",
  "profile_id": "387f4d1d-9fb7-4f4d-88ba-73ecbe6d0aec",
  "status": "pending",
  "sections": null,
  "overall_score": null,
  "error_message": null,
  "created_at": "2026-10-06T18:37:32.383556Z",
  "updated_at": "2026-10-06T18:37:32.383561Z"
}
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run:
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`

2. Partial re-run of `pkg-14` after revising `uncertainty-honest`:
   `agreement: 1/1 scored items`

3. Partial re-run of `pkg-01`, `pkg-04`, `pkg-06`, and `pkg-10` as canaries:
   `agreement: 4/4 scored items`

4. Confirming full run:
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

pkg-14: my final rubric decided `accept`, and the gold label was also `accept`.

An earlier version of my rubric rejected this package on `uncertainty-honest`. The plan gave a diagnosis based on the reproduced behavior, but the repro evidence did not directly prove every internal detail of that mechanism. I decided that was not a good reason to reject the plan. At the planning stage, a diagnosis can reasonably be an inference from the reproduced behavior; requiring the mechanism to already be directly proven would make the check too strict and would reject plans that are still well grounded enough to guide implementation.

I considered this a significant rubric problem, so I revised `uncertainty-honest` even though the previous full run had already reached 19/20.

**Check rationale**
> | uncertainty-honest | The plan's stated risks, assumptions, unknowns, and any recorded deviations, read against what the issue context and repro evidence actually establish. | Pass if any material unresolved question or assumption that could affect the plan's scope, implementation approach, or verification is identified as uncertainty rather than presented as established fact, and any known deviation from the plan is stated explicitly. If no such material uncertainty or deviation is present, the check may still pass. A diagnosis is not an unresolved assumption merely because its internal mechanism is inferred from repro evidence rather than directly proven. Fail if a material unresolved question or assumption that could affect the planned work is presented as certain, or a known deviation that affects the planned work is concealed. | required |

I revised this check after an earlier version rejected `pkg-14`. The earlier wording was too strict because it could treat a reasoned diagnosis as an unresolved assumption when the repro did not directly prove every internal detail.

I changed it to focus on whether material uncertainty is stated honestly, rather than requiring the diagnosis itself to be fully proven before implementation.


**Trade-offs**

I loosened `uncertainty-honest` so that a reasoned diagnosis is not rejected just because the repro does not directly prove every internal detail. The trade-off is that the rubric becomes more permissive toward inferred diagnoses, which could risk letting a weak assumption pass.

To check for that, I re-ran `pkg-01`, `pkg-04`, `pkg-06`, and `pkg-10` as canaries across the other reject categories. All four still matched the gold labels (`4/4`), and the confirming full run then reached `20/20`. This showed that the revision fixed the false reject on `pkg-14` without flipping those representative reject cases.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
