# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

b6tang

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/67#issuecomment-5889015273

I’m taking a look at #67. I’ll try the reported POST /reviews case with another user’s profile_id and post back with the setup, steps, and what I find.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/67#issuecomment-5890883113

I reproduced #67 on commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`.

Environment:
- Windows 11 Home 25h 
- git version 2.55.0
- Python 3.14.7
- Docker 29.6.1
- Docker Compose v5.3.0
- PostgreSQL and Redis containers healthy
- Working tree clean

Steps:
1. Started the backing services with `docker compose up -d`, ran `make setup`, and started the application with `make run`.
2. Authenticated as `user2@example.com`.
3. Created a profile for user2. The profile ID was:
   90e79b04-93cb-475d-b454-5af97c5076ed
4. Authenticated as `user1@example.com`.
5. While authenticated as user1, sent **POST /reviews** with user2's profile ID:
```
   {
     "profile_id": "90e79b04-93cb-475d-b454-5af97c5076ed"
   }
```
Expected:
The request should be rejected because the profile belongs to a different user.

Actual:
The request returned **HTTP 200** and created a review with status "pending":
```
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
This reproduces the reported behavior: user1 was able to create a review using a profile belonging to user2.

One environment note: the Chroma vector-db container exited during startup because Chroma 0.4.22 encountered an incompatibility with NumPy 2.x (`np.float_` was removed). The API still started successfully, and this did not prevent POST /reviews from returning the HTTP 200 response above.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

- Run 1: 19/20 scored items (bar: 18/20: PASS)
- Run 2: 20/20 scored items (bar: 18/20: PASS)

**Package analysis**

I analyzed `pkg-10`. In my first full run, my rubric decided `reject`, while the gold label was `accept`.

The run said:

> "Prompt renders `monorepo/packages/app-dir on master` and `starship explain` lists the module, the opposite of the issue's reported blank/omitted module."

At the same time, `outcome-honest` said:

> "Stated cannot-reproduce conclusion matches the artifacts, with explicit reasoning tying the gap to untested fish PWD behavior."

My original `behavior-matches-issue` check treated the absence of the reported failure as a reason to reject, even when the package directly tested the target and honestly reported that it could not reproduce it. That made the check too strict for well-evidenced negative results, so I revised it to judge whether the artifacts directly bear on the target behavior rather than requiring the reported failure itself to appear.

**Check rationale**

My current `behavior-evidenced` check is:

```text
| behavior-evidenced | Use the Behavior shown sources in `references/evidence-guide.md`: the repro report's output excerpts, logs, screenshots, or other artifacts read against the behavior described in the issue. | Pass if the artifacts bear directly on the issue's target behavior and make the observed result checkable. A reproduced result can pass when the reported behavior is shown. An evidenced cannot-reproduce can also pass when the target behavior does not occur, or when a necessary trigger or precondition cannot be achieved, as long as that limitation is explicitly identified and supported by the artifacts. Fail if the artifacts show a different failure instead of the issue's reported behavior, and the package claims that the issue was reproduced. | required |
```

I revised this check after the original version rejected honest cannot-reproduce packages such as `pkg-10`. After that first revision, I re-ran `pkg-09` and `pkg-10` with `--only`. `pkg-10` now passed, but `pkg-09` became a new edge case:

> "the differential arg-size-limit trigger scenario 2 depends on was never actually achieved, so the artifact tests an adjacent condition, not the target behavior"

That package still honestly documented a failed attempt to achieve the necessary trigger. I therefore made the final rule allow an evidenced cannot-reproduce when a necessary trigger or precondition cannot be achieved, but only when that limitation is explicitly identified and supported by artifacts. I kept the explicit failure case for packages that show a different failure and nevertheless claim the issue was reproduced.


**Trade-offs**

`behavior-evidenced` gives up some strictness by allowing a well-supported cannot-reproduce to pass instead of requiring the reported failure itself to appear. After the first revision, I re-ran `pkg-09` and `pkg-10` with `--only`: `pkg-09` was `reject` with gold `accept`, while `pkg-10` was `accept` with gold `accept`. I revised the check again to allow a cannot-reproduce when a necessary trigger or precondition could not be achieved but the limitation was clearly documented. In the final full run, both `pkg-09` and `pkg-10` matched gold, and the overall agreement was `20/20`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
