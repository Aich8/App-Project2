# FEATURE-SPEC-003: Modifying The Money Amount

Spec ID: `FEATURE-SPEC-003`

Spec type: `FeatureSpec`

Covers: silently correcting the complete main money amount when the tracked amount does not match the user's real cash.

Status: `Accepted`

## User Goal

The user can correct the complete main money amount so it matches the real cash they currently have.

`Modify` is a correction. It does not represent a normal increase or decrease in cash and must remain distinct from `Add` and `Subtract`.

## Availability

- `Modify` is available when the main money amount is any valid value from `0.00$` through `999,999.99$`.
- The user may correct the money amount whenever needed. The feature has no usage schedule or frequency limit.

## Action Rules

- The user chooses `Modify`, enters the corrected complete money amount, and confirms the action.
- The corrected money amount may be any value from `0.00$` through `999,999.99$`, including both limits.
- A valid confirmed amount replaces the existing main money amount; it is not added to or subtracted from that amount.
- The user may cancel before confirmation without changing anything.

## Same-Amount Case

- If the confirmed money amount is exactly the same as the main money amount already shown, nothing changes.
- No `Balance Changes`, activity, or notification entry is created, and no message appears.
- The Modify flow remains open with the entered value so the user can enter a different valid amount or cancel.

## Invalid Cases

- A negative money amount is not applied.
- A money amount above `999,999.99$` is not applied.
- An invalid attempt does not change the main money amount or create a `Balance Changes`, activity, or notification entry.

## Successful Result

- A valid corrected amount that differs from the current amount replaces the complete main money amount exactly.
- `0.00$` is a valid successful result when the current main money amount is greater than `0.00$`.
- The corrected main money amount is shown to the user.
- No `Balance Changes`, activity, or notification entry is created.
- The completed Modify flow closes, and no success message appears.

## Failure Result

- If the website cannot keep a valid Modify result, the main money amount remains unchanged.
- No `Balance Changes`, activity, or notification entry is created.
- The exact message `Changes could not be saved.` is shown.
- The Modify flow remains open with the entered value so the user can retry or cancel.

## Boundaries

- This spec does not define `Add` or `Subtract` behavior.
- It does not define the action-menu layout or the detailed money amount entry controls.
- It does not define the presentation, ordering, retention, or deletion of `Balance Changes`.
- It does not define storage, data structures, algorithms, source files, or implementation technology.

## Acceptance Expectations

- A different valid corrected amount replaces the complete main money amount exactly.
- A successful Modify never creates history, activity, or notification entries.
- Confirming the amount already shown is a silent no-action case that leaves the Modify flow open.
- Invalid, canceled, or failed Modify attempts do not change the main money amount.
