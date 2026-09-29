# TECHNICAL-SPEC-021: Deleting A Saving

Spec ID: `TECHNICAL-SPEC-021`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-011: Deleting A Saving`](FEATURE-SPEC-011-deleting-a-saving.md)

Covers: confirmation state, normal and broken target identity, complete deletion-candidate creation, and coordinated save results.

Status: `Accepted`

## Purpose

Remove exactly one selected normal or broken `Saving` after confirmation without changing the main money amount or creating a `Balance Changes` entry.

## Confirmation State

```ts
type DeleteSavingConfirmationState = {
  target: SavingActionTarget;
  confirming: boolean;
  saveFailureVisible: boolean;
  listenerController: AbortController;
};
```

`SavingActionTarget` is the TechnicalSpec 010 union containing either a stable normal `Saving` ID or a broken slot's in-memory `runtimeKey`.

A normal target enters this state through the TechnicalSpec 018 continuation. TechnicalSpec 018 prepares the complete state and outside-interaction handling, creates the TechnicalSpec 017 temporary Back step, and then publishes the state with the exact `saving-delete-confirmation` descriptor under the existing TechnicalSpec 010 token.

A broken target starts directly from its TechnicalSpec 005 `Delete` action. Use one TechnicalSpec 010 interaction context and one coherent controller and ready-state snapshot. Continue only while `Savings` is current, TechnicalSpec 009 is not updating, TechnicalSpec 017 has no active history operation, the exact `runtimeKey` still resolves to one broken slot, the interaction began with no action and remains unclaimed, and the coordinator is still empty at the captured revision.

Prepare the broken-target state, descriptor, listener controller, and handlers without exposing them. Follow the TechnicalSpec 010 direct-start checks and TechnicalSpec 017 pre-publication Back-step rule as one guarded transition: expose the confirmation only after its temporary Back step succeeds. A marker failure aborts the prepared controller and leaves the coordinator empty. A publication failure after the marker was created uses TechnicalSpec 017's rollback rule. Every failed start changes and saves nothing and shows no message.

Both paths start with `confirming` and `saveFailureVisible` set to `false`, show `Delete this Saving?`, and provide the exact actions `Cancel` and `Delete`. The state and its listeners exist only in memory.

## Interaction And Closing

Every handler captures and checks the current action token, coordinator revision, complete `saving-delete-confirmation` descriptor, and target. A stale handler stops silently.

Only a completed click or tap outside the confirmation closes it. An initial press, pointer movement, wheel event, or scroll event alone does not close it. An interaction inside the confirmation is not outside interaction. TechnicalSpec 010 prevents the same interaction from closing this confirmation and starting another action.

`Cancel`, an accepted outside interaction, or either Back action remains available while a save request is pending. Every close first marks that request as no longer current, then clears the confirmation state, listener controller, handlers, and matching TechnicalSpec 010 entry together.

- Browser/device Back consumes the temporary Back step and keeps `Savings` active.
- `Cancel`, an outside interaction, the website's Back action, an unavailable target, and successful deletion remove the temporary Back step exactly once through TechnicalSpec 017's internal operation.
- Lifecycle discard and newer saved information use TechnicalSpecs 008, 009, and 017 to discard the flow and safely normalize its Back step.

No later result from an invalidated request can change the closed confirmation. TechnicalSpec 009 governs a writer call that already finished before invalidation. Every close that did not follow a successful deletion changes no active information and shows no message.

## One Delete Attempt

Choosing `Delete` while `confirming` is already `true` does nothing. Otherwise:

1. capture one current coordinator snapshot and one complete active TechnicalSpec 007 ready-state snapshot;
2. verify that `Savings` is current, TechnicalSpec 009 is not updating, TechnicalSpec 017 has no active history operation, and the action token, coordinator revision, complete descriptor, target, and ready state are current;
3. resolve a normal target to exactly one normal slot with the same stable ID, or a broken target to exactly one broken slot with the same `runtimeKey` symbol, in that ready-state snapshot;
4. close the stale confirmation through the normal closing operation without a write or message if the target is unavailable;
5. hide the previous failure message, set `confirming` to `true`, and capture one `attemptTimeMs`; and
6. create one attempt containing the captured action values, ready-state snapshot, its exact stored-value baseline, resolved target slot and `sourceIndex`, time, and a fresh TechnicalSpec 009 request identity.

Perform these checks and captures without awaited work. The resolved `sourceIndex` identifies the matching item only in the captured ready snapshot's stored `savings` list. A `runtimeKey` from an older load can never identify a slot in a newer load.

## Complete Candidate

Inside one failure boundary, build a separate `StoredWebsiteDataV1` candidate only from the attempt's captured ready-state snapshot:

- remove the single `savings` item at the resolved `sourceIndex`;
- preserve every remaining `Saving` value in saved-list order, reusing every remaining broken slot's exact `rawValue`;
- keep every remaining stored `order` unchanged;
- keep the main money amount unchanged; and
- remove only `Balance Changes` records expired at `attemptTimeMs`, preserving every other history record in its current order.

Require exactly one resolved target before requesting a save. Do not mutate the active document or its arrays. Pass the loaded slots from the same captured ready-state snapshot with the candidate so the TechnicalSpec 007 encoder can verify every retained broken raw value.

If time capture or candidate construction throws, cannot finish, or produces an invalid candidate before registration, register no save request and call no writer. Keep the selected `Saving`, confirmation, and old active information, reset `confirming`, and show `Changes could not be saved.`.

After registration, the shared TechnicalSpec 007 encoder performs its complete candidate check. If it rejects the candidate, `setItem` is not called and the coordinated request returns `save-failed`.

## Coordinated Save And Result

Immediately before registering the save, confirm again that the attempt's token, revision, descriptor, target, ready-state snapshot, and `confirming` state are current. A mismatch stops silently. Otherwise, register one TechnicalSpec 009 `user-action` request using the known baseline from that same snapshot. The `confirming` flag and request registry prevent duplicate writes.

The active document and visible `Savings` information remain unchanged while the request is pending. The result handler acts only while the same token, revision, descriptor, target, and request identity still own the current deletion attempt.

- `saved`: first build one complete TechnicalSpec 007 ready-state snapshot from the candidate and returned baseline. Run the complete remaining saved `Saving` list through TechnicalSpecs 005 and 007 again, set pending history cleanup to `false`, and rebuild TechnicalSpec 004 coverage. An unchanged record becomes normal only when it now passes every validation rule; otherwise it remains broken. Only after all saved and derived values are ready, replace the old ready state with the new document, baseline, loaded lists, cleanup state, and coverage together. Then clear the confirmation state, handlers, and matching TechnicalSpec 010 entry together, remove the temporary Back step through TechnicalSpec 017, and show no message or undo action.
- `save-failed`: discard the candidate, keep the selected `Saving` and all other active information unchanged, reset `confirming`, keep the confirmation open, and show `Changes could not be saved.`.
- `superseded` or silent invalidation: run no deletion success or failure effect; TechnicalSpecs 008 and 009 discard the old confirmation and load the newer information.

Deletion creates no history record or newest-entry scrolling request. A successful complete save also completes any already-pending silent history cleanup contained in its candidate.

Every retry captures a fresh coordinator and ready-state snapshot, target slot, baseline, time, candidate, and request identity. A late result whose ownership checks fail does nothing.

## Failure Message Lifetime

Keep `Changes could not be saved.` visible without a timer while the failed confirmation remains unchanged. A retry hides the old message while the new attempt runs. Another failure shows it again, and success closes the confirmation.

`Cancel`, an accepted outside interaction, either Back action, lifecycle discard, or newer saved information destroys the confirmation and message. A later delete confirmation starts with `saveFailureVisible` set to `false`.

## Essential Verification

Verify that:

- normal IDs and broken runtime symbols each resolve only their exact current target;
- deleting a normal target removes the one stored item belonging to its stable ID and no other item;
- deleting a broken target removes the one stored item belonging to its current runtime symbol, preserves every other broken raw value by exact `Object.is` identity, and never serializes any runtime symbol;
- a broken target opens only after every direct-start check and temporary Back-step operation succeeds;
- a stale target closes without a write or message;
- only a completed outside click or tap closes the confirmation, and the same interaction cannot start another action;
- `Cancel`, outside interaction, and Back invalidate a pending request and close the confirmation without changing active information;
- closing while a request is waiting aborts it before any writer call when possible, and a request invalidated after its synchronous writer turn cannot publish a late success, failure, or message into the closed flow;
- every close clears handlers and handles the temporary Back step exactly once;
- one attempt uses one coherent ready-state snapshot, its matching target and baseline, and one fresh request identity;
- repeated `Delete` choices while pending create one request and at most one writer call;
- a successful candidate removes exactly the selected source item plus normally expired history;
- every remaining `Saving` value and order is preserved exactly;
- candidate-construction failure registers no request, calls no writer, and follows the save-failure behavior;
- encoder rejection calls no `setItem` and returns `save-failed`;
- success builds and installs one complete ready state, rechecks every remaining `Saving`, recalculates `Savings`, closes the confirmation, and creates no message, history entry, scrolling request, or undo state;
- deleting the earlier reserving record in a duplicate-ID, duplicate-name, or duplicate-order pair makes the later unchanged record normal only when complete revalidation finds that all of its fields are now valid;
- deleting a record that does not resolve every invalid field leaves each still-invalid unchanged record broken after complete revalidation;
- failure retains the target and confirmation and changes no active information;
- one failed deletion, a retry, a second failure, and a successful retry respectively show, hide, show, and finally remove the failure message while retaining the same selected target until success;
- retry clears the old failure message, another failure restores it, and every close removes it;
- stale handlers and late results cannot affect a closed, replaced, or newer deletion flow;
- success and every non-navigation close leave the primary view inside `Savings`, while TechnicalSpec 017 removes or consumes the temporary Back step exactly once; and
- a remaining record appears normally after success only when complete-list rechecking finds it valid.

## Boundaries

TechnicalSpec 004 owns coverage calculations. TechnicalSpec 005 owns broken slot identity and read-model access to deletion. TechnicalSpecs 006 and 007 own encoding, atomic writing, complete ready-state rebuilding, and complete-list classification. TechnicalSpec 009 owns save coordination. TechnicalSpecs 008 and 010 own lifecycle discard and one-action priority. TechnicalSpec 017 owns the temporary Back step and its cleanup. TechnicalSpec 018 owns the normal-target transition into confirmation; this spec owns the broken-target direct start.
