# FEATURE-SPEC-013: Viewing Savings Coverage

Spec ID: `FEATURE-SPEC-013`

Spec type: `FeatureSpec`

Covers: showing how the main money amount covers the planned money amounts in `Savings`.

Status: `Accepted`

## User Goal

The user can see which `Saving` plans are fully covered, partly covered, or not covered by their current tracked cash.

Coverage is planning information. It does not move cash or change the main money amount.

## Meaning Of Coverage

- A `Saving` is fully covered when enough money remains in the coverage order for its complete planned money amount.
- A `Saving` is partly covered when some money remains for it, but not enough for its complete planned money amount.
- A `Saving` is not covered when no money remains for it in the coverage order.
- Being covered does not move or reserve cash for a `Saving`.

## Coverage Order

- Coverage starts with the complete main money amount.
- `Saving` squares are checked from top to bottom.
- Each `Saving` is covered as much as possible before the next `Saving` is checked.
- A lower `Saving` uses only the amount left after the `Saving` squares above it are covered.
- Changing the order can change which `Saving` squares are fully, partly, or not covered.

## Coverage Information

- Every `Saving` shows its covered state.
- A partly covered or not covered `Saving` shows how much more money it needs.
- This information uses the exact format `{money amount} needed`.
- A fully covered `Saving` does not show an amount still needed.
- The amount still needed is the part of the planned money amount that is not covered.

## Savings Money Amount

- `Savings money amount` shows how much of the main money amount remains after all `Saving` plans are considered.
- When no `Saving` squares exist, `Savings money amount` equals the main money amount.
- `Savings money amount` never goes below `0.00$`.
- When the planned money amounts equal or exceed the main money amount, `Savings money amount` is `0.00$`.
- A `Savings money amount` of `0.00$` is normal planning information, not an error or warning.

## Overall Amount Needed

- When all planned money amounts together are greater than the main money amount, `Savings` shows the difference.
- The difference uses the exact format `{money amount} needed`.
- No overall amount needed is shown when all planned money amounts together equal or are less than the main money amount.

## Example

When the main money amount is `100.00$`, a top `Saving` of `80.00$` is fully covered.

A second `Saving` of `50.00$` is partly covered by the remaining `20.00$` and shows `30.00$ needed`.

In this example, `Savings money amount` is `0.00$`, and the overall amount needed is `30.00$ needed`.

## Updating Coverage

- Coverage updates after a successful change to the main money amount.
- Coverage updates after a successful creation, planned money amount change, deletion, or reorder of a `Saving`.
- Renaming a `Saving` does not change coverage.
- Unsaved `Saving` information does not change or preview coverage.
- A canceled or failed action leaves coverage based on the last successfully kept information.
- Updating coverage does not create an additional `Balance Changes` entry or show a message.

## Acceptance Expectations

- `Saving` squares are covered from top to bottom using the main money amount.
- Each `Saving` is shown as fully covered, partly covered, or not covered.
- A `Saving` that is not fully covered shows its own amount still needed.
- `Savings money amount` shows what remains and never becomes negative.
- `Savings` shows an overall amount needed only when the total planned money amount exceeds the main money amount.
- Coverage updates only from successfully kept information.
- Coverage never moves cash or changes the main money amount.
