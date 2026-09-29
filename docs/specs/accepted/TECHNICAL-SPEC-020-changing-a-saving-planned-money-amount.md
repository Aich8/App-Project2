# TECHNICAL-SPEC-020: Changing A Saving's Planned Money Amount

Spec ID: `TECHNICAL-SPEC-020`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-010: Changing The Planned Money Amount Of A Saving`](FEATURE-SPEC-010-changing-the-planned-money-amount-of-a-saving.md)

Covers: temporary amount-change state, exact amount input and validation, replacement or zero-removal candidates, and coordinated save results.

Status: `Accepted`

## Purpose

Replace one normal `Saving`'s planned money amount, or remove that `Saving` when the confirmed amount is zero, without changing the main money amount or creating a `Balance Changes` entry.

This spec implements the linked accepted FeatureSpec 010.

## Temporary Change State

```ts
type ChangeSavingPlannedMoneyState = {
  targetSavingId: string;
  rawPlannedMoneyAmount: string;
  confirming: boolean;
  saveFailureVisible: boolean;
};
```

TechnicalSpec 018 prepares this state from the same coherent snapshot used for its `saving-planned-money-change` continuation. Set `targetSavingId` to the selected normal record's stable ID, `rawPlannedMoneyAmount` to its normalized stored amount such as `5895.50`, and both booleans to `false`. Publish the state and exact `{ kind: "saving-planned-money-change", savingId: targetSavingId }` descriptor together under the existing TechnicalSpec 010 token.

The state exists only in memory. `rawPlannedMoneyAmount` never includes a comma or `$` sign.

## Amount Input And Parsing

Reuse the TechnicalSpec 003 controlled `type="text"` input with `inputMode="decimal"`: accept digits and one decimal point, allow at most two fractional digits, and reject letters, spaces, `$` signs, commas, a second decimal point, and pasted content without changing the raw value or showing a message.

When `confirming` is `false`, an accepted input transition that changes the raw value clears `saveFailureVisible`. Rejected input leaves both the raw value and failure-message state unchanged.

While `confirming` is `true`, ignore amount edits and additional `Save` selections. `Cancel` and either Back action remain available and mark any pending save request as no longer current before closing the flow.

Clicking or tapping outside the change flow and scrolling do not close or change it. TechnicalSpec 010 blocks every unrelated action while the flow remains open.

Parse the raw text through string parts into exact integer cents. Do not use decimal floating-point arithmetic. A complete value must match `^(?:[0-9]+(?:\.[0-9]{0,2})?|\.[0-9]{1,2})$`, so `5`, `5.`, `5.0`, and `5.00` all produce `500` cents, `.50` produces `50` cents, and `005.00` produces `500` cents. A lone `.`, missing text, malformed text, or an amount above the limit returns `null`. Unlike creation, this flow accepts zero, so every accepted result is an integer from `0` through `99_999_999`.

## Validation And No-Write Results

When `Save` is chosen while `confirming` is `false`, capture one coordinator snapshot and one complete active TechnicalSpec 007 ready-state snapshot. Confirm that `Savings` is current, TechnicalSpec 009 is not updating, TechnicalSpec 017 has no active history operation, and the action token, coordinator revision, and complete expected `saving-planned-money-change` descriptor all match.

Then:

1. resolve `targetSavingId` to exactly one normal slot in that ready snapshot and close the stale flow without writing or showing a message when it is unavailable;
2. clear the previous failure message, set `confirming` to `true`, and copy the submitted raw amount;
3. parse that copy inside one failure boundary;
4. when parsing returns `null`, reset `confirming` to `false` and keep the flow open without writing or showing a validation message; and
5. when the parsed cents equal the target's current planned money amount, close the flow without writing or showing a message.

Canceled, invalid, unchanged, and unavailable-target results do not change saved or derived information.

## Complete Candidate

For a valid changed amount, capture one `attemptTimeMs` and one fresh TechnicalSpec 009 request identity. The complete attempt owns the captured token, coordinator revision, expected descriptor, ready-state snapshot, target slot, submitted raw amount, parsed cents, time, and request identity.

Build a separate `StoredWebsiteDataV1` candidate only from that captured ready-state snapshot.

When the amount is greater than zero:

- replace only the captured target normal record at its captured `sourceIndex`;
- store the new cents as the normalized TechnicalSpec 003 decimal string; and
- preserve the target's `id`, `name`, and `order`.

When the amount is zero, remove exactly the captured target item at its captured `sourceIndex`. Do not ask for another confirmation and do not renumber or reorder the remaining stored records.

For both candidates:

- preserve every other `Saving` value in saved-list order, including each broken slot's exact `rawValue`;
- keep the main money amount unchanged; and
- remove only `Balance Changes` records expired at `attemptTimeMs`, preserving every other history record in order.

Do not mutate the active document. Pass the loaded slots from the same captured ready-state snapshot to the TechnicalSpec 007 encoder so retained broken raw values can be verified.

Keep parsing, time capture, amount normalization, and complete candidate construction inside the same failure boundary. If any of them throws or cannot finish, register no save request and call no writer. Reset `confirming` to `false`, retain the raw entered amount, keep the old saved information active, and show `Changes could not be saved.`. An expected invalid input result still follows the no-message rule above.

After a complete candidate is registered, the shared TechnicalSpec 007 encoder performs the candidate validation owned by TechnicalSpecs 006 and 007. If the encoder rejects the candidate, `setItem` is not called and the coordinated request returns `save-failed`, which follows the failure result below.

## Coordinated Save And Result

Immediately before registering the save, confirm again that the attempt's token, revision, descriptor, ready-state snapshot, target, and `confirming` state are current. A mismatch stops silently. Otherwise, register one TechnicalSpec 009 `user-action` request using the known baseline from that same ready-state snapshot. The flag and request registry prevent duplicate writes.

The active document and visible `Savings` information remain unchanged while the request is pending. The result handler acts only while the same token, revision, descriptor, target, and request identity still own the current amount-change attempt.

- `saved`: First build one complete new TechnicalSpec 007 ready-state snapshot from the candidate and returned baseline. Run the complete saved `Saving` list through TechnicalSpecs 005 and 007 again, set pending history cleanup to `false`, and rebuild coverage through TechnicalSpec 004. After zero removal, another unchanged raw record becomes normal only when it now passes every validation rule; otherwise it remains broken. Only after every saved and derived value is ready, replace the old ready state with the new document, baseline, loaded lists, cleanup state, and coverage together so no partial result is exposed. Then close the change flow and TechnicalSpec 010 entry together, remove the temporary Back step through TechnicalSpec 017, and show no success message.
- `save-failed`: discard the candidate, leave the old planned money amount and selected `Saving` unchanged, reset `confirming`, retain the entered raw amount, and show `Changes could not be saved.`.
- `superseded` or silent invalidation: run no success or failure effect; TechnicalSpecs 008 and 009 discard the old flow and load the newer information.

If a zero-removal save fails, the target remains present. After successful zero removal, revalidation may make another unchanged broken record appear normally and therefore changes the recalculated `Savings` result. Every successful result creates no history record or newest-history scrolling signal and never changes the main money amount.

Every retry captures a fresh ready-state snapshot, target slot, time, candidate, and request identity. A late result whose ownership checks fail does nothing.

## Closing And Back Cleanup

Every close path first invalidates a pending request when one exists. Clear the amount-change state, its handlers, and matching TechnicalSpec 010 entry together.

- Browser/device Back consumes the existing temporary action Back step and keeps `Savings` active.
- `Cancel`, the website's Back action, unchanged input, an unavailable target, and successful saving remove that step exactly once through TechnicalSpec 017's internal operation. New actions and normal navigation remain blocked until that cleanup finishes.
- Lifecycle discard and newer saved information use TechnicalSpecs 008, 009, and 017 to discard the flow and safely normalize its Back step.

Every close keeps the user inside `Savings`, changes no saved information unless the planned money amount change already succeeded, and shows no message.

## Failure Message Lifetime

After `save-failed`, keep `Changes could not be saved.` visible without a timer while the change flow and entered amount remain unchanged. An accepted amount edit clears it; rejected input leaves it visible. A retry clears it while the new attempt runs; another failure shows it again, and success closes the flow.

Cancel, either Back action, lifecycle discard, or newer saved information destroys the state and message. Outside interaction and scrolling leave both unchanged because they do not close or edit the flow. A later amount-change flow starts with no earlier message.

## Essential Verification

Verify that:

- a new state starts with the current normalized planned money amount and two `false` booleans;
- typing and paste handling match FeatureSpec 010 exactly, and rejected input does not clear a failure message;
- parsing produces exact cents from `0` through `99_999_999` without decimal rounding, treats `5`, `5.`, `5.0`, and `5.00` as `500` cents, treats `.50` as `50` cents, treats `005.00` as `500` cents, and rejects a lone `.`;
- missing, invalid, unchanged, canceled, and stale-target attempts write nothing;
- outside interaction and scrolling keep the flow open, and a pending attempt accepts no edit or repeated `Save`;
- one attempt uses one coherent ready-state snapshot, its matching target and baseline, and one fresh request identity;
- a nonzero candidate changes only the target planned money amount and normal expired-history cleanup;
- zero removes exactly the target without another confirmation or order renumbering;
- all retained normal and broken stored values are preserved exactly;
- candidate-construction failure registers no request, keeps the entered amount, and follows the save-failure result;
- encoder rejection calls no `setItem` and returns the coordinated `save-failed` result;
- success recalculates `Savings` from the active candidate but does not change the main money amount;
- success builds every saved and derived ready-state value before replacing the active state, so no partial result is exposed;
- failure retains the target, its old saved amount, and the entered raw amount;
- repeated `Save` choices while pending create one request;
- stale result handlers cannot affect a closed, replaced, or newer change attempt;
- the failure message clears after an accepted edit, retry, or closure and appears again after another failed retry;
- every close clears the action and its temporary Back step exactly once while keeping `Savings` active;
- complete-list validation after zero removal makes another record normal only when it now passes every rule and recalculates `Savings` when needed; and
- success closes the flow without a message, history entry, or newest-history scrolling signal.

## Boundaries

TechnicalSpec 003 owns stored planned-money formatting and the shared typing behavior. TechnicalSpec 004 owns coverage calculation. TechnicalSpec 005 owns loaded slot identity and complete-list classification. TechnicalSpecs 006, 007, and 009 own encoding, ready-state rebuilding, atomic writing, and save coordination. TechnicalSpecs 008, 010, 017, and 018 own discard, one-action priority, Back-step cleanup, and the transition into this flow.
