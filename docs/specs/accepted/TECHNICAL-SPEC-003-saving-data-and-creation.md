# TECHNICAL-SPEC-003: Saving Data And Creation

Spec ID: `TECHNICAL-SPEC-003`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-007: Creating A Saving`](FEATURE-SPEC-007-creating-a-saving.md)

Covers: the saved `Saving` record, temporary creation state, planned money amount input, normalization, validation, ordering, and creation result.

Status: `Accepted`

## Purpose

Create one valid `Saving` record without changing the main money amount or creating a `Balance Changes` entry.

## Stored Record

```ts
type StoredSavingV1 = {
  id: string;
  name: string;
  plannedMoneyAmount: string;
  order: number;
};
```

- `id` is unique and created with `crypto.randomUUID()`.
- `name` is the saved name after surrounding spaces are removed.
- `plannedMoneyAmount` is a normalized decimal string from `0.01` through `999999.99` with exactly two decimal digits, no sign, no comma, no `$` sign, and no unnecessary leading zeros.
- `order` is a unique non-negative safe integer used to restore the saved order.

The record does not store coverage, a needed money amount, the main money amount, temporary input, or created and updated dates.

After validation, `plannedMoneyAmount` is converted to integer cents for calculations. Decimal floating-point arithmetic is not used.

## Name Normalization

The creation executor derives two values from the entered name:

```ts
const savedName = rawName.trim();
const comparisonName = savedName.toLowerCase();
```

- `savedName` preserves all spaces and characters inside the name.
- `comparisonName` is a temporary derived value used only for duplicate checks. It is never shown or stored.
- An empty `savedName` is a missing name.
- No length check, truncation, word restriction, or character restriction is applied.
- Two names are duplicates when their `comparisonName` values match.

Duplicate checks include normal saved `Saving` names and readable non-empty names reserved by broken saved `Saving` records. The saved-data loader supplies those reserved names. Duplicate-ID and broken-record handling remain the responsibility of saved-data loading and recovery.

## Temporary Creation State

```ts
type SavingCreationState = {
  rawName: string;
  rawPlannedMoneyAmount: string;
  confirming: boolean;
  saveFailureVisible: boolean;
};
```

A new creation flow starts with both strings empty and both booleans set to `false`. This state exists only in memory and is never saved as a draft.

The planned money amount uses a controlled `type="text"` input with `inputMode="decimal"`:

- digits and one decimal point are accepted;
- no more than two digits are accepted after the decimal point;
- letters, spaces, `$` signs, commas, a second decimal point, and pasted content are rejected;
- rejected input leaves the raw value and message state unchanged; and
- the raw decimal text remains visible while the creation flow stays open.

The adapter converts the raw text to exact integer cents when `Save` is chosen. It uses string parsing rather than decimal floating-point arithmetic, right-pads zero or one fractional digit to two digits, and returns `null` when the text is missing, does not represent a money amount, represents `0.00$`, or is greater than `999,999.99$`.

## Creation Input

The save-time executor receives:

```ts
type CreateSavingInput = {
  rawName: string;
  plannedMoneyAmountCents: number | null;
};
```

`plannedMoneyAmountCents` must be an integer from `1` through `99_999_999`. The input adapter must produce this value without decimal floating-point arithmetic.

The executor receives the parsed cents only when `Save` is chosen. The raw input remains owned by `SavingCreationState`.

## Validation Order

The executor validates in this exact order:

1. The planned money amount is missing, non-integer, below `1` cent, or above `99_999_999` cents.
2. The normalized name is empty.
3. The normalized comparison name is already reserved.

The executor returns one outcome:

```ts
type CreateSavingOutcome =
  | "saved"
  | "invalid-amount"
  | "missing-name"
  | "duplicate-name"
  | "save-failed";
```

- `invalid-amount` and `missing-name` keep the creation flow open without saving.
- `duplicate-name` saves nothing and lets the creation flow close under FeatureSpec 007.
- `save-failed` keeps the entered values available while the flow remains open.
- None of these unsuccessful outcomes changes existing saved information or creates a `Balance Changes` entry.

Every completed attempt resets `confirming` to `false` unless the flow closes. Invalid amount and missing-name outcomes leave `saveFailureVisible` set to `false`; a duplicate-name outcome closes and destroys the temporary state.

## Candidate Save

After validation succeeds, the executor:

1. creates one unique ID;
2. stores the trimmed name;
3. converts the exact integer cents to the normalized stored money amount;
4. calculates the new record's `order`; and
5. appends the record after every existing `Saving` in a candidate copy of the latest saved information.

An existing `order` is usable for this calculation when it is a non-negative safe integer, including when its record is broken for another reason. The new `order` is `0` when no usable existing order is present; otherwise, it is one greater than the highest usable existing order.

If one greater than the highest order is not a safe integer, creation returns `save-failed` without calling the storage writer or changing anything. Appending the new record and giving it the new highest usable order keeps every existing `Saving` in its current relative position and gives the new `Saving` the last position.

The candidate is passed to the shared atomic browser-storage writer. The new `Saving` becomes active only after the complete save succeeds. A failed save discards the candidate, keeps the last successfully saved information active, and returns `save-failed`.

A successful creation closes the flow and makes the new record available to the `Savings` read model. Coverage and `Savings money amount` are derived from successfully saved records by their matching technical work.

Creation never changes the main money amount, adds or removes a `Balance Changes` record, or creates a success message.

## Failure Message And Flow Lifetime

The entered name and planned money amount remain only in the open creation state until saving succeeds.

An accepted edit that changes either value sets `saveFailureVisible` to `false`. Rejected amount input leaves it unchanged because the entered value did not change.

Choosing `Save` clears the previous failure message and sets `confirming` to `true` while one attempt runs. The guard prevents a duplicate attempt. A `save-failed` result resets `confirming` and sets `saveFailureVisible` to `true`; a failed retry therefore shows the message again. A successful save closes and destroys the state.

Cancel, either Back action, refresh, close, reopen, or a newer cross-tab update discards the complete temporary creation state, including any failure message. A later creation flow always starts clean.

## Essential Verification

Verify that:

- valid one-character, number-only, multi-word, symbol, punctuation, emoji, and long names are accepted;
- surrounding spaces are removed while internal spaces remain unchanged;
- duplicate checks ignore case and surrounding spaces but do not alter the saved visible name;
- normal and reserved readable broken names participate in duplicate checks;
- planned money amounts accept exactly `1` through `99_999_999` integer cents and round-trip without decimal rounding;
- planned money amount typing accepts and rejects the exact input required by FeatureSpec 007 and remains raw until saving;
- planned money amount validation happens before missing-name and duplicate-name validation;
- one valid creation produces one normalized record with a unique ID at the end of the existing order;
- the first saved `Saving` receives order `0`, and later creations receive one more than the highest usable existing order even when earlier order numbers contain gaps;
- creation fails without changing anything when no next safe order can be assigned;
- existing `Saving` order and coverage priority remain unchanged when a new record is appended;
- invalid, duplicate, canceled, failed, and unfinished creation attempts save no record;
- a failed save keeps existing saved information unchanged;
- the failure message clears after an accepted edit, retry, or closure and reappears after another failed retry; and
- creation never changes the main money amount or `Balance Changes`.

## Boundaries

Coverage calculations, saved-data recovery, the atomic write mechanism, cross-tab synchronization, and later rename, amount-change, deletion, and reorder operations belong to their matching technical work.
