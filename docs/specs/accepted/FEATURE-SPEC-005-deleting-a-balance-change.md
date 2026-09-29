# FEATURE-SPEC-005: Deleting A Balance Change

Spec ID: `FEATURE-SPEC-005`

Spec type: `FeatureSpec`

Covers: deleting one visible `Balance Changes` entry without changing the main money amount.

Status: `Accepted`

## User Goal

The user can permanently delete one `Balance Changes` entry they no longer want to keep.

Deleting an entry changes only `Balance Changes`. It does not change the main money amount.

## Opening Delete Actions

- A delete action can be opened for any visible entry.
- On a touch screen, the user presses and holds the entry for `600ms`.
- With a mouse, the user clicks and holds the entry for `600ms`.
- Completing the hold provides the exact actions `Delete` and `Cancel`.
- Delete actions can be open for only one entry at a time.

## Canceling A Hold

- Releasing before `600ms` cancels the hold.
- Moving the pointer or finger before `600ms` cancels the hold.
- Starting to scroll before `600ms` cancels the hold.
- A canceled hold does not change or delete anything.
- A canceled hold shows no message.

## Closing Delete Actions

- Choosing `Cancel` closes the delete actions without changing anything.
- Clicking or tapping outside closes the delete actions without changing anything.
- Browser Back closes the delete actions without leaving the dashboard or changing anything.
- Pointer or finger movement does not close the delete actions after they are open.
- Scrolling does not close the delete actions after they are open.

## Delete Confirmation

- Choosing `Delete` closes the delete actions and asks `Delete this Balance Change?`.
- The confirmation provides the exact actions `Cancel` and `Delete`.
- Choosing `Cancel` closes the confirmation without deleting the entry.
- Clicking or tapping outside closes the confirmation without deleting the entry.
- Browser Back closes the confirmation without leaving the dashboard or deleting the entry.
- If the selected entry reaches its normal expiration and is no longer visible before the user confirms or retries, the stale confirmation closes without saving, deleting anything else, or showing a message.

## Successful Deletion

- Choosing `Delete` in the confirmation removes only the selected entry.
- Other `Balance Changes` entries remain unchanged.
- The main money amount remains unchanged.
- The deletion does not create a new `Balance Changes` entry.
- The deleted entry cannot be restored with an undo action.

## Failure Result

- If the website cannot keep the deletion, the selected entry remains in `Balance Changes`.
- Other entries and the main money amount remain unchanged.
- The confirmation remains open so the user can retry or cancel.
- The exact message `Changes could not be saved.` is shown.
- The message remains visible without a timer while the failed confirmation remains unchanged.
- Choosing `Delete` to retry removes the old message while the retry is being attempted.
- A failed retry shows the message again, and a successful retry closes the confirmation.
- Choosing `Cancel`, clicking or tapping outside, or using Browser Back removes the message by closing the confirmation.
- Refreshing, closing, reopening, or receiving newer information from another tab or window discards the confirmation and its message. A later new confirmation starts without the old message.

## Acceptance Expectations

- A completed `600ms` hold opens `Delete` and `Cancel` for the selected entry.
- An interrupted hold does not open the delete actions or change anything.
- Canceling or leaving either deletion step does not delete the entry.
- The user must confirm before an entry is deleted.
- A successful deletion removes only the selected entry and does not change the main money amount.
- A failed deletion leaves the selected entry and the main money amount unchanged.
- A save-failure message remains while its confirmation is unchanged, clears during retry or when the confirmation closes, and returns after another failed retry.
- Confirming or retrying after the selected entry has expired closes the stale confirmation without saving or showing a message.
