# TECHNICAL-SPEC-004: Savings Coverage Calculations

Spec ID: `TECHNICAL-SPEC-004`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-013: Viewing Savings Coverage`](FEATURE-SPEC-013-viewing-savings-coverage.md)

Covers: exact coverage calculation for ordered `Saving` records, `Savings money amount`, and the overall amount needed.

Status: `Accepted`

## Purpose

Derive all `Savings` coverage information from the latest successfully kept main money amount and valid `Saving` records without moving or changing money.

## Calculation Input

```ts
const ZERO_CENTS = BigInt(0);
const ONE_CENT = BigInt(1);
const MAX_MONEY_AMOUNT_CENTS = BigInt("99999999");

type CoverageSaving = {
  id: string;
  plannedMoneyAmountCents: bigint;
};

type SavingsCoverageInput = {
  mainMoneyAmountCents: bigint;
  savings: readonly CoverageSaving[];
};
```

- `mainMoneyAmountCents` is from `ZERO_CENTS` through `MAX_MONEY_AMOUNT_CENTS`.
- Every `plannedMoneyAmountCents` is from `ONE_CENT` through `MAX_MONEY_AMOUNT_CENTS`.
- Each `id` is unique.
- `savings` contains only normal valid `Saving` records in their visible top-to-bottom order.
- Broken `Saving` records and unsaved temporary input are not included.

The loader and ordering work prepare this input. The coverage calculator does not repair records or choose their order.

The existing money helpers provide validated integer-cent `number` values. The input adapter converts each one with `BigInt(value)` before calling the coverage calculator. Conversion happens only after the value is confirmed to be a safe integer inside its accepted range, and the calculator never mixes `number` and `bigint` arithmetic.

## Calculation Result

```ts
type SavingCoverageState =
  | "fully-covered"
  | "partly-covered"
  | "not-covered";

type SavingCoverageResult = {
  id: string;
  plannedMoneyAmountCents: bigint;
  coveredCents: bigint;
  neededCents: bigint;
  state: SavingCoverageState;
};

type SavingsCoverageResult = {
  savings: readonly SavingCoverageResult[];
  savingsMoneyAmountCents: bigint;
  overallNeededCents: bigint;
};
```

The result keeps the same `Saving` IDs and order as the input.

## Coverage Algorithm

Start `remainingCents` with the complete `mainMoneyAmountCents`, start `totalPlannedCents` at `ZERO_CENTS`, and process each `Saving` in input order.

For each `Saving`:

1. Add its planned money amount to `totalPlannedCents`.
2. `coveredCents` is the smaller of its planned money amount and `remainingCents`.
3. `neededCents` is its planned money amount minus `coveredCents`.
4. Subtract `coveredCents` from `remainingCents`.
5. Its state is:
   - `fully-covered` when `neededCents` is `ZERO_CENTS`;
   - `partly-covered` when both `coveredCents` and `neededCents` are greater than `ZERO_CENTS`; or
   - `not-covered` when `coveredCents` is `ZERO_CENTS`.

After every `Saving` is processed:

- `savingsMoneyAmountCents` equals `remainingCents` and can never be negative.
- `overallNeededCents` equals `totalPlannedCents - mainMoneyAmountCents` when that difference is positive; otherwise, it is `ZERO_CENTS`.

All calculations use integer `bigint` cents. Decimal floating-point arithmetic and rounding are not used. `bigint` also keeps the total exact when many planned money amounts together exceed JavaScript's safe integer range.

`bigint` values exist only in the derived in-memory calculation. They are never passed to `JSON.stringify()` or saved directly in browser storage. Saved money amounts remain normalized decimal strings, and the in-memory `bigint` values are rebuilt from validated saved information whenever coverage is recalculated.

## Needed Text

The shared money formatter accepts the non-negative `bigint` cents produced here and converts them to comma-separated text with exactly two decimal digits and the `$` sign after the amount.

- A `Saving` with `neededCents` greater than `ZERO_CENTS` shows `{formatted needed amount} needed`.
- A fully covered `Saving` has no needed text.
- `overallNeededCents` greater than `ZERO_CENTS` produces `{formatted overall amount} needed`.
- An `overallNeededCents` value of `ZERO_CENTS` produces no overall needed text.
- `savingsMoneyAmountCents` is always formatted as a money amount, including `0.00$`.

## Recalculation Rules

Coverage is derived in memory and is not saved as separate data.

Recalculate from the latest successfully kept information after:

- loading the current information;
- a successful main money amount change;
- a successful `Saving` creation, planned money amount change, deletion, fix, or reorder; or
- receiving newer successfully saved information from another open tab or window.

Renaming a `Saving` does not change the calculation. Unsaved, canceled, invalid, or failed actions do not replace the calculation input and therefore do not change coverage.

The calculator is a pure function. It does not write browser storage, change the main money amount, change a `Saving`, or create a `Balance Changes` entry or message.

## Essential Verification

Verify that:

- when there are no `Saving` records, `Savings money amount` equals the complete main money amount and no overall needed amount is produced;
- main money amount `100.00$` with ordered plans of `80.00$` and `50.00$` produces full coverage of `80.00$`, partial coverage of `20.00$`, `30.00$ needed` for the second `Saving`, `0.00$` as `Savings money amount`, and `30.00$ needed` overall;
- a zero main money amount makes every valid `Saving` not covered and leaves its complete planned money amount needed;
- planned money amounts equal to the main money amount produce `0.00$` as `Savings money amount` and no overall needed text;
- planned money amounts below the main money amount leave the exact difference as `Savings money amount` and produce no overall needed text;
- changing the order can change individual coverage results without changing `Savings money amount` or the overall amount needed;
- broken records and unsaved input do not participate in the calculation;
- failed and canceled actions leave the previous calculation result unchanged; and
- very large combined planned totals remain exact without floating-point rounding or safe-integer overflow.

## Boundaries

Saved-data validation, ordering interactions, storage writes, and presentation of the calculated result belong to their matching technical work.
