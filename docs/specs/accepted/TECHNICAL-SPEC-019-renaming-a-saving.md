# TECHNICAL-SPEC-019: Renaming A Saving

Spec ID: `TECHNICAL-SPEC-019`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-009: Renaming A Saving`](FEATURE-SPEC-009-renaming-a-saving.md)

Covers: temporary rename state, name normalization and duplicate checks, complete candidate creation, and coordinated save results.

Status: `Accepted`

## Purpose

Replace the name of one current normal `Saving` without changing its stored planned money amount, order, the main money amount, or `Balance Changes`.

This spec implements the linked accepted FeatureSpec 009.

## Temporary Rename State

```ts
type RenameSavingState = {
  targetSavingId: string;
  rawName: string;
  confirming: boolean;
  saveFailureVisible: boolean;
};
```

TechnicalSpec 018 prepares this state from the same coherent snapshot used for its `saving-rename` continuation. Set `targetSavingId` to the selected normal record's stable ID, `rawName` to that record's current saved name, and both booleans to `false`. Publish the state and exact `{ kind: "saving-rename", savingId: targetSavingId }` descriptor together under the existing TechnicalSpec 010 token.

The state exists only in memory. Use a controlled name input that preserves the entered text and applies no length, word, or character restriction. When `confirming` is `false`, an edit that changes `rawName` is accepted and clears `saveFailureVisible`.

While `confirming` is `true`, ignore name edits and additional `Save` selections. `Cancel` and either Back action remain available and mark any pending save request as no longer current before closing the flow.

Clicking or tapping outside the rename flow and scrolling do not close or change it. TechnicalSpec 010 blocks every unrelated action while rename remains open.

## Validation And No-Write Results

When `Save` is chosen while `confirming` is `false`, capture one coordinator snapshot and one complete active TechnicalSpec 007 ready-state snapshot. Confirm that `Savings` is current, TechnicalSpec 009 is not updating, TechnicalSpec 017 has no active history operation, and the action token, coordinator revision, and complete expected `saving-rename` descriptor all match.

Resolve `targetSavingId` to exactly one normal slot in that ready snapshot. If it is unavailable, close the stale flow without writing or showing a message. Otherwise, clear the previous failure message, set `confirming` to `true`, copy the submitted `rawName`, and normalize that copy through TechnicalSpec 003 inside one failure boundary.

Apply these rules in order:

1. If the trimmed name is empty, reset `confirming` to `false` and keep the flow open without writing or showing a validation message.
2. If the trimmed saved name exactly equals the target's current stored name, close the flow without writing or showing a message.
3. Otherwise, compare its trimmed lowercase name with every other normal name and every readable name reserved by a broken slot.
4. If another slot reserves that comparison name, close the flow without writing or showing a duplicate-name message.
5. Otherwise, continue with the valid trimmed name.

The selected normal record is excluded from the duplicate check. This allows a case-only change when no other slot reserves that comparison name.

Missing, unchanged, duplicate, canceled, and unavailable-target results create no candidate and no storage request. They never change money information or `Balance Changes`. Unchanged and duplicate results close the flow; missing input keeps it open with the previous failure message cleared.

## Complete Candidate

For a valid new name, capture one `attemptTimeMs` and one fresh TechnicalSpec 009 request identity. The complete attempt owns the captured token, coordinator revision, expected descriptor, ready-state snapshot, target slot, submitted name, normalized name, time, and request identity.

Build a separate `StoredWebsiteDataV1` candidate only from that captured ready-state snapshot:

- replace only the captured target normal record at its captured `sourceIndex` with a fresh record containing the same `id`, `plannedMoneyAmount`, and `order`, plus the trimmed new `name`;
- preserve every other `Saving` value in saved-list order, including the exact `rawValue` of each broken slot;
- keep the main money amount unchanged; and
- remove only `Balance Changes` records that are expired at `attemptTimeMs`, while preserving every other history record in its current order.

Do not mutate the active document or its arrays. Pass the loaded slots from the same captured ready-state snapshot with the candidate so the TechnicalSpec 007 encoder can verify every retained broken raw value.

Keep normalization, duplicate scanning, time capture, and complete candidate construction inside the same failure boundary. If any of them throws, cannot finish, or produces an invalid candidate, register no save request and call no writer. Reset `confirming` to `false`, retain `rawName`, keep the old saved information active, and show `Changes could not be saved.`.

## Coordinated Save And Result

Immediately before registering the save, confirm again that the attempt's token, revision, descriptor, ready-state snapshot, target, and `confirming` state are current. A mismatch stops silently. Otherwise, register one TechnicalSpec 009 `user-action` request using the known baseline from that same ready-state snapshot. The flag and request registry prevent duplicate writes.

The active document and visible `Savings` information remain unchanged while the request is pending. The result handler acts only while the same token, revision, descriptor, target, and request identity still own the current rename attempt.

- `saved`: TechnicalSpec 009 atomically activates the complete candidate and returned baseline, runs the complete saved `Saving` list through TechnicalSpecs 005 and 007 again, clears pending history cleanup, and rebuilds coverage through TechnicalSpec 004. Another unchanged raw record becomes normal only when it now passes every validation rule; otherwise it remains broken. Then close the rename flow and TechnicalSpec 010 entry together, remove the temporary Back step through TechnicalSpec 017, and show no success message.
- `save-failed`: discard the candidate, keep the old name and all active information unchanged, reset `confirming`, retain `rawName`, and show `Changes could not be saved.`.
- `superseded` or silent invalidation: run no rename success or failure effect; TechnicalSpecs 008 and 009 discard the old flow and load the newer information.

Only the selected stored record's name changes. Revalidation may make another unchanged broken record appear normally and may therefore recalculate information and coverage inside `Savings`. No result changes the main money amount, creates a `Balance Changes` record, or publishes a newest-history scrolling signal. A successful complete save also completes any already-pending silent history cleanup contained in its candidate.

Every retry captures a fresh ready-state snapshot, target slot, time, candidate, and request identity. A late result whose ownership checks fail does nothing.

## Closing And Back Cleanup

Every close path first invalidates a pending request when one exists. Clear the rename state, its handlers, and matching TechnicalSpec 010 entry together.

- Browser/device Back consumes the existing temporary action Back step and keeps `Savings` active.
- `Cancel`, the website's Back action, unchanged or duplicate input, an unavailable target, and successful saving remove that step exactly once through TechnicalSpec 017's internal operation. New actions and normal navigation remain blocked until that cleanup finishes.
- Lifecycle discard and newer saved information use TechnicalSpecs 008, 009, and 017 to discard the flow and safely normalize its Back step.

Every close keeps the user inside `Savings`, changes no saved information unless the rename already succeeded, and shows no message.

## Failure Message Lifetime

After `save-failed`, keep `Changes could not be saved.` visible without a timer while the rename flow and entered name remain unchanged. An accepted name edit clears it. A retry clears it while the new attempt runs; another failure shows it again, and success closes the flow.

Cancel, either Back action, lifecycle discard, or newer saved information destroys the state and message. Outside interaction and scrolling leave both unchanged because they do not close or edit the flow. A later rename flow starts with no earlier message.

## Essential Verification

Verify that:

- a new state starts with the current saved name and two `false` booleans;
- missing, unchanged, duplicate, canceled, and stale-target attempts write nothing;
- trimming removes only surrounding spaces and valid names of every accepted character type remain allowed;
- the target does not duplicate itself, while all other normal and readable broken names remain reserved;
- outside interaction and scrolling keep rename open, and a pending attempt accepts no edit or repeated `Save`;
- one attempt uses one coherent ready-state snapshot, its matching target and baseline, and one fresh request identity;
- a valid candidate changes only the target name and normal expired-history cleanup;
- the target ID, planned money amount, and order remain unchanged;
- every other normal or broken stored value is preserved exactly;
- candidate-construction failure registers no request, keeps the entered name, and follows the save-failure result;
- repeated `Save` choices while one attempt is pending create one request;
- stale result handlers cannot affect a closed, replaced, or newer rename attempt;
- failure keeps the old saved name and entered new name available;
- the failure message clears after an accepted edit, retry, or closure and appears again after another failed retry;
- every close clears the action and its temporary Back step exactly once while keeping `Savings` active;
- successful complete-list validation makes another record normal only when it now passes every rule and recalculates `Savings` when needed; and
- success closes the flow without a message, history entry, or newest-history scrolling signal.

## Boundaries

TechnicalSpec 003 owns name normalization and stored `Saving` rules. TechnicalSpec 004 owns coverage calculation. TechnicalSpec 005 owns broken-slot identity, reserved readable names, and complete-list classification. TechnicalSpecs 006, 007, and 009 own encoding, ready-state rebuilding, atomic writing, and save coordination. TechnicalSpecs 008, 010, 017, and 018 own discard, one-action priority, Back-step cleanup, and the transition into rename.
