# FEATURE-SPEC-011: Deleting A Saving

Spec ID: `FEATURE-SPEC-011`

Spec type: `FeatureSpec`

Covers: permanently deleting one existing `Saving` from `Savings`.

Status: `Draft`

## User Goal

The user can delete a `Saving` they no longer want to keep.

Deleting a `Saving` does not change the main money amount.

## Starting Deletion

- The user first opens the actions for a `Saving` and then chooses `Delete`.
- Choosing `Delete` does not remove the `Saving` immediately.
- The exact question `Delete this Saving?` is shown.
- The confirmation provides the exact actions `Cancel` and `Delete`.
- A delete confirmation can be open for only one `Saving` at a time.

## Closing Without Deleting

- Choosing `Cancel` closes the confirmation without deleting the `Saving`.
- Clicking or tapping outside the confirmation closes it without deleting the `Saving`.
- The website's Back action and the browser/device Back action close the confirmation and keep the user inside `Savings`.
- The selected `Saving` and all other information remain unchanged.
- No `Balance Changes` entry is created, and no message is shown.
- An open confirmation is not kept after the website is refreshed, closed, or opened later.

## Successful Deletion

- Choosing `Delete` in the confirmation removes only the selected `Saving`.
- The confirmation closes and returns to the `Savings` view.
- The selected `Saving` name and planned money amount are removed.
- Deletion does not directly edit or reorder any remaining `Saving`.
- The complete remaining `Saving` list is checked again after deletion.
- An unchanged record that is now valid is shown as a normal `Saving`.
- An unchanged record that is still invalid remains a broken `Saving`.
- A record that becomes normal is shown according to its existing order.
- The information shown inside `Savings` updates using the resulting normal `Saving` squares.
- The main money amount remains unchanged.
- No `Balance Changes` entry is created, and no success message is shown.
- The deleted `Saving` cannot be restored with an undo action.

## Failure Result

- If the website cannot keep the deletion, the selected `Saving` remains unchanged.
- Other `Saving` squares and the main money amount remain unchanged.
- The confirmation stays open, so the user can try `Delete` again or choose `Cancel`.
- The exact message `Changes could not be saved.` is shown.
- The message remains visible without a timer while the failed confirmation remains unchanged.
- Choosing `Delete` to retry removes the old message while the retry is being attempted.
- A failed retry shows the message again, and a successful retry closes the confirmation.
- Choosing `Cancel`, clicking or tapping outside, or using either Back action removes the message by closing the confirmation.
- Refreshing, closing, reopening, or receiving newer saved information from another open tab or window discards the confirmation and its message.
- A later delete confirmation starts without the previous message.

## Acceptance Expectations

- Choosing `Delete` for a `Saving` asks for confirmation before removing it.
- Canceling, selecting outside, or using Back does not delete the `Saving`.
- A successful deletion removes only the selected `Saving`.
- The complete remaining `Saving` list is checked again, and each record is shown according to whether it is now normal or broken.
- The remaining information inside `Savings` updates after deletion.
- Deleting a `Saving` never changes the main money amount or creates a `Balance Changes` entry.
- A failed deletion keeps the selected `Saving` and its confirmation available.
- A save-failure message remains while its confirmation is unchanged, clears during retry or when the confirmation closes, and returns after another failed retry.
