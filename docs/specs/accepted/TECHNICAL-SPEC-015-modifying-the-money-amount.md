# TECHNICAL-SPEC-015: Modifying The Money Amount

Spec ID: `TECHNICAL-SPEC-015`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-003: Modifying The Money Amount`](FEATURE-SPEC-003-modifying-the-money-amount.md)

Covers: validating a confirmed complete replacement amount, building a complete saved-data candidate without creating a new `Balance Changes` record, and applying the coordinated save result.

Status: `Accepted`

## Purpose

Replace the complete main money amount exactly while creating no `Balance Changes`, activity, or notification entry.

## Executor Contract

The Modify executor receives:

- the integer cents produced by TechnicalSpec 001;
- the captured Modify action token, coordinator revision, and complete expected `{ kind: "main-money-entry", action: "modify" }` descriptor; and
- one complete TechnicalSpec 007 ready-state snapshot.

The executor reads the known stored-value baseline from that same ready-state snapshot. The baseline is not supplied separately, so information from two different ready states cannot be combined.

It returns `saved`, `not-applied`, or `save-failed` to the still-current TechnicalSpec 001 entry flow. A `superseded` or silently invalidated request discards the old flow through TechnicalSpecs 008 and 009 instead of delivering an outcome to it.

## Confirmation Validation

Before building a candidate:

1. confirm that the captured token, coordinator revision, and complete expected descriptor still match the current Modify entry flow;
2. confirm that the supplied ready-state snapshot is still the active ready state;
3. return `not-applied` when the entered cents are not a safe integer from `0` through `99_999_999`;
4. return `save-failed` when the active ready state's current main money amount is not a safe integer from `0` through `99_999_999`; and
5. return `not-applied` when the entered cents exactly equal the current main money amount.

If either ownership check in steps 1 and 2 fails, stop silently without delivering an outcome, building a candidate, requesting a write, changing information, or showing a message. TechnicalSpec 001 prevents negative or above-limit entered values, but the executor rejects them defensively as `not-applied` without a write. The invalid-current-amount check is a defensive failure because TechnicalSpec 007 normally guarantees a valid ready state. The entered integer cents are the complete replacement value; they are never added to or subtracted from the current amount.

A `not-applied` result keeps the Modify flow and entered value available and shows no message. It creates no candidate, performs no history cleanup, requests no storage write, and publishes no history, activity, or notification event.

## Complete Candidate

For a different valid amount, capture one `attemptTimeMs` for the TechnicalSpec 002 expiration check and build a separate `StoredWebsiteDataV1` candidate from the same active ready-state snapshot:

- set `mainMoneyAmount` to the normalized decimal string for the entered cents with exactly two decimal digits and no comma or `$` sign;
- remove any `Balance Changes` records expired at `attemptTimeMs` while preserving every unexpired record in its existing saved-list order;
- create no new `Balance Changes` record; and
- preserve every current `Saving` value in its existing saved-list order, reusing the exact in-memory raw value of each broken slot.

Build the candidate inside a failure boundary without mutating active information. Pass the current loaded `Saving` slots with it so the encoder can verify retained broken raw values. Candidate-construction failure returns `save-failed`, leaves all active information unchanged, registers no save request, and does not call the writer.

The replacement money amount, required history cleanup, and every unchanged saved value form one complete candidate and must succeed or fail together.

## Coordinated Save

Immediately before registering a save request, confirm again that the captured Modify token, coordinator revision, complete expected descriptor, and ready-state snapshot are still current. A mismatch stops silently. Otherwise, create one TechnicalSpec 009 `user-action` request identity for the confirmation attempt and request a known-baseline save using the baseline from that same ready-state snapshot. The TechnicalSpec 001 `confirming` guard and request registry prevent a duplicate writer call.

The active money amount and history remain unchanged while the request is pending.

The save-result handler captures the same Modify token, coordinator revision, and complete expected descriptor. Before handling a delivered result, confirm that all three still match. A mismatch stops silently without applying an outcome or effect.

- `saved`: atomically activate the complete candidate, use the returned serialized value as the new storage baseline, derive every ready-state value from the candidate, clear pending balance cleanup, and then run the Modify success effects.
- `save-failed`: discard the candidate, retain the active ready state and baseline, and return `save-failed` to the entry flow.
- `superseded` or silent invalidation: run no Modify success or failure effect and let TechnicalSpecs 008 and 009 discard the old flow and load newer information.

## Successful Modify Effects

After the complete saved candidate is active:

1. through TechnicalSpec 010, clear the Modify entry state and its matching active action token and descriptor together in one operation;
2. show the corrected formatted main money amount through TechnicalSpec 011; and
3. show no success message.

Do not publish a newest-history record ID or create any history, activity, or notification entry. TechnicalSpec 012 derives the later available actions from the corrected amount.

## Failed Save And Retry

On `save-failed`, TechnicalSpec 001 keeps the entered value and Modify flow open and shows `Changes could not be saved.` under its accepted lifetime rules. The displayed main money amount and all saved information remain unchanged.

A retry captures one new current ready-state snapshot, uses the known baseline inside that snapshot, and captures a new `attemptTimeMs` for the expiration check. Success replaces the amount once; earlier failed candidates produce no active result.

## Essential Verification

Verify that:

- every different integer-cent amount from `0` through `99_999_999` can become the exact replacement;
- the replacement is never added to or subtracted from the old amount;
- a stale token, revision, descriptor, or ready-state snapshot stops silently before a candidate or save request is created;
- the save-result handler ignores a result that no longer belongs to the same Modify entry step;
- the stored-value baseline always comes from the same ready-state snapshot used to build the candidate;
- confirming the current amount returns `not-applied`, keeps the flow open, and performs no cleanup or storage request;
- invalid entered cents return `not-applied`, while an invalid current ready-state amount returns `save-failed`, and neither case calls the writer;
- a valid different amount creates one normalized complete candidate without mutating active information;
- candidate-construction failure leaves active information unchanged and registers no save request or writer call;
- the candidate creates no new `Balance Changes`, activity, or notification entry;
- expired history is removed while every unexpired record retains its order and contents;
- every `Saving`, including exact broken raw values, remains unchanged;
- repeated confirmation produces no duplicate write;
- pending and failed requests do not change the displayed money amount or saved information;
- success atomically activates the corrected amount and cleaned unchanged information, then clears the Modify entry state and matching active action together;
- success publishes no newest-entry scrolling ID and shows no success message;
- failure keeps the entered amount available and creates no activity of any kind;
- a superseded or invalidated request produces no success or failure effect; and
- every retry uses a new current ready-state snapshot, its matching baseline, and a new expiration-check time.

## Boundaries

TechnicalSpec 001 owns amount entry and its failure-message state. TechnicalSpec 002 owns existing history validation, retention, and display. TechnicalSpecs 006 and 009 own atomic writing and save coordination. TechnicalSpecs 013 and 014 separately own `Add` and `Subtract` calculations.
