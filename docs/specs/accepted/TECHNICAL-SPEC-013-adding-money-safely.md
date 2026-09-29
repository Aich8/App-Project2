# TECHNICAL-SPEC-013: Adding Money Safely

Spec ID: `TECHNICAL-SPEC-013`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-001: Adding Money`](FEATURE-SPEC-001-adding-money.md)

Covers: validating a confirmed Add amount, calculating the exact result, building its complete saved-data candidate, and applying the coordinated save result.

Status: `Accepted`

## Purpose

Increase the main money amount by an exact confirmed amount. Build one matching positive `Balance Changes` record as part of the temporary save candidate. The new money amount and record become active and visible only after the complete candidate is saved successfully.

## Executor Contract

The Add executor receives:

- the integer cents produced by TechnicalSpec 001;
- the captured Add action token, coordinator revision, and complete expected `{ kind: "main-money-entry", action: "add" }` descriptor; and
- one complete TechnicalSpec 007 ready-state snapshot.

The executor reads the known stored-value baseline from that same ready-state snapshot. The baseline is not supplied separately, so information from two different ready states cannot be combined.

It returns `saved`, `not-applied`, or `save-failed` to the still-current TechnicalSpec 001 entry flow. A `superseded` or silently invalidated coordinated request discards the old flow through TechnicalSpecs 008 and 009 instead of delivering an outcome to it.

## Confirmation Validation

Before building a candidate:

1. confirm that the captured token, coordinator revision, and complete expected descriptor still match the current Add entry flow;
2. confirm that the supplied ready-state snapshot is still the active ready state;
3. return `not-applied` when the entered cents are not a safe integer from `0` through `99_999_999`;
4. return `save-failed` when the active ready state's current main money amount is not a safe integer from `0` through `99_999_999`;
5. return `not-applied` when the entered cents equal `0`;
6. calculate `resultCents = currentMainMoneyAmountCents + enteredCents` using integer arithmetic; and
7. return `not-applied` when `resultCents` is greater than `99_999_999`.

If either ownership check in steps 1 and 2 fails, stop silently without delivering an outcome, building a candidate, requesting a write, changing information, or showing a message. TechnicalSpec 001 prevents negative or above-limit entered values, but the executor rejects them defensively as `not-applied` without a write. The invalid-current-amount check is a defensive failure because TechnicalSpec 007 normally guarantees a valid ready state. No validation path uses decimal floating-point arithmetic.

A `not-applied` result creates no candidate, save request, or `Balance Changes` record. It keeps the Add flow and entered value available and shows no message.

## Attempt Time And Record

For a result that passes validation, capture one `attemptTimeMs` and its matching local date and time. A retry captures a new time. Use one failure boundary for both the record and complete candidate construction described in this and the next section.

Inside a failure boundary:

1. create one record ID with `crypto.randomUUID()` and confirm that it differs from every record ID in the current ready-state snapshot;
2. normalize the entered cents to the TechnicalSpec 002 stored decimal amount without a sign, comma, or `$` sign;
3. set `action` to `add`;
4. set `createdAtMs` to `attemptTimeMs`;
5. store the matching local year, month, day, hour, and minute; and
6. set `visibleUntilMs` to exactly `attemptTimeMs + 2_592_000_000`.

The time, local parts, ID, and expiration must pass the TechnicalSpec 002 record rules. A repeated ID or any other record-construction failure returns `save-failed` without calling the writer.

## Complete Candidate

Inside the same failure boundary, build a separate `StoredWebsiteDataV1` candidate from the same active ready-state snapshot:

- set `mainMoneyAmount` to the normalized decimal string for `resultCents` with exactly two decimal digits and no comma or `$` sign;
- remove any `Balance Changes` records expired at `attemptTimeMs`, then place the new Add record first;
- preserve every current `Saving` value in its existing saved-list order; and
- preserve the exact in-memory raw value of every broken `Saving` slot as required by TechnicalSpecs 005 through 007.

Do not mutate the active document or its arrays. Pass the current loaded `Saving` slots with the candidate so the TechnicalSpec 007 encoder can confirm every retained broken raw value.

The candidate contains the new money amount, the new history record, required history cleanup, and all unchanged saved information. These values must succeed or fail as one complete write.

If any record or complete-candidate construction step throws, cannot finish, or produces an invalid candidate, return `save-failed`, leave the active information unchanged, and do not register a save request or call the writer.

## Coordinated Save

Immediately before registering a save request, confirm again that the captured Add token, coordinator revision, complete expected descriptor, and ready-state snapshot are still current. A mismatch stops silently. Otherwise, create one TechnicalSpec 009 `user-action` request identity for the confirmation attempt and request a known-baseline save using the baseline from that same snapshot. The TechnicalSpec 001 `confirming` guard and the request registry prevent repeated confirmation from producing another writer call.

The active document, displayed money amount, and visible history remain unchanged while the request is pending.

The save-result handler captures the same Add token, coordinator revision, and complete expected descriptor. Before handling a delivered result, confirm that all three still match. A mismatch stops silently without applying an outcome or effect.

Handle the coordinated result as follows:

- `saved`: TechnicalSpec 009 atomically activates the complete candidate with the returned serialized baseline, derives all ready-state values from it, clears any pending balance cleanup, and then completes the Add success effects;
- `save-failed`: discard the candidate, leave the active ready state and baseline unchanged, and return `save-failed` to the entry flow; or
- `superseded` or silent invalidation: perform no Add success or failure effect and let TechnicalSpecs 008 and 009 discard the old flow and load the newer information.

## Successful Add Effects

After the complete saved candidate is active:

1. through TechnicalSpec 010, clear the Add entry state and its matching active action token and descriptor together in one operation;
2. show the new formatted main money amount through TechnicalSpec 011;
3. publish the new record ID so TechnicalSpec 002 moves `Balance Changes` to the top after the row renders; and
4. show no success message.

If the result is `99_999_999` cents, the TechnicalSpec 012 selector no longer includes `Add` the next time the main money actions open.

## Failed Save And Retry

On `save-failed`, TechnicalSpec 001 keeps the entered amount and Add flow open and shows `Changes could not be saved.` under its accepted message-lifetime rules. The failed candidate record is never added to the active history.

Retrying captures one new current ready-state snapshot, uses the known baseline inside that snapshot, and creates a new record ID and attempt time. A successful retry produces exactly one Add record; the failed attempt produces none.

## Essential Verification

Verify that:

- integer-cent addition is exact from `0.01$` through the maximum result;
- a stale token, revision, descriptor, or ready-state snapshot stops silently before a candidate or save request is created;
- the save-result handler ignores a result that no longer belongs to the same Add entry step;
- the stored-value baseline always comes from the same ready-state snapshot used to build the candidate;
- confirming `0.00$` returns `not-applied` and requests no save;
- invalid entered cents return `not-applied`, while an invalid current ready-state amount returns `save-failed`, and neither case calls the writer;
- a result above `999,999.99$` returns `not-applied`, keeps the flow open, and changes nothing;
- a valid result creates one normalized complete candidate without mutating active information;
- a repeated record ID or another record or candidate construction failure returns `save-failed` without registering a save request or calling the writer;
- the candidate adds exactly one positive record containing the entered amount, not the resulting total;
- the candidate removes expired history while preserving unexpired history order after the new first record;
- every `Saving`, including exact broken raw values, remains unchanged;
- repeated confirmation produces no duplicate save or record;
- no pending or failed request changes the displayed money amount or visible history;
- success activates the new money amount and matching record together, clears the Add entry state and matching active action together, and then scrolls history to the top;
- failure keeps the entered amount available and creates no active record;
- a retry creates fresh record identity and time but only its successful record becomes active;
- a superseded or invalidated request shows no failure or success effect; and
- a result at the maximum removes `Add` from later available actions.

## Boundaries

TechnicalSpec 001 owns amount entry and its failure-message state. TechnicalSpec 002 owns record validation, display, retention, and newest-entry scrolling. TechnicalSpecs 006 and 009 own atomic writing and save coordination. TechnicalSpec 014 and TechnicalSpec 015 separately own `Subtract` and `Modify` calculations.
