# FEATURE-SPEC-007: Creating A Saving

Spec ID: `FEATURE-SPEC-007`

Spec type: `FeatureSpec`

Covers: creating one new `Saving` square named by the user with a planned money amount inside `Savings`.

Status: `Accepted`

## User Goal

The user can create a `Saving` to plan how much money to set aside for a specific purpose, such as rent or a vacation.

A `Saving` records a name and a planned money amount. It does not move cash or change the main money amount.

## Starting Creation

- The user starts creating a `Saving` by choosing the `+` action inside `Savings`.
- The creation flow asks for a `Saving` name and a planned money amount.
- The creation flow provides the exact actions `Save` and `Cancel`.
- A new `Saving` is created only after a successful `Save`.

## Saving Name Rules

- A `Saving` name is required.
- Spaces at the beginning and end of the name are removed before saving.
- Spaces inside the name are kept.
- A name containing only spaces counts as missing.
- A one-character name is valid.
- A number-only name is valid.
- A name can contain letters, numbers, spaces, symbols, punctuation, and emoji characters.
- A name has no maximum length.
- Each `Saving` name must be unique inside `Savings`.
- Duplicate-name checks ignore uppercase and lowercase differences.
- Duplicate-name checks also ignore spaces at the beginning and end of a name.

## Planned Money Amount Rules

- Every `Saving` must have a money amount. This amount shows how much money the user wants to set aside for that `Saving`.
- It must be greater than `0.00$`.
- It must not be greater than `999,999.99$`.
- It may include cents with no more than two digits after the decimal point.
- It can be greater than the main money amount.
- A `Saving` can be for more money than the user currently has. The remaining part is shown as still needed.
- A `Saving` is only a plan. Creating it does not change the main money amount.

## Entering The Planned Money Amount

- The planned money amount uses the same typing and paste rules as the `Entering The New Amount` section of [`FEATURE-SPEC-010: Changing The Planned Money Amount Of A Saving`](FEATURE-SPEC-010-changing-the-planned-money-amount-of-a-saving.md#entering-the-new-amount).
- Creation still uses this spec's saved-value rules: `0.00$` is invalid and does not create a `Saving`.

## Invalid Save Attempts

- The planned money amount is checked before the `Saving` name.
- A missing or invalid planned money amount keeps the creation flow open.
- A missing name keeps the creation flow open.
- These invalid attempts create no `Saving` and show no message.
- When both the planned money amount and name are invalid, the planned money amount rule happens first.
- A duplicate name closes the creation flow and returns to the `Savings` view.
- A duplicate name creates no `Saving` and shows no message.

## Canceling Creation

- Choosing `Cancel` closes the creation flow.
- The website's Back action and the browser/device Back action close the creation flow and keep the user inside `Savings`.
- Choosing `Cancel` discards the unsaved name and planned money amount.
- Both the website's Back action and the browser/device Back action also discard this unsaved information.
- No `Saving` is created.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no message is shown.
- Unsaved creation information is not kept after the website is refreshed, closed, or opened later.

## Successful Result

- One new `Saving` square is created with the saved name and planned money amount.
- The name is saved after spaces at its beginning and end are removed.
- The new `Saving` is placed after all existing `Saving` squares.
- Existing `Saving` squares keep their current order.
- The new `Saving` has the last coverage priority until the user reorders it.
- The creation flow closes and returns to the `Savings` view.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.

## Failure Result

- If the website cannot keep a valid new `Saving`, no `Saving` is created.
- Existing `Saving` squares and the main money amount remain unchanged.
- The exact message `Changes could not be saved.` is shown.
- The creation flow stays open with the entered name and money amount, so the user can try `Save` again or choose `Cancel`.
- The message remains while the user leaves the failed creation flow unchanged.
- Changing the entered name or making an accepted change to the entered planned money amount removes the previous message.
- Rejected planned money amount input does not remove the message because it does not change the entered amount.
- Choosing `Save` again removes the previous message while the new attempt is made.
- If the new attempt also fails, the message appears again.
- A successful retry closes the creation flow and removes the message.
- Canceling, using either Back action, refreshing, closing, reopening, or receiving newer saved information from another open tab or window removes the message by closing or discarding the flow.
- A later new creation flow starts without the previous message.

## Acceptance Expectations

- A valid name and planned money amount create exactly one new `Saving`.
- The name is saved without surrounding spaces and is unique inside `Savings`.
- Planned money amount entry follows the same typing and paste rules as changing a planned money amount, while creation still rejects `0.00$`.
- Invalid planned money amounts and missing names keep creation open without creating a `Saving`.
- A duplicate name closes creation without creating a `Saving`.
- A new `Saving` is last in the existing order and has the last coverage priority until reordered.
- Canceling or leaving the creation flow keeps existing information unchanged.
- Creating a `Saving` never changes the main money amount or creates a `Balance Changes` entry.
- A failed creation keeps the entered information available for another attempt.
- The previous save-failure message disappears after an accepted edit, a retry, or closure, and appears again if the retry fails.
