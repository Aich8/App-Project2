# FEATURE-SPEC-010: Changing The Planned Money Amount Of A Saving

Spec ID: `FEATURE-SPEC-010`

Spec type: `FeatureSpec`

Covers: changing the planned money amount of one existing `Saving`.

Status: `Accepted`

## User Goal

The user can change how much money they want to set aside for an existing `Saving`.

This change does not move cash or change the main money amount.

## Starting The Change

- The user first opens the actions for a `Saving` and then selects its planned money amount.
- The change flow asks the user to enter the new planned money amount.
- The change flow provides the exact actions `Save` and `Cancel`.
- The planned money amount changes only after a successful `Save`.

## Planned Money Amount Rules

- A new planned money amount is required.
- It may be any amount from `0.00$` through `999,999.99$`.
- It may include cents with no more than two digits after the decimal point.
- It can be greater than the main money amount.
- The new amount replaces the old planned money amount.
- The new amount is not added to or subtracted from the old planned money amount.

## Entering The New Amount

- The user can enter digits and one decimal point.
- Letters, `$` signs, comma separators, a second decimal point, and more than two digits after the decimal point are not entered.
- Spaces cannot be entered in the planned money amount.
- Pasted content is not entered.
- Rejected typing and pasted content do not change the entered amount or show a message.
- The entered amount is shown as decimal number text while the user is typing.
- The `$` sign, comma separators, and two-digit decimal formatting are applied after a successful `Save`.

## Saving The Current Amount

- If the user saves the planned money amount that the `Saving` already has, the change flow closes.
- The `Saving` remains unchanged, and no message is shown.

## Invalid Save Attempts

- A missing planned money amount keeps the change flow open.
- An amount greater than `999,999.99$` keeps the change flow open.
- The old planned money amount remains unchanged, and no message is shown.

## Saving A Nonzero Amount

- The new amount replaces the old planned money amount.
- The change flow closes and returns to the `Savings` view.
- The `Saving` name and its place among the other `Saving` squares remain unchanged.
- The money amount, needed amount, and coverage shown inside `Savings` update using the new planned money amount.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.

## Saving Zero

- Saving `0.00$` removes the selected `Saving` from `Savings`.
- No separate delete confirmation is shown.
- The information shown inside `Savings` updates using the remaining `Saving` squares.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.

## Canceling The Change

- Choosing `Cancel` closes the change flow.
- The website's Back action and the browser/device Back action close the change flow and keep the user inside `Savings`.
- Choosing `Cancel` or using either Back action discards the unsaved amount.
- The old planned money amount remains unchanged.
- No message is shown.
- An unsaved amount is not kept after the website is refreshed, closed, or opened later.

## Failure Result

- If the website cannot keep the new planned money amount, the old planned money amount remains unchanged.
- The selected `Saving` is not removed when saving `0.00$` fails.
- The exact message `Changes could not be saved.` is shown.
- The change flow stays open with the entered amount, so the user can try `Save` again or choose `Cancel`.

## Acceptance Expectations

- A valid new planned money amount replaces the old planned money amount for only the selected `Saving`.
- Saving the current amount closes the change flow without changing anything.
- Saving `0.00$` removes the selected `Saving`.
- A missing or above-limit amount keeps the change flow open without changing the old amount.
- Canceling or using Back keeps the old planned money amount.
- A successful change updates the money amount, needed amount, and coverage shown inside `Savings`.
- Changing a planned money amount never changes the main money amount or creates a `Balance Changes` entry.
- A failed change keeps the entered amount available for another attempt.
