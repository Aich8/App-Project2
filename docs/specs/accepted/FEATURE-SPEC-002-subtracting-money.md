# FEATURE-SPEC-002: Subtracting Money

Spec ID: `FEATURE-SPEC-002`

Spec type: `FeatureSpec`

Covers: manually decreasing the main money amount when the user's real cash decreases.

Status: `Accepted`

## User Goal

The user can record a decrease in their real cash by subtracting a money amount from the amount already tracked by the website.

`Subtract` represents a normal cash change. It does not replace or silently correct the full money amount.

## Availability

- `Subtract` is available only when the main money amount is greater than `0.00$`.
- `Subtract` is unavailable when the main money amount is `0.00$`.
- The user may subtract money rarely or many times in one day. The feature has no usage schedule or frequency limit.

## Action Rules

- The user chooses `Subtract`, enters the money amount to subtract, and confirms the action.
- The entered money amount must be greater than `0.00$` and no greater than `999,999.99$`.
- A valid confirmed amount lowers the existing main money amount; it does not replace that amount.
- The main money amount must never become negative.
- The user may cancel before confirmation without changing anything.

## Subtracting More Than Available

- If the entered money amount is greater than the main money amount, the main money amount becomes `0.00$`.
- Only the money amount actually removed is treated as subtracted.
- The matching `Balance Changes` entry uses the amount actually removed, not the larger entered amount.
- No warning or additional confirmation is required for this case.

## No-Action Cases

- Confirming `0.00$` does not change the main money amount.
- Confirming `0.00$` creates no `Balance Changes` entry and shows no message.
- After confirming `0.00$`, the Subtract flow remains open so the user can enter a valid amount or cancel.
- A negative amount or an entered amount above `999,999.99$` is not applied.
- Because `Subtract` is unavailable at `0.00$`, no subtraction or `Balance Changes` entry can be created from that state.

## Successful Result

- When the entered amount is no greater than the main money amount, the main money amount decreases by exactly the confirmed amount.
- When the entered amount is greater than the main money amount, the main money amount decreases by exactly the amount that was available and stops at `0.00$`.
- The updated main money amount is shown to the user.
- One separate negative `Balance Changes` entry is created as `-{money amount actually removed} subtracted`.
- The completed Subtract flow closes, and no success message appears.
- If the result is `0.00$`, `Subtract` is no longer available.

## Failure Result

- If the website cannot keep a valid Subtract result, the main money amount and `Balance Changes` remain unchanged.
- The exact message `Changes could not be saved.` is shown.
- The Subtract flow remains open with the entered value so the user can retry or cancel.

## Boundaries

- This spec does not define `Add` or `Modify` behavior.
- It does not define the action-menu layout or the detailed money amount entry controls.
- It does not define how `Balance Changes` is displayed, ordered, retained, or deleted.
- It does not define storage, data structures, algorithms, source files, or implementation technology.

## Acceptance Expectations

- A valid Subtract decreases the main money amount without allowing a negative result.
- Every successful Subtract creates exactly one matching negative `Balance Changes` entry for the amount actually removed.
- Invalid, canceled, or failed Subtract attempts do not change the main money amount or create history.
