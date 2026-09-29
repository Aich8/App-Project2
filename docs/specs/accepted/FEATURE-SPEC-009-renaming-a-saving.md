# FEATURE-SPEC-009: Renaming A Saving

Spec ID: `FEATURE-SPEC-009`

Spec type: `FeatureSpec`

Covers: changing the name of one existing `Saving`.

Status: `Accepted`

## User Goal

The user can give an existing `Saving` a new name.

Renaming a `Saving` does not change its planned money amount.

## Starting Rename

- The user first opens the actions for a `Saving` and then selects its name.
- The rename flow asks only for a new `Saving` name.
- The name input starts with the selected `Saving`'s current saved name.
- The rename flow provides the exact actions `Save` and `Cancel`.
- The name changes only after a successful `Save`.

## New Name Rules

- The new name follows the Saving name rules in `FEATURE-SPEC-007: Creating A Saving`.
- The current `Saving` does not count as a duplicate of itself.
- A name already used by another `Saving` counts as a duplicate.

## Saving The Current Name

- If the user saves the current name without changing it, the rename flow closes.
- The `Saving` remains unchanged, and no message is shown.

## Invalid Save Attempts

- A missing name keeps the rename flow open.
- The old name remains unchanged, and no message is shown.
- A duplicate name closes the rename flow and returns to the `Savings` view.
- The old name remains unchanged, and no duplicate-name message is shown.

## Keeping Rename Open

- Clicking or tapping outside the rename flow does not close it.
- Scrolling does not close the rename flow.
- An unrelated action does not open while rename remains open.

## While Saving

- While one `Save` attempt is running, the entered name cannot be edited and choosing `Save` again does nothing.
- `Cancel`, the website's Back action, and browser/device Back remain available while a save attempt is waiting.
- Using one of those closing actions cancels any still-waiting save attempt and keeps the old name unchanged.

## Canceling Rename

- Choosing `Cancel` closes the rename flow.
- The website's Back action and the browser/device Back action close the rename flow and keep the user inside `Savings`.
- Choosing `Cancel` or using either Back action discards the unsaved name.
- The old name remains unchanged.
- No message is shown.
- An unsaved name is not kept after the website is refreshed, closed, or opened later.

## Successful Result

- The new name replaces the old name for the selected `Saving`.
- Spaces at the beginning and end of the new name are removed before it is saved.
- The rename flow closes and returns to the `Savings` view.
- The selected `Saving`'s planned money amount and saved order remain unchanged.
- After the rename succeeds, the website checks the complete saved `Saving` list again.
- If another `Saving` was broken only because it had the same name, it appears normally when all its saved information is now valid.
- A `Saving` that remains invalid for any reason stays broken.
- When another `Saving` becomes normal, the information and coverage shown inside `Savings` are recalculated from the complete valid list. Otherwise, the money amounts and coverage shown inside `Savings` remain unchanged.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.

## Failure Result

- If the website cannot keep the new name, the old name remains unchanged.
- The exact message `Changes could not be saved.` is shown.
- The rename flow stays open with the entered name, so the user can try `Save` again or choose `Cancel`.
- The message remains while the failed rename flow is unchanged.
- Changing the entered name removes the previous message.
- Choosing `Save` again removes the previous message while the new attempt is made.
- If the new attempt also fails, the message appears again.
- A successful retry closes the rename flow and removes the message.
- Canceling, using either Back action, refreshing, closing, reopening, or receiving newer saved information from another open tab or window removes the message by closing or discarding the flow.
- A later rename flow starts without the previous message.

## Acceptance Expectations

- Rename starts with the selected `Saving`'s current saved name.
- A valid new name replaces the old name for only the selected `Saving`.
- Saving the current name closes the rename flow without changing anything.
- A missing name keeps the rename flow open without changing the old name.
- A duplicate name closes the rename flow without changing the old name.
- Clicking or tapping outside and scrolling keep rename open.
- While a save is running, another name edit or `Save` selection is not accepted, while `Cancel` and Back remain available.
- Canceling or using Back keeps the old name.
- A successful rename checks the complete saved `Saving` list again and shows any other record that has become fully valid.
- Renaming never changes the main money amount or creates a `Balance Changes` entry.
- A failed rename keeps the entered name available for another attempt.
- The previous save-failure message disappears after a name edit, retry, or closure and appears again if the retry fails.
