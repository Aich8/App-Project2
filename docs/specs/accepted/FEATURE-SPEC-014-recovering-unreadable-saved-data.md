# FEATURE-SPEC-014: Recovering Unreadable Saved Data

Spec ID: `FEATURE-SPEC-014`

Spec type: `FeatureSpec`

Covers: letting the user start with empty information when the website cannot load its saved data as a whole.

Status: `Accepted`

## User Goal

The user can recover access to the website when its saved information cannot be loaded.

Recovery starts the website again with empty information.

## When Recovery Appears

- The recovery state appears when the website cannot load its saved information as a usable whole.
- The website does not show information that it cannot load reliably.
- The exact message `Saved data could not be loaded.` is shown.
- The exact action `Start again` is available.
- The normal dashboard is not available until recovery succeeds.

## When Recovery Does Not Appear

- A user with valid saved information does not see `Start again`.
- A user with no saved information sees the normal dashboard with the main money amount at `0.00$`.
- `Start again` is not available as a normal reset or clear-all action.
- One broken item does not cause full recovery when the rest of the saved information can still be loaded.

## Choosing Start Again

- Choosing `Start again` immediately attempts recovery without another confirmation.
- Successful recovery permanently removes all of the website's previously saved information.
- The dashboard opens with the main money amount at `0.00$`.
- `Balance Changes` is empty.
- No `Saving` squares exist.
- No `Balance Changes` entry or success message is created.
- The removed information cannot be restored with an undo action.

## Returning Later

- Closing and reopening the website before recovery succeeds shows the recovery state again.
- After successful recovery, closing and reopening the website shows the new empty information.

## Failure Result

- If the website cannot keep the new empty information, recovery does not happen.
- The previously saved unreadable information remains unchanged.
- The recovery state remains open with `Start again` available.
- The exact message `Changes could not be saved.` is shown.
- The user can choose `Start again` to try again.

## Acceptance Expectations

- Unreadable saved information shows `Saved data could not be loaded.` and `Start again`.
- Valid saved information and a normal empty state do not show `Start again`.
- Successful recovery opens an empty dashboard with `0.00$`, no `Balance Changes`, and no `Saving` squares.
- Recovery permanently removes the unreadable saved information without creating a history entry.
- Failed recovery keeps the recovery state and unreadable information unchanged.
- A failed recovery shows `Changes could not be saved.` and allows another attempt.
