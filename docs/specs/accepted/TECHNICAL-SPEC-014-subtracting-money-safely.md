# TECHNICAL-SPEC-014: Subtracting Money Safely

Spec ID: `TECHNICAL-SPEC-014`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-002: Subtracting Money`](FEATURE-SPEC-002-subtracting-money.md)

Covers: validating a confirmed Subtract amount, calculating the non-negative result and actual removed amount, building the complete candidate, and applying the coordinated save result.

Status: `Accepted`

## Purpose

Lower the main money amount without going below zero. Build one matching negative `Balance Changes` record for only the money amount actually removed as part of the temporary save candidate. The new money amount and record become active and visible only after the complete candidate is saved successfully.

## Executor Contract

The Subtract executor receives:

- the integer cents produced by TechnicalSpec 001;
- the captured Subtract action token, coordinator revision, and complete expected `{ kind: "main-money-entry", action: "subtract" }` descriptor; and
- one complete TechnicalSpec 007 ready-state snapshot.

The executor reads the known stored-value baseline from that same ready-state snapshot. The baseline is not supplied separately, so information from two different ready states cannot be combined.

It returns `saved`, `not-applied`, or `save-failed` to the still-current TechnicalSpec 001 entry flow. A `superseded` or silently invalidated request discards the old flow through TechnicalSpecs 008 and 009 instead of delivering an outcome to it.

## Confirmation Validation And Calculation

Before building a candidate:

1. confirm that the captured token, coordinator revision, and complete expected descriptor still match the current Subtract entry flow;
2. confirm that the supplied ready-state snapshot is still the active ready state;
3. return `not-applied` when the entered cents are not a safe integer from `0` through `99_999_999`;
4. return `save-failed` when the active ready state's current main money amount is not a safe integer from `0` through `99_999_999`;
5. return `not-applied` when the entered cents equal `0`;
6. return `not-applied` when the current main money amount equals `0`;
7. calculate `removedCents = Math.min(enteredCents, currentMainMoneyAmountCents)`; and
8. calculate `resultCents = currentMainMoneyAmountCents - removedCents`.

If either ownership check in steps 1 and 2 fails, stop silently without delivering an outcome, building a candidate, requesting a write, changing information, or showing a message. TechnicalSpec 001 prevents negative or above-limit entered values, but the executor rejects them defensively as `not-applied` without a write. The invalid-current-amount check is a defensive failure because TechnicalSpec 007 normally guarantees a valid ready state. Every calculation uses integer cents. `removedCents` is always positive for a valid action, and `resultCents` can never be negative.

A `not-applied` result creates no candidate, save request, or `Balance Changes` record. It keeps the Subtract flow and entered value available and shows no message.

## Attempt Time And Record

For a result that passes validation, capture one `attemptTimeMs` and its matching local date and time. A retry captures a new time. Use one failure boundary for both the record and complete candidate construction described in this and the next section.

Inside a failure boundary, build one TechnicalSpec 002 record:

- create its ID with `crypto.randomUUID()` and confirm that it differs from every record ID in the current ready-state snapshot;
- set `action` to `subtract`;
- normalize `removedCents`, not the larger entered amount, as its positive stored decimal amount;
- set `createdAtMs` and the recorded local date and time from the same attempt time; and
- set `visibleUntilMs` to exactly `attemptTimeMs + 2_592_000_000`.

The ID, amount, time, local parts, and expiration must pass every TechnicalSpec 002 record rule. A repeated ID or any other record-construction failure returns `save-failed` without calling the writer.

## Complete Candidate

Inside the same failure boundary, build a separate `StoredWebsiteDataV1` candidate from the same active ready-state snapshot:

- set `mainMoneyAmount` to the normalized decimal string for `resultCents` with two decimal digits and no comma or `$` sign;
- remove history records expired at `attemptTimeMs`, then place the new Subtract record first;
- preserve every current `Saving` value in its existing saved-list order; and
- reuse the exact in-memory raw value of every broken `Saving` slot.

Do not mutate the active document or its arrays. Pass the current loaded `Saving` slots with the candidate so the encoder can verify every retained broken raw value.

The new main money amount, negative history record, required history cleanup, and unchanged saved information form one complete candidate and must succeed or fail together.

If any record or complete-candidate construction step throws, cannot finish, or produces an invalid candidate, return `save-failed`, leave the active information unchanged, and do not register a save request or call the writer.

## Coordinated Save

Immediately before registering a save request, confirm again that the captured Subtract token, coordinator revision, complete expected descriptor, and ready-state snapshot are still current. A mismatch stops silently. Otherwise, create one TechnicalSpec 009 `user-action` request identity for the confirmation attempt and request a known-baseline save using the baseline from that same snapshot. The TechnicalSpec 001 `confirming` guard and the request registry prevent another writer call for repeated confirmation.

The active money amount and visible history stay unchanged while the request is pending.

The save-result handler captures the same Subtract token, coordinator revision, and complete expected descriptor. Before handling a delivered result, confirm that all three still match. A mismatch stops silently without applying an outcome or effect.

- `saved`: atomically activate the complete candidate, use the returned serialized value as the new storage baseline, derive all ready-state values from the candidate, clear pending balance cleanup, and then run the Subtract success effects.
- `save-failed`: discard the candidate, keep the active ready state and baseline unchanged, and return `save-failed` to the entry flow.
- `superseded` or silent invalidation: run no Subtract success or failure effect and let TechnicalSpecs 008 and 009 discard the old flow and load the newer information.

## Successful Subtract Effects

After the saved candidate is active:

1. through TechnicalSpec 010, clear the Subtract entry state and its matching active action token and descriptor together in one operation;
2. show the new formatted main money amount through TechnicalSpec 011;
3. publish the new record ID so TechnicalSpec 002 moves `Balance Changes` to the top after rendering; and
4. show no success message.

If `resultCents` is `0`, the TechnicalSpec 012 selector no longer includes `Subtract` when the main money actions next open.

## Failed Save And Retry

On `save-failed`, TechnicalSpec 001 keeps the entered value and Subtract flow open and shows `Changes could not be saved.` under its accepted lifetime rules. The failed candidate record never becomes active.

A retry captures one new current ready-state snapshot, uses the known baseline inside that snapshot, and creates a new record ID and attempt time. Only a successful retry produces one active Subtract record.

## Essential Verification

Verify that:

- subtracting less than or equal to the current amount removes exactly the entered integer cents;
- subtracting more than the current amount produces `0.00$` without a negative result;
- an over-available subtraction records only the amount that was available;
- a stale token, revision, descriptor, or ready-state snapshot stops silently before a candidate or save request is created;
- the save-result handler ignores a result that no longer belongs to the same Subtract entry step;
- the stored-value baseline always comes from the same ready-state snapshot used to build the candidate;
- confirming `0.00$` requests no save and keeps the flow open;
- a defensive request from an existing `0.00$` state requests no save or record;
- invalid entered cents return `not-applied`, while an invalid current ready-state amount returns `save-failed`, and neither case calls the writer;
- one valid attempt builds one normalized complete candidate without mutating active information;
- a repeated record ID or another record or candidate construction failure returns `save-failed` without registering a save request or calling the writer;
- expired history is removed, the new Subtract record is first, and every unexpired older record keeps its order;
- every `Saving`, including exact broken raw values, remains unchanged;
- repeated confirmation produces no duplicate save or record;
- pending and failed requests change neither the displayed amount nor history;
- success activates the lower amount and matching negative record together, clears the Subtract entry state and matching active action together, and then moves history to the top;
- failure retains the entered amount and produces no active record;
- a superseded or invalidated request produces no success or failure effect; and
- a zero result removes `Subtract` from later available actions.

## Boundaries

TechnicalSpec 001 owns amount entry and its failure-message state. TechnicalSpec 002 owns record validation, display, retention, and newest-entry scrolling. TechnicalSpecs 006 and 009 own atomic writing and save coordination. TechnicalSpecs 013 and 015 separately own `Add` and `Modify` calculations.
