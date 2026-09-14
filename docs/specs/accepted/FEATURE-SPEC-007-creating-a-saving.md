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
- The new `Saving` becomes available to review inside `Savings`.
- The creation flow closes and returns to the `Savings` view.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.

## Failure Result

- If the website cannot keep a valid new `Saving`, no `Saving` is created.
- Existing `Saving` squares and the main money amount remain unchanged.
- The exact message `Changes could not be saved.` is shown.
- The creation flow stays open with the entered name and money amount, so the user can try `Save` again or choose `Cancel`.

## Acceptance Expectations

- A valid name and planned money amount create exactly one new `Saving`.
- The name is saved without surrounding spaces and is unique inside `Savings`.
- Invalid planned money amounts and missing names keep creation open without creating a `Saving`.
- A duplicate name closes creation without creating a `Saving`.
- Canceling or leaving the creation flow keeps existing information unchanged.
- Creating a `Saving` never changes the main money amount or creates a `Balance Changes` entry.
- A failed creation keeps the entered information available for another attempt.
