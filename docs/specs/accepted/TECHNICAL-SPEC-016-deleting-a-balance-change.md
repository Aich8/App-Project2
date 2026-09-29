# TECHNICAL-SPEC-016: Deleting A Balance Change

Spec ID: `TECHNICAL-SPEC-016`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-005: Deleting A Balance Change`](FEATURE-SPEC-005-deleting-a-balance-change.md)

Covers: detecting the delete hold, coordinating the two deletion steps, and atomically removing one selected `Balance Changes` record.

Status: `Accepted`

## Temporary State

Keep three in-memory states:

- pending hold: record ID, complete TechnicalSpec 010 interaction context, accepted pointer ID, starting coordinates, monotonic start time, current timer ID, listener controller, and completion flag;
- delete actions: record ID and listener controller; and
- confirmation: record ID, `confirming`, and `saveFailureVisible`.

TechnicalSpec 010 represents the last two states with `balance-change-delete-actions` and `balance-change-delete-confirmation` descriptors under one action token. Install or clear each state and its descriptor together. TechnicalSpec 008 discards all three states and their resources during lifecycle or newer-information cleanup.

## Opening Delete Actions

Start a hold only for the primary touch contact or primary mouse button on a currently visible TechnicalSpec 002 row. Use Pointer Events and track only the accepted pointer ID. Capture the pointer when supported; otherwise, use document-level listeners. Listen for movement, release, pointer cancellation, lost pointer capture, and scrolling from every active dashboard or history scrolling source.

Use one monotonic clock and a `600ms` timer. If the callback runs early, replace the timer with one for the remaining time. Before completion, cancel on release, any change from the starting coordinates, pointer cancellation, lost capture, or scrolling. Ignore other pointer IDs. Cancellation clears the current timer, listeners, capture, and pending state without changing information or showing a message.

At or after `600ms`, verify that the same pointer is still held without movement or scrolling and that the record is still a visible current row. Request the delete-actions state and matching descriptor through TechnicalSpec 010 using the original interaction context. Mark the hold completed whether the request opens or is blocked.

After completion, movement and scrolling do not close the opened actions. Suppress the matching release and resulting click so they cannot start another action, then release capture and clear the hold resources. The opened state exposes the exact actions `Delete` and `Cancel`.

## Delete Steps And Back

The delete-actions controller owns its outside-interaction listeners and is aborted when that step closes, continues, or is discarded.

- `Cancel`, outside interaction, or Back clears the delete-actions state and matching coordinator entry without changing information.
- Movement and scrolling leave the delete actions open.
- `Delete` uses the TechnicalSpec 010 continuation operation to atomically replace the actions state and descriptor with a confirmation for the same record ID. The token is reused, its revision changes, and the confirmation starts with `confirming` and `saveFailureVisible` set to `false`. It exposes `Delete this Balance Change?` with the exact actions `Cancel` and `Delete`.

Every handler captures the action token, coordinator revision, and complete expected descriptor. A handler acts only while all three remain current, so an old actions handler cannot affect the confirmation.

Before exposing the delete actions, TechnicalSpec 017 creates one temporary Back step for the token. Confirmation reuses it. Browser, device, or system Back consumes it and clears the current deletion step through TechnicalSpec 010 while keeping the dashboard active. Every other close path removes it once through TechnicalSpec 017's internal operation; no new action or normal navigation starts until that removal finishes.

## Confirmation And Attempt

`Cancel`, outside interaction, or Back marks the confirmation's pending save request as no longer current, then clears the confirmation and matching coordinator entry without writing. The same outside interaction cannot open another action, and a late result cannot affect the closed confirmation.

Choosing confirmation `Delete` while `confirming` is `true` does nothing. Otherwise:

1. capture one current coordinator snapshot and one complete active TechnicalSpec 007 ready-state snapshot;
2. verify the token, revision, descriptor, and ready state;
3. capture one `attemptTimeMs` and confirm that the selected record is still present and visible;
4. stop silently if ownership or the ready state is stale;
5. close the confirmation without writing or showing a message if the target is missing or expired; and
6. for a current target, hide the old failure message, set `confirming` to `true`, and capture one attempt containing the verified action values, ready state, its stored-value baseline, record ID, time, and a fresh TechnicalSpec 009 request identity.

Capture the attempt without awaited work between these checks. Newer cross-tab information normally discards the action earlier through TechnicalSpecs 008 and 009.

## Candidate And Save

Inside a failure boundary, build a separate `StoredWebsiteDataV1` candidate from the attempt:

- keep the main money amount unchanged;
- remove exactly one record matching the selected ID;
- remove other records expired at `attemptTimeMs`;
- preserve all other history records in saved-list order; and
- preserve all `Saving` values in order, reusing the exact raw value of every broken slot.

Require exactly one selected-ID match. Do not mutate active information. Supply the loaded `Saving` slots so the TechnicalSpec 007 encoder can verify broken raw values. The deletion, expiration cleanup, and unchanged information form one complete candidate and create no new history record.

If candidate construction throws, cannot finish, or produces an invalid candidate, keep the selected entry and confirmation, reset `confirming`, show `Changes could not be saved.`, and register no save request or writer call.

Immediately before registering the save, verify the attempt's action values and ready state again. A mismatch stops silently. Otherwise, register its TechnicalSpec 009 `user-action` request and request a known-baseline save. The `confirming` guard and request registry prevent duplicate writes.

The result handler verifies the same token, revision, and descriptor before acting:

- `saved`: activate the complete candidate, use the returned serialized value as the new baseline, derive one ready state from the candidate, clear pending balance cleanup, clear the confirmation and matching coordinator entry together, then remove the Back step; show no message or undo action;
- `save-failed`: discard the candidate, keep the earlier ready state and selected entry, reset `confirming`, keep the confirmation open, and show the failure message; or
- `superseded` or silent invalidation: perform no deletion effect and let TechnicalSpecs 008 and 009 discard the old confirmation and load newer information.

Deletion never changes the main money amount or publishes a newest-entry scrolling ID.

## Failure Message And Retry

Keep `Changes could not be saved.` visible without a timer while the failed confirmation remains unchanged. A retry hides the old message while pending and captures a fresh ready state, baseline, time, and request identity. Another failure shows the message again; success closes the confirmation.

`Cancel`, outside interaction, Back, lifecycle discard, or newer cross-tab information removes the confirmation and message. A later confirmation starts without the old message. An expired target closes silently as required by accepted FeatureSpec 005.

## Essential Verification

Verify that:

- only an uninterrupted `600ms` primary touch or mouse hold on a current row can open delete actions, and every hold resource is cleaned up;
- the completed hold's release cannot start another action, while later movement and scrolling leave opened actions visible;
- each close path changes no saved information, and the actions-to-confirmation continuation keeps the same target safely;
- exactly one Back step exists for both deletion steps and is consumed or removed once;
- stale handlers, states, results, and missing or expired targets cannot delete another record;
- one confirmation attempt produces at most one writer call and removes exactly the selected record plus normally expired records;
- success preserves all other information, while candidate or save failure keeps the selected entry and follows the accepted message rules; and
- deletion creates no history entry, scrolling request, success message, or undo state.

## Boundaries

TechnicalSpec 002 owns history records and expiration. TechnicalSpecs 006 and 009 own writing and save coordination. TechnicalSpecs 008 and 010 own discard and one-action priority. TechnicalSpec 017 owns the temporary Back step.
