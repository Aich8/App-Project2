# FEATURE-SPEC-001: Adding Money

Spec ID: `FEATURE-SPEC-001`

Spec type: `FeatureSpec`

Covers: manually increasing the main money amount when the user's real cash increases.

Status: `Accepted`

## User Goal

The user can record an increase in their real cash by adding a money amount to the amount already tracked by the website.

`Add` represents a normal cash change. It does not replace or silently correct the full money amount.

## Availability

- `Add` is available when the main money amount is less than `999,999.99$`, including when it is `0.00$`.
- `Add` is unavailable when the main money amount is exactly `999,999.99$`.
- The user may add money rarely or many times in one day. The feature has no usage schedule or frequency limit.

## Action Rules

- The user chooses `Add`, enters the money amount to add, and confirms the action.
- The entered money amount must be greater than `0.00$` and no greater than `999,999.99$`.
- The resulting main money amount must not be greater than `999,999.99$`.
- A valid confirmed amount is added to the existing main money amount; it does not replace that amount.
- The user may cancel before confirmation without changing anything.

## No-Action Cases

- Confirming `0.00$` does not change the main money amount.
- Confirming `0.00$` creates no `Balance Changes` entry and shows no message.
- After confirming `0.00$`, the Add flow remains open so the user can enter a valid amount or cancel.
- A negative amount or an entered amount above `999,999.99$` is not applied.
- If an otherwise valid entered amount would make the resulting main money amount exceed `999,999.99$`, it is not applied.
- For that over-limit result, the main money amount remains unchanged, no `Balance Changes` entry or message appears, and the Add flow stays open with the entered value so the user can correct it or cancel.

## Successful Result

- The main money amount increases by exactly the confirmed amount.
- The updated main money amount is shown to the user.
- One separate positive `Balance Changes` entry is created as `+{money amount} added`.
- The completed Add flow closes, and no success message appears.
- If the result is exactly `999,999.99$`, `Add` is no longer available.

## Failure Result

- If the website cannot keep a valid Add result, the main money amount and `Balance Changes` remain unchanged.
- The exact message `Changes could not be saved.` is shown.
- The Add flow remains open with the entered value so the user can retry or cancel.

## Boundaries

- This spec does not define `Subtract` or `Modify` behavior.
- It does not define the action-menu layout or the detailed money amount entry controls.
- It does not define how `Balance Changes` is displayed, ordered, retained, or deleted.
- It does not define storage, data structures, algorithms, source files, or implementation technology.

## Acceptance Expectations

- A valid Add increases the main money amount by the entered amount without exceeding `999,999.99$`.
- Every successful Add creates exactly one matching positive `Balance Changes` entry.
- Invalid, canceled, or failed Add attempts do not change the main money amount or create history.
