# FEATURE-SPEC-018: Updating Other Open Tabs And Windows

Spec ID: `FEATURE-SPEC-018`

Spec type: `FeatureSpec`

Covers: automatically showing the latest successfully saved information in other open copies of the website in the same browser.

Status: `Accepted`

## User Goal

The user sees consistent information when the website is open in multiple tabs or windows of the same browser.

The user does not need to refresh another open tab or window after saving a change.

## When An Update Happens

- A successful saved change in one tab or window updates the other open tabs and windows automatically.
- A canceled, invalid, no-action, or failed change does not update the other tabs or windows.
- This behavior applies only to open copies of the same website in the same browser.

## Information That Updates

- The main money amount updates to the latest successfully saved value.
- `Balance Changes` updates to the latest successfully saved entries.
- `Saving` squares, their information, and their order update.
- `Savings money amount`, the overall amount needed, and coverage update using the latest information.
- Saved-data recovery results also update the other open tabs and windows.

## Receiving The Update

- A tab or window with no unfinished action shows the latest information automatically.
- The update happens without a message.
- The update itself is not treated as another user action.
- The update creates no additional `Balance Changes` entry.

## Receiving An Update During An Unfinished Action

- Any unfinished action in the receiving tab or window closes as if the user canceled it.
- Unsaved entered information is discarded.
- The pending deletion in the receiving tab or window is not carried out.
- An unfinished reorder is canceled.
- The receiving tab or window does not save any part of its unfinished action.
- The latest successfully saved information from the other tab or window is shown.
- No message or additional `Balance Changes` entry is created.

## Final Result

- All open tabs and windows show the same latest successfully saved information.
- Information from an unfinished action never replaces the newer saved information.
- The user can start a new action after the update is complete.

## Acceptance Expectations

- A successful saved change automatically updates other open copies of the website in the same browser.
- Main money amount, `Balance Changes`, `Savings`, and coverage information all update together.
- An update creates no message or additional `Balance Changes` entry.
- A receiving tab or window silently cancels any unfinished action and discards its unsaved information.
- Canceled, invalid, no-action, and failed changes do not update other tabs or windows.
- All open copies finish with the latest successfully saved information.
