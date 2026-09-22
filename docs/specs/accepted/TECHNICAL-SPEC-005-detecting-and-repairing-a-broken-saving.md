# TECHNICAL-SPEC-005: Detecting And Repairing A Broken Saving

Spec ID: `TECHNICAL-SPEC-005`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-015: Fixing A Saving That Could Not Be Loaded`](FEATURE-SPEC-015-fixing-a-saving-that-could-not-be-loaded.md)

Covers: identifying one unreadable saved `Saving`, keeping its place, excluding it from calculations, and replacing it with a valid repaired record.

Status: `Accepted`

## Purpose

Keep the usable website information available when one saved `Saving` is broken, and let the user replace that broken record without changing the main money amount or `Balance Changes`.

## Loader Boundary

This spec receives the parsed items from the saved `Saving` list after the surrounding saved-data document and data version are readable.

An unreadable saved-data document or an unusable saved `Saving` list belongs to whole-data recovery. One unusable item inside an otherwise readable list becomes a broken `Saving` under this spec.

Detecting a broken `Saving` does not rewrite browser storage. Its original saved value remains unchanged until a successful fix or confirmed deletion.

## Saved Record Validation

A normal record must satisfy the accepted `StoredSavingV1` rules:

- it is a non-array object;
- `id` is a canonical UUID string;
- `name` is a string, is not empty after trimming, and has no surrounding spaces;
- `plannedMoneyAmount` is a normalized decimal string from `0.01` through `999999.99` with exactly two decimal digits, no sign, comma, `$` sign, or unnecessary leading zero;
- `order` is a non-negative safe integer; and
- its ID, normalized comparison name, and order are unique in the saved `Saving` list.

The validator scans records in saved-list order. The first usable ID, trimmed lowercase comparison name, or order reserves that value. A later matching value is a duplicate, even when the earlier record is broken for a different reason.

A readable broken name is any string whose trimmed value is not empty. Its trimmed lowercase comparison value remains reserved until that broken record is fixed or deleted.

## Loaded Slot Model

```ts
type BrokenSavingReason =
  | "invalid-record"
  | "invalid-id"
  | "duplicate-id"
  | "invalid-name"
  | "duplicate-name"
  | "invalid-planned-money-amount"
  | "invalid-order"
  | "duplicate-order";

type LoadedSavingSlot =
  | {
      kind: "normal";
      sourceIndex: number;
      record: StoredSavingV1;
    }
  | {
      kind: "broken";
      sourceIndex: number;
      runtimeKey: symbol;
      rawValue: unknown;
      reservedComparisonName: string | null;
      reasons: readonly BrokenSavingReason[];
    };
```

- `sourceIndex` identifies the record's place in the currently loaded saved list.
- `runtimeKey` is a unique in-memory symbol created separately for every broken slot during each complete load.
- A later load creates new symbols, so a key from an older loaded state cannot identify a different broken slot.
- `runtimeKey` is used only for in-memory identity. It is never rendered as stored information, converted to a saved ID, or written to browser storage.
- `rawValue` is retained unchanged for a later fix or deletion candidate.
- `reasons` are internal and are not user-facing error details.

Broken slots stay fixed in their loaded positions. Normal records are ordered by their valid `order` values and occupy the remaining normal positions. This lets normal `Saving` records be reordered while broken slots remain in place.

## Broken Saving Read Model

A broken slot produces a broken `Saving` read model with the exact text `Saving could not be loaded.` and the exact actions `Fix` and `Delete`.

It produces no planned money amount or coverage input, cannot become a reorder target, and does not affect `Savings money amount`, the overall amount needed, or another `Saving`'s coverage.

Choosing `Fix` opens one temporary fix state for that broken slot. Choosing `Delete` passes that same slot identity to the accepted `Saving` deletion flow; deletion implementation remains separate.

## Temporary Fix State

```ts
type FixSavingState = {
  targetRuntimeKey: symbol;
  rawName: string;
  rawPlannedMoneyAmount: string;
  confirming: boolean;
  saveFailureVisible: boolean;
};
```

Choosing `Fix` creates this state with the selected broken slot's `runtimeKey`, `rawName` set to `""`, `rawPlannedMoneyAmount` set to `""`, `confirming` set to `false`, and `saveFailureVisible` set to `false`.

No readable name or planned money amount is copied from the broken raw value. The empty strings contain no entered characters or digits and do not represent `0` or `0.00`.

The state exists only in memory and is never saved as a draft. Name normalization and planned-money input use the same accepted adapters and exact-cents conversion as `Saving` creation.

Cancel, either Back action, refresh, close, reopen, or a newer cross-tab update discards the fix state and leaves the original broken record unchanged.

## Fix Validation

When `Save` is chosen, resolve `targetRuntimeKey` against the current loaded state before validating the entered information. If no broken slot has that exact symbol, return `target-unavailable` immediately.

When the target is available, validate in this exact order:

1. the planned money amount is missing, below `1` cent, above `99_999_999` cents, or otherwise invalid;
2. the trimmed name is empty; and
3. the trimmed lowercase comparison name is reserved by another normal or broken `Saving`.

The target broken record does not reserve its own name against itself. All other readable broken names remain reserved.

```ts
type FixSavingOutcome =
  | "fixed"
  | "invalid-amount"
  | "missing-name"
  | "duplicate-name"
  | "target-unavailable"
  | "save-failed";
```

- `invalid-amount` and `missing-name` keep the fix flow open without changing anything or showing a validation message.
- `duplicate-name` changes nothing and closes the fix flow.
- `target-unavailable` closes and destroys the fix state, calls no storage writer, shows no message, and leaves the latest successfully saved information visible.
- `save-failed` keeps the fix flow and its entered values available and shows `Changes could not be saved.`.

## Save Attempt And Failure Message

Choosing `Save` while `confirming` is already `true` does nothing. Otherwise, choosing `Save` clears any previous save-failure message, sets `confirming` to `true`, and starts exactly one attempt.

Every completed attempt resets `confirming` to `false` unless its result closes and destroys the fix state. Invalid-amount and missing-name results keep the flow open with `saveFailureVisible` set to `false`. Duplicate-name and target-unavailable results close and destroy the state.

An accepted edit that changes either entered value sets `saveFailureVisible` to `false`. Rejected planned money amount input leaves it unchanged because the entered value did not change.

A `save-failed` result resets `confirming` to `false` and sets `saveFailureVisible` to `true`. Another retry clears that message while its attempt runs and shows it again only if that attempt also fails. A successful fix closes and destroys the state.

Cancel, either Back action, refresh, close, reopen, or a newer cross-tab update destroys the complete fix state, including any failure message. A later fix flow starts without the previous message.

## Candidate Repair

After validation succeeds:

1. create a UUID that does not match any usable ID in another saved `Saving` record;
2. normalize the trimmed name and exact planned money amount using the accepted `StoredSavingV1` rules;
3. replace the target broken slot in a candidate copy of the latest loaded `Saving` list;
4. keep every remaining broken raw value in its current slot without changing it;
5. collect the usable order numbers from all remaining broken raw records and reserve those numbers;
6. assign every normal record the next available non-negative safe-integer order, beginning with `0`, in the normal records' current visible top-to-bottom order and skipping every reserved number; and
7. pass the complete candidate to the shared atomic browser-storage writer.

An order number in a remaining broken raw record is usable and reserved when it is a non-negative safe integer, even when that record is broken for another field or has a duplicate order. Repeated reserved values occupy one entry in the reserved set.

The allocator starts with `0`. Before assigning an order to each normal record, it moves upward until it finds a safe integer that is not reserved. It assigns that number and continues above it for the next normal record. If no unused safe integer is available, the repair returns `save-failed` without calling the storage writer or changing saved information.

The assigned order values are unique and increase in the normal records' current visible order. Renumbering changes their internal order values without changing their relative order. The repaired `Saving` therefore keeps the broken slot's previous visible place when the other broken slots remain broken; complete-list revalidation handles any record whose validity changes.

The repair becomes active only after the complete storage write succeeds. Success closes the fix flow and runs the complete saved `Saving` list through the same validation and loading process again instead of retaining earlier normal or broken classifications.

The repaired target becomes a normal `Saving`. Any other raw record that now passes every validation rule becomes normal, even if it was broken before the fix. Any record that still fails at least one rule remains broken. Revalidation does not rewrite the raw value of another broken record.

After revalidation, `Savings` is recalculated from the newly loaded normal records.

A failed write discards the candidate and leaves the original broken record and all other saved information unchanged. Repair never changes the main money amount or creates, removes, or edits a `Balance Changes` entry.

## Essential Verification

Verify that:

- each invalid or duplicate required field makes only that saved item broken when the surrounding saved data remains usable;
- duplicate ID, name, and order detection is deterministic in saved-list order;
- a readable name from a broken record remains reserved for create, rename, and other fix operations;
- every broken slot receives a different in-memory symbol during a load, and a later load reuses none of those symbol identities;
- a broken record is visible but supplies no coverage or money calculation input and cannot be reordered;
- normal records retain their order around the remaining broken slots;
- a newly opened fix state contains two empty raw input strings, two `false` booleans, and no information copied from the broken raw value;
- amount validation happens before missing-name and duplicate-name validation;
- the target record does not block its own readable name, while every other reserved name still does;
- invalid and duplicate fixes write nothing and leave the record broken;
- a target-unavailable result closes the stale fix flow without calling the storage writer or showing a message;
- a successful fix replaces only the selected broken value with one valid normalized record in the same visible place;
- every usable order number from a remaining broken raw record is reserved before normal orders are assigned;
- the successful candidate gives normal records unique increasing safe-integer orders, skips all reserved order numbers, and keeps their visible relative order;
- repair returns `save-failed` without writing when no unused safe order number is available;
- the complete list is revalidated after a successful fix, so another unchanged record is normal only when it now passes every rule and otherwise remains broken;
- a failed write retains the broken record, entered fix values, and all previously saved information;
- repeated `Save` selections while one attempt is running do not start additional attempts;
- the failure message clears after an accepted edit, retry, or closure and appears again after another failed retry; and
- detection and repair never change the main money amount or `Balance Changes`.

## Boundaries

Whole-data recovery, deletion execution, the atomic write mechanism, cross-tab delivery, coverage calculation, and reorder interaction belong to their matching technical work.
