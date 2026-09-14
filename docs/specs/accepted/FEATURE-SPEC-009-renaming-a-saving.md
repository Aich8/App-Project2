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

## Canceling Rename

- Choosing `Cancel` closes the rename flow.
- The website's Back action and the browser/device Back action close the rename flow and keep the user inside `Savings`.
- Choosing `Cancel` or using either Back action discards the unsaved name.
- The old name remains unchanged.
- No message is shown.
- An unsaved name is not kept after the website is refreshed, closed, or opened later.

## Successful Result

- The new name replaces the old name.
- Spaces at the beginning and end of the new name are removed before it is saved.
- The rename flow closes and returns to the `Savings` view.
- The planned money amount remains unchanged.
- The order of the `Saving` squares remains unchanged.
- The money amounts and coverage shown inside `Savings` remain unchanged.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.

## Failure Result

- If the website cannot keep the new name, the old name remains unchanged.
- The exact message `Changes could not be saved.` is shown.
- The rename flow stays open with the entered name, so the user can try `Save` again or choose `Cancel`.

## Acceptance Expectations

- A valid new name replaces the old name for only the selected `Saving`.
- Saving the current name closes the rename flow without changing anything.
- A missing name keeps the rename flow open without changing the old name.
- A duplicate name closes the rename flow without changing the old name.
- Canceling or using Back keeps the old name.
- Renaming never changes any money amount or creates a `Balance Changes` entry.
- A failed rename keeps the entered name available for another attempt.
