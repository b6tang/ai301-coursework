# Plan

## Diagnosis

`POST /reviews` accepts a `profile_id` supplied by the authenticated user and delegates review creation to `core/services/review_service.py:create_review`.

The service already receives both `profile_id` and `user_id`, but it currently creates the `Review` directly from the supplied `profile_id` without loading the corresponding `Profile` or checking that `Profile.user_id` matches the authenticated user. This allows one user to create a review for another user's profile, matching the Unit 2 reproduction.

Existing `get_review` and `list_reviews` logic already restrict access through `Profile.user_id == user_id`, so the missing ownership validation is specific to the review-creation path.

## Scope

In scope:
- Add profile ownership validation to `core/services/review_service.py:create_review` before a `Review` is created.
- Require the supplied profile to belong to the authenticated `user_id`.
- Add regression coverage for valid same-user creation and rejected cross-user creation in `tests/unit/test_review_service.py`.
- Re-run the Unit 2 `POST /reviews` reproduction through the API after the change.

Out of scope:
- Changes to authentication or profile creation.
- Broader authorization refactors outside review creation.
- Changes to `ReviewCreate`, since it already validates that `profile_id` is a UUID.
- Changes to the existing ownership behavior of `get_review` or `list_reviews`.
- The Chroma / NumPy startup incompatibility observed during reproduction.

## Files or areas

- `core/services/review_service.py`
  - Update `create_review(db, profile_id, user_id)` to verify that the supplied profile belongs to `user_id` before constructing and committing a review.

- `tests/unit/test_review_service.py`
  - Add regression tests alongside the existing `create_review` tests for same-user and cross-user profile ownership.

- `api/routes/reviews.py`
  - `create_review_endpoint` is the caller of `create_review`. No route change is planned unless the service's rejection needs to be mapped using an existing API error pattern.

## Approach

1. In `create_review`, query for the supplied profile using both its profile ID and the authenticated `user_id`.

2. If no matching profile exists, stop before constructing or committing a `Review` and reject the request using the repository's existing error-handling convention for an inaccessible or invalid resource.

3. If the profile belongs to the authenticated user, preserve the current creation path: create the review with status `pending`, add it to the database, commit, and refresh it.

4. Add a regression test where the supplied profile belongs to the authenticated user and confirm review creation still succeeds.

5. Add a cross-user regression test where the supplied profile belongs to another user and confirm that review creation is rejected and no review is committed.

## Test plan

Unit tests:

- Run the relevant tests in `tests/unit/test_review_service.py`.
- Confirm a user can still create a review for their own profile.
- Confirm a user cannot create a review for another user's profile.
- Confirm the rejected case does not commit a new review.

API reproduction:

1. Start the backing services and application using the same setup from Unit 2.
2. Authenticate as user2 and create a profile.
3. Authenticate as user1.
4. Send `POST /reviews` using user2's `profile_id`.

Expected after the change:
- The cross-user request is rejected instead of returning HTTP 200.
- No pending review is created for user2's profile by user1.

Control:
- Send `POST /reviews` using a profile belonging to the authenticated user.
- The request should continue to create a pending review successfully.

## Risks and unknowns

- The code inspection establishes where the ownership check is missing, but it does not yet establish which existing HTTP status and error body the repository uses for this exact failure case. The implementation should follow the repository's existing error-handling convention rather than introduce a new response shape.
- The ownership check adds a profile lookup to review creation. The change should stay limited to that validation rather than refactoring the broader review service.
- `api/routes/reviews.py` should not need structural changes unless the existing service-to-route error handling requires an explicit mapping.


## Deviations

No material deviation from the posted plan.
- `core/services/review_service.py` now performs the planned ownership check before creating a review.
- `api/routes/reviews.py` uses the conditional route handling anticipated in the plan and returns `404 "Profile not found"` when ownership validation fails.
- Regression coverage stayed in `tests/unit/test_review_service.py`: the cross-user rejection case was added as a new test, while the existing success test was extended to cover same-user ownership. 
- The API repro also matched the plan: cross-user repro changed from `200` to `404`, while same-user remained `200` with status `pending`.
