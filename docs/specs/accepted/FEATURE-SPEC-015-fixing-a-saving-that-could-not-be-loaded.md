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
- The name starts with no entered text, and the planned money amount starts with no entered digits.
- The fix flow does not reuse any name or planned money amount information from the broken record.
- The fix flow does not automatically enter `0` or `0.00`.

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
- Choosing `Save` before entering either value applies the missing planned money amount rule first, keeps the fix flow open, and shows no message.

## Canceling Fix

- Choosing `Cancel` closes the fix flow and returns to the broken `Saving`.
- The website's Back action and the browser/device Back action close the fix flow and keep the user inside `Savings`.
- Choosing `Cancel` or using either Back action discards the unsaved name and planned money amount.
- The broken `Saving` and all other information remain unchanged.
- No `Balance Changes` entry is created, and no message is shown.
- Unsaved fix information is not kept after the website is refreshed, closed, or opened later.

## Fix Target No Longer Available

- If the selected broken `Saving` is no longer present in the latest loaded information, the fix flow closes and discards its entered information.
- The website does not attempt to save the fix.
- The latest successfully saved information is shown.
- No message or `Balance Changes` entry is created.

## Successful Fix

- The broken `Saving` becomes a normal `Saving` with the saved name and planned money amount.
- It keeps its previous place among the other `Saving` squares when possible.
- The fix flow closes and returns to the `Savings` view.
- `Savings money amount`, the overall amount needed, and coverage update using the fixed `Saving`.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.
- The complete saved `Saving` list is checked again after the fix succeeds.
- Another unchanged record that is now valid is shown as a normal `Saving`.
- Another unchanged record that is still invalid remains a broken `Saving`.

## Failure Result

- If the website cannot keep the fixed `Saving`, it remains broken.
- The other saved information remains unchanged.
- The exact message `Changes could not be saved.` is shown.
- The fix flow stays open with the entered name and planned money amount, so the user can try `Save` again or choose `Cancel`.
- The message remains while the user leaves the failed fix flow unchanged.
- Changing the entered name or making an accepted change to the entered planned money amount removes the previous message.
- Rejected planned money amount input does not remove the message because it does not change the entered amount.
- Choosing `Save` again removes the previous message while the new attempt is made.
- If the new attempt also fails, the message appears again.
- A successful retry closes the fix flow and removes the message.
- Canceling, using either Back action, refreshing, closing, reopening, or receiving newer saved information from another open tab or window removes the message by closing or discarding the flow.
- A later fix flow starts without the previous message.

## Acceptance Expectations

- One broken `Saving` does not prevent the rest of the saved information from being used.
- A broken `Saving` shows `Saving could not be loaded.`, `Fix`, and `Delete`.
- A broken `Saving` does not affect money amounts or coverage and cannot be reordered.
- A new fix flow starts without entered name or planned money amount information from the broken record.
- Valid fix information changes the broken `Saving` into a normal `Saving`.
- A missing or invalid planned money amount, or a missing name, keeps the fix flow open without changing saved information.
- A duplicate name closes the fix flow and leaves the `Saving` broken.
- Canceling or using Back leaves the `Saving` broken and discards unsaved fix information.
- A fix whose selected broken `Saving` is no longer available closes without saving and shows the latest successfully saved information.
- A failed fix keeps the entered information available for another attempt.
- The previous save-failure message disappears after an accepted edit, a retry, or closure, and appears again if the retry fails.
- A successful fix checks the complete saved `Saving` list again and shows every record according to whether it is now valid or broken.
- Fixing a `Saving` never changes the main money amount or creates a `Balance Changes` entry.
