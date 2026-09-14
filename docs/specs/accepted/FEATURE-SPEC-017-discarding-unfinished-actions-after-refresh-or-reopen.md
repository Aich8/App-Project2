# FEATURE-SPEC-017: Discarding Unfinished Actions After Refresh Or Reopen

Spec ID: `FEATURE-SPEC-017`

Spec type: `FeatureSpec`

Covers: discarding an unfinished action when the website is refreshed, closed, or opened later.

Status: `Accepted`

## User Goal

The user returns to the latest successfully saved information without an unfinished action being applied accidentally.

## Unfinished Actions

An unfinished action includes:

- Open main money actions.
- An open `Add`, `Subtract`, or `Modify` flow.
- Open `Balance Changes` delete actions or confirmation.
- Open actions for a `Saving`.
- An open `Saving` creation, rename, planned money amount change, or fix flow.
- An open `Saving` delete confirmation.
- A `Saving` reorder that has not been completed.

## Refreshing Or Returning Later

- Refreshing the website discards any unfinished action.
- Closing the website and opening it later discards any unfinished action.
- The unfinished action is not reopened automatically.
- Information entered into an unfinished action is not kept as a draft.
- Information from a failed save attempt is also discarded if the website is refreshed, closed, or opened later.

## Discarded Information

- An entered money amount is discarded.
- An entered `Saving` name or planned money amount is discarded.
- A pending deletion is canceled, and the selected item is not deleted.
- An unfinished reorder is canceled, and the last successfully saved order is used.
- Open money or `Saving` actions are closed.

## Information That Returns

- The website shows the latest successfully saved main money amount.
- The website shows the latest successfully saved `Balance Changes`.
- The website shows the latest successfully saved `Saving` information and order.
- Changes that were successfully saved before the refresh or close remain available.

## Result

- Discarding an unfinished action does not change any saved information.
- It does not create a `Balance Changes` entry.
- It does not show a message.
- The website does not show a leave warning only because an unfinished action is open.

## Acceptance Expectations

- Refreshing, closing, or reopening the website clears every unfinished action.
- Unsaved entered information is not restored as a draft.
- Pending deletions and unfinished reorders are not completed.
- Only the latest successfully saved information returns.
- Successfully saved changes are not lost.
- Discarding an unfinished action creates no `Balance Changes` entry, message, or leave warning.
