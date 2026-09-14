# FEATURE-SPEC-015: Fixing A Saving That Could Not Be Loaded

Spec ID: `FEATURE-SPEC-015`

Spec type: `FeatureSpec`

Covers: repairing one saved `Saving` that the website cannot load normally.

Status: `Accepted`

## User Goal

The user can repair one broken `Saving` without losing the rest of their saved information.

## Broken Saving State

- A broken `Saving` does not cause the full saved-data recovery state when the rest of the saved information can be loaded.
- The broken `Saving` remains visible inside `Savings`.
- The exact message `Saving could not be loaded.` is shown.
- It provides the exact actions `Fix` and `Delete`.
- Choosing `Delete` uses the confirmation and deletion rules in `FEATURE-SPEC-011: Deleting A Saving`.
- The other saved information remains available.

## While A Saving Is Broken

- A broken `Saving` does not affect `Savings money amount`, the overall amount needed, or the coverage of other `Saving` squares.
- A broken `Saving` does not show coverage information.
- A broken `Saving` cannot be reordered until it is fixed or deleted.
- Other normal `Saving` squares can still be reordered while the broken `Saving` keeps its place.
- Showing a broken `Saving` does not change the main money amount or create a `Balance Changes` entry.

## Starting Fix

- Choosing `Fix` starts repairing that `Saving`.
- The fix flow asks the user to enter a `Saving` name and planned money amount.
- The fix flow provides the exact actions `Save` and `Cancel`.
- The `Saving` remains broken until a successful `Save`.

## Fix Information Rules

- The name follows the rules in `FEATURE-SPEC-007: Creating A Saving`.
- The planned money amount follows the rules in `FEATURE-SPEC-007: Creating A Saving`.
- A readable name belonging to another broken `Saving` is still considered in use.
- The broken `Saving` being fixed does not count as a duplicate of itself.

## Invalid Save Attempts

- The planned money amount is checked before the name.
- A missing planned money amount keeps the fix flow open.
- A planned money amount of `0.00$` or greater than `999,999.99$` keeps the fix flow open.
- A missing name keeps the fix flow open.
- These invalid attempts leave the `Saving` broken and show no message.
- When the planned money amount is invalid, the fix flow stays open even if the name is a duplicate.
- A duplicate name closes the fix flow and returns to the `Savings` view.
- A duplicate name leaves the `Saving` broken and shows no message.

## Canceling Fix

- Choosing `Cancel` closes the fix flow and returns to the broken `Saving`.
- The website's Back action and the browser/device Back action close the fix flow and keep the user inside `Savings`.
- Choosing `Cancel` or using either Back action discards the unsaved name and planned money amount.
- The broken `Saving` and all other information remain unchanged.
- No `Balance Changes` entry is created, and no message is shown.
- Unsaved fix information is not kept after the website is refreshed, closed, or opened later.

## Successful Fix

- The broken `Saving` becomes a normal `Saving` with the saved name and planned money amount.
- It keeps its previous place among the other `Saving` squares when possible.
- The fix flow closes and returns to the `Savings` view.
- `Savings money amount`, the overall amount needed, and coverage update using the fixed `Saving`.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.

## Failure Result

- If the website cannot keep the fixed `Saving`, it remains broken.
- The other saved information remains unchanged.
- The exact message `Changes could not be saved.` is shown.
- The fix flow stays open with the entered name and planned money amount, so the user can try `Save` again or choose `Cancel`.

## Acceptance Expectations

- One broken `Saving` does not prevent the rest of the saved information from being used.
- A broken `Saving` shows `Saving could not be loaded.`, `Fix`, and `Delete`.
- A broken `Saving` does not affect money amounts or coverage and cannot be reordered.
- Valid fix information changes the broken `Saving` into a normal `Saving`.
- A missing or invalid planned money amount, or a missing name, keeps the fix flow open without changing saved information.
- A duplicate name closes the fix flow and leaves the `Saving` broken.
- Canceling or using Back leaves the `Saving` broken and discards unsaved fix information.
- A failed fix keeps the entered information available for another attempt.
- Fixing a `Saving` never changes the main money amount or creates a `Balance Changes` entry.
