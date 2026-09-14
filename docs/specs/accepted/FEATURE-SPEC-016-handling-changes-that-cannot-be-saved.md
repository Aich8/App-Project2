# FEATURE-SPEC-016: Handling Changes That Cannot Be Saved

Spec ID: `FEATURE-SPEC-016`

Spec type: `FeatureSpec`

Covers: protecting the user's previously saved information when the website cannot keep a requested change.

Status: `Accepted`

## User Goal

The user can clearly understand when a requested change did not succeed.

The website must not show or keep a requested change as successful when it could not be saved.

## When This Behavior Applies

- This behavior applies when a valid action should change saved information but the website cannot keep the change.
- It applies to main money amount changes, `Balance Changes` deletion, `Saving` changes, and saved-data recovery.
- The failure message is not shown for viewing information, canceling an action, entering invalid information, or completing a no-action case.

## Shared Failure Result

- A requested change succeeds only when the website can keep its complete result.
- If the result cannot be kept, no part of the requested change is applied.
- The main money amount, `Balance Changes`, and `Savings` remain based on the last successfully saved information.
- The exact message `Changes could not be saved.` is shown.
- No success message is shown.
- The failed action creates no new `Balance Changes` entry.

## Money Amount Input Flows

- A failed `Add`, `Subtract`, or `Modify` leaves the selected flow open.
- The entered money amount remains available.
- The user can try the action again or cancel it.
- The main money amount and `Balance Changes` remain unchanged.

## Saving Input Flows

- A failed `Saving` creation, rename, planned money amount change, or fix leaves its flow open.
- The entered name or planned money amount remains available.
- The user can try `Save` again or choose `Cancel`.
- Existing `Saving` information remains unchanged.

## Delete Confirmations

- A failed `Balance Changes` deletion or `Saving` deletion leaves its confirmation open.
- The selected item remains visible and is not deleted.
- The user can try `Delete` again or choose `Cancel`.

## Saving Reorder

- A failed reorder returns every `Saving` to the last successfully saved order.
- Names, planned money amounts, coverage, and money amounts return to their previous state.
- The user can start another reorder attempt.

## Start Again Recovery

- A failed `Start again` keeps the saved-data recovery state open.
- The unreadable saved information remains unchanged.
- The `Start again` action remains available for another attempt.

## Retrying Or Canceling

- A successful retry completes the original action using that action's accepted rules.
- Canceling after a failed attempt keeps the last successfully saved information unchanged.

## Acceptance Expectations

- A change is never presented as successful unless its complete result can be kept.
- The exact message `Changes could not be saved.` is shown after every failed attempt to save a change.
- A failed input action keeps its entered information available for another attempt.
- A failed deletion keeps the selected item and confirmation available.
- A failed reorder restores the previous order.
- A failed `Start again` keeps the recovery state and unreadable information.
- Failed changes do not alter saved information or create `Balance Changes` entries.
