# Functional Spec: Cash Money Organizer Website

## Starting State And Money Amount Controls

When a new user has no saved data yet, the website should show the dashboard with the money amount set to `0.00$`.

Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`. Visible money amounts of `1,000.00$` or more should use comma separators every three digits before the decimal point, starting from the right, such as `5,895.50$` and `999,999.99$`.

Saved browser storage money amount values should use normalized plain decimal strings without the `$` sign or comma separators, such as `0.00`, `5.00`, `5895.50`, and `999999.99`. User-facing website display should still add comma separators and the `$` sign, such as `5,895.50$`.

Money amount values can include cents, but the maximum allowed money amount is `999,999.99$`. The main money amount and each `Saving` square planned money amount should never be saved above `999,999.99$`. Examples of valid visible money amounts include `5.05$`, `0.05$`, and `999,999.99$`. Values above `999,999.99$` are invalid.

Showing the default `0.00$` money amount should not create saved browser data by itself.

The default `0.00$` money amount should not create a `Balance Changes` entry.

The dashboard should show the main money amount with the user-facing label `Current Balance`.

The dashboard should show the exact website name and short trust label as `Manual Cash Tracker`.

In the first version, clickable controls should be used by mouse click or finger tap.

Keyboard use is required only for typing inside money amount inputs and `Saving` square inputs.

The specified `Space` key behavior inside main money amount inputs should still work.

The first version does not need custom keyboard navigation for clickable controls.

The first version does not need custom `Tab` order rules.

The first version does not need `Enter` or `Space` activation rules for clickable controls.

The first version does not need focus-return rules after save, cancel, or delete.

The first version does not need keyboard support for reordering `Saving` squares.

The main money amount should be clickable.

When the money amount is `0.00$` and the user clicks the main money amount, the website should show two actions near the main money amount:

- `Add` for adding money to the main money amount.
- `Modify` for correcting the total displayed amount.

At `0.00$`, the website should not show `Subtract` because the main money amount cannot go below `0.00$`.

When the money amount is greater than `0.00$` and less than `999,999.99$` and the user clicks the main money amount, the website should show three actions near the main money amount:

- `Add` for adding money to the main money amount.
- `Subtract` for subtracting money from the main money amount.
- `Modify` for correcting the total displayed amount.

When the money amount is exactly `999,999.99$` and the user clicks the main money amount, the website should show two actions near the main money amount:

- `Subtract` for subtracting money from the main money amount.
- `Modify` for correcting the total displayed amount.

At exactly `999,999.99$`, the website should not show `Add` because the main money amount cannot go above `999,999.99$`.

The visible main money actions should be buttons lined together horizontally near the main money amount.

When `Subtract` is hidden at `0.00$`, the left-to-right order should be:

- `Add`
- `Modify`

When all three main money actions are visible, the left-to-right order should be:

- `Add`
- `Subtract`
- `Modify`

When `Add` is hidden at `999,999.99$`, the left-to-right order should be:

- `Subtract`
- `Modify`

The main money action buttons should not appear as a modal, bottom sheet, separate page, or large expanded area.

If the main money action buttons are visible and the user clicks or taps the main money amount again, the buttons should go away, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

If the main money action buttons are visible and the user clicks or taps outside the main money amount and outside the visible action buttons, the buttons should go away, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

If the main money action buttons are visible and the user scrolls the dashboard page, the buttons should go away, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking `Add`, `Subtract`, or `Modify` should start that selected money amount input flow and should not be treated as an outside click.

When a selected main money amount input flow starts, the visible main money action buttons should go away.

The selected action should be remembered internally only. The open input flow should not show a visible reminder of whether the user selected `Add`, `Subtract`, or `Modify`.

The dashboard should use `Current Balance` as the label for the main money amount.

The `Current Balance` label is user-facing website text. In the specs, the concept should be described as the money amount.

These controls are manual money tracking actions. They only update the tracked money amount shown in the website.

The user can use the available money actions as often or as rarely as they want. `Add` is available only when the money amount is less than `999,999.99$`, including when the money amount is `0.00$`. `Modify` is available when the money amount is from `0.00$` through `999,999.99$`. `Subtract` is available only after the money amount is greater than `0.00$`. There should be no daily, weekly, monthly, or yearly usage limit on money amount changes, but each money amount and the resulting main money amount must stay at or below `999,999.99$`.

`Add`, `Subtract`, and `Modify` money amount input flows should show a horizontal money amount input square in the middle of the screen.

The main money amount circle should stay visible only as a dimmed, inactive background element while the horizontal input square is open.

While the horizontal input square is open, the input flow is the active part of the screen. Clicking or tapping the visible main money amount circle behind the input flow should act like `Cancel`: the input flow should close, the dashboard money amount view should return, nothing should change, nothing should save, no `Balance Changes` entry should be created, and no message should appear. Clicking or tapping the visible main money amount circle should not reopen the main money action buttons.

The horizontal input square should start by showing `0.00` without the `$` sign.

When the horizontal input square opens, it should be focused and ready for typing immediately. On supported mobile devices, opening the input flow should request the mobile keyboard immediately.

The user should type only the money amount into the horizontal input square, without the `$` sign. If the typed money amount reaches `1,000.00` or more, the input should add comma separators automatically while the user is typing.

Main money amount inputs should request a mobile keyboard suitable for digit entry on supported devices. All users should be able to enter cents with `Space` or with a rectangular `Cent` button shown as part of the main money amount input controls while a main money amount input is open. The `Cent` button should appear directly under the horizontal input square on mobile and desktop. On mobile, when the browser and keyboard allow it, this placement should also keep the `Cent` button directly above the mobile keyboard. The `Cent` button should behave the same as pressing `Space`, should not add visible text to the input, and should not cancel or close the input flow. The user should not enter cents in main money amount inputs by typing a decimal point.

Main money amount inputs should accept only numbers from `0` through `9` and separator input from the `Space` key or `Cent` button. The first accepted character should be a number from `0` through `9`. Typed decimal points, comma separators, `$` signs, letters, minus signs, and other blocked characters should not change the field and should show no message. The decimal point and comma separators in the displayed input should be generated by the website only.

Digits typed before an accepted separator should be whole money amount digits. Typed digits should not automatically become cents because more digits were typed. `0` should be a normal digit, not a starting zero-position skip. Unneeded leading zeros in the whole money amount should be normalized away, so typing `0005` should show `5.00`, not `00.05`.

The `Space` key and `Cent` button should act as a non-visible separator between digit groups. A separator should be accepted only after at least one digit and only when the previous accepted input is a digit. Starting `Space` key presses, starting `Cent` taps, consecutive `Space` key presses, consecutive `Cent` taps, or mixed consecutive separator inputs should be blocked with no message. Typing `Space`, `Space`, `Space`, then `5` should block the three `Space` key presses and then show `5.00` after the `5` is typed. Tapping `Cent`, `Cent`, `Cent`, then typing `5` should block the three `Cent` taps and then show `5.00` after the `5` is typed.

The horizontal input square should always show the typed value with two digits after the decimal point, automatic comma separators for thousands and larger values, and no `$` sign while the user is typing. After the user saves, browser storage should save the money amount as a normalized plain decimal string without the `$` sign or comma separators, and the displayed money amount should show two digits after the decimal point, comma separators for thousands and larger values, and the `$` sign at the end.

For accepted input without separator input, all digits are whole money amount digits: typing `5` should show `5.00`, `58` should show `58.00`, `589` should show `589.00`, `5895` should show `5,895.00`, `58955` should show `58,955.00`, and `589550` should show `589,550.00`.

For accepted input with separator input from `Space` or `Cent`, the website should split the accepted digits into groups at each accepted separator. If the input ends with a separator, all completed digit groups should be treated as the whole money amount and cents should show as `00`. If the final group after a separator has one or two digits, that final group should be treated as cents and should be left-padded with `0` when it has one digit. All earlier groups should be joined together as the whole money amount. If there is only one accepted separator and the final group grows to three or more digits, that final group should be treated as another whole money amount group and cents should show as `00`. Once the input has two accepted separators, the final group is the cents group and should accept at most two digits. Additional separator input after two accepted separators should be blocked with no message.

Examples: typing `5`, then `Space`, should keep the display at `5.00`; typing `5`, then `Space`, then `5` should show `5.05`; typing `5`, then `Space`, then `50` should show `5.50`; typing `58`, then `Space`, then `430` should show `58,430.00`; typing `58`, then `Space`, then `430`, then `Space`, then `88` should show `58,430.88`; typing `0`, then `Space`, then `5` should show `0.05`; and typing `999999`, then `Space`, then `99` should show `999,999.99`. Tapping or clicking `Cent` should work the same as pressing `Space`, so typing `5`, then choosing `Cent`, then typing `50` should show `5.50`.

If the user deletes one typed character from the horizontal input square, the remaining accepted input should be formatted again with two digits after the decimal point, automatic comma separators when needed, and no `$` sign while the input is still open. Deleting should remove the last accepted digit or accepted separator. For example, deleting from `5.50` after typing `5`, `Space`, `50` or typing `5`, tapping `Cent`, then typing `50` should return to `5.05`; deleting again should return to `5.00`.

Main money amount inputs should be append-only. Clicking, tapping, or focusing the horizontal input square should not let the user move the cursor into the middle of the formatted money amount. The cursor should stay at the end when shown. The user should not be able to select part of the displayed money amount, replace selected text, or edit generated comma separators or the generated decimal point. If the browser or device shows a text selection anyway, the website should ignore that selection for money amount input behavior. Accepted typing should still be added to the end of the accepted input sequence, and delete should still remove only the last accepted digit or accepted separator.

If the user deletes all typed numbers from the horizontal input square, the square should return to `0.00`. The horizontal input square should not become empty.

If the user types letters, a minus sign, a decimal point, the `$` sign, a comma separator, a starting `Space`, a consecutive `Space`, an additional `Space` after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, or any other blocked character into a main money amount input, the field should not change and no message should appear. If the user taps `Cent` when a separator would be blocked by the same rules, the field should not change and no message should appear.

Main money amount inputs should allow any valid money amount from `0.00` through `999,999.99`. A maximum-sized money amount should stay editable in the horizontal input square without making the page overflow horizontally, overlapping `Cent`, overlapping `Save Changes`, overlapping `Yes` or `Cancel`, or hiding those controls. The layout may contain, wrap, break, shrink within readable limits, or internally scroll the typed money amount as needed, but it should preserve the full typed money amount.

The dashboard, `Balance Changes`, and `Savings` displays should handle money amounts up to `999,999.99$` without rejecting them, creating horizontal page overflow, overlapping other content, or hiding actions. The layout may wrap, break long number strings, or adjust text size within readable limits, but it should preserve the full visible money amount with required comma separators.

The user should not be able to paste into main money amount inputs. If the user tries to paste letters, numbers, symbols, or any other content into a main money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear.

Under the horizontal input square, the website should show the rectangular `Cent` button.

Under the `Cent` button, the website should show the exact text `Save Changes`.

Under `Save Changes`, the website should show two buttons exactly named `Yes` and `Cancel`.

In `Add`, `Subtract`, and `Modify`, clicking `Yes` should try to apply the typed money amount using the rules for that action.

After a successful `Add`, `Subtract`, or `Modify` that changes the money amount, the website should close the horizontal input square, reset the temporary typed input so the next main money amount input starts at `0.00`, return to the dashboard money amount view, hide the main money action buttons, show the updated money amount, save only the data required by that action, and show no message.

In `Add`, `Subtract`, and `Modify`, clicking `Cancel` should close the money amount input flow, return to the dashboard money amount view, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking or tapping outside the horizontal input square, `Save Changes`, `Yes`, `Cancel`, and the `Cent` button should act like `Cancel`. This includes clicking or tapping the dimmed, inactive main money amount circle behind the input flow.

Using the browser Back button, mobile browser back gesture, or system Back action while an `Add`, `Subtract`, or `Modify` money amount input flow is open should act like `Cancel`: close the input flow, return to the dashboard money amount view, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

If the user refreshes the page, closes the browser tab or window, or reopens the website later while an `Add`, `Subtract`, or `Modify` money amount input flow has unsaved typed input, the website should discard that open input flow and typed value. The next website load should show the last successfully saved data, change nothing, save nothing, create no `Balance Changes` entry, and show no message or browser leave warning.

When the user chooses `Modify`:

- The website asks for the corrected total money amount.
- The entered amount must be from `0.00$` through `999,999.99$`.
- The horizontal input square should start at `0.00`.
- The user should type the money amount using the main money amount input rules, including optional cents.
- If the user deletes all typed numbers, the horizontal input square should return to `0.00`.
- If the user clicks `Yes` with the same money amount that is already shown, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, browser storage should not be created or updated, no `Balance Changes` entry should be created, `Balance Changes` cleanup should not run, and the same money amount input step should stay open until the user enters a different valid money amount or cancels.
- If the user clicks `Yes` while the input is `0.00` and the current money amount is greater than `0.00$`, the main money amount should become `0.00$`.
- A negative entered amount should be blocked.
- A different valid entered amount replaces the current money amount.
- The new current money amount is saved in browser storage only when `Modify` changes the money amount.
- The action should not create a history entry.
- The action should not create a `Balance Changes` entry.
- The action should not create a notification.
- The action should not display as `+{money amount} added`, `-{money amount} subtracted`, or a manual correction.
- If `Modify` changes the money amount to `0.00$`, the next click on the main money amount should show `Add` and `Modify`, but not `Subtract`.
- If `Modify` changes the money amount to `999,999.99$`, the next click on the main money amount should show `Subtract` and `Modify`, but not `Add`.

When the user chooses `Add`:

- The website asks for the amount to add.
- The entered amount must be greater than `0.00$` and not greater than `999,999.99$`.
- The horizontal input square should start at `0.00`.
- The user should type the money amount using the main money amount input rules, including optional cents.
- If the user deletes all typed numbers, the horizontal input square should return to `0.00`.
- If the user clicks `Yes` while the input is `0.00`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a money amount greater than `0.00$` or cancels.
- A negative entered amount should be blocked.
- The entered amount is added to the main money amount.
- If adding the entered amount would make the main money amount greater than `999,999.99$`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a valid money amount or cancels.
- The action is saved in `Balance Changes` as a positive entry.
- The history entry should display as `+{money amount} added`.
- The history entry should stay separate from other add or subtract entries.
- If the money amount was `0.00$` before a successful `Add`, and the new money amount is greater than `0.00$` and less than `999,999.99$`, then the next click on the main money amount should show `Add`, `Subtract`, and `Modify`.
- If a successful `Add` makes the money amount exactly `999,999.99$`, then the next click on the main money amount should show `Subtract` and `Modify`, but not `Add`.

When the user chooses `Subtract`:

- The website asks for the amount to subtract.
- The entered amount must be greater than `0.00$` and not greater than `999,999.99$`.
- The horizontal input square should start at `0.00`.
- The user should type the money amount using the main money amount input rules, including optional cents.
- If the user deletes all typed numbers, the horizontal input square should return to `0.00`.
- If the user clicks `Yes` while the input is `0.00`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a money amount greater than `0.00$` or cancels.
- A negative entered amount should be blocked.
- The website subtracts up to the current money amount.
- The action is saved in `Balance Changes` as a negative entry.
- The history entry should display as `-{money amount} subtracted`.
- The history entry should stay separate from other add or subtract entries.

The main money amount should never go below `0.00$`.

If the user enters a subtract amount greater than the current money amount:

- The website should set the main money amount to `0.00$`.
- The website should not show a negative money amount.
- The visible history entry should use the actual amount removed from the money amount.

Example:

- Current money amount is `20.00$`.
- User enters `50.00` in `Subtract`.
- New money amount becomes `0.00$`.
- Visible history shows `-20.00$ subtracted`.
- Visible history should not show `-50.00$ subtracted`.

If the current money amount is already `0.00$`, `Subtract` should not be shown. If a subtract action still runs while the money amount is `0.00$`, it should keep the money amount at `0.00$`, show no message, leave saved data unchanged, and not create a visible history entry.

The website should not combine separate add and subtract actions into one history result.

Example:

- Show `+56.00$ added`.
- Show `-34.00$ subtracted`.
- Do not replace those entries with only `+22.00$ net change`.

If `Balance Changes` has no entries, the history list should show no rows.

Empty `Balance Changes` should not show a sentence, placeholder, icon, or any other empty-state content.

Each visible `Balance Changes` row should show the signed money amount, action text, and both the created date and created time for that entry:

- `+{money amount} added`.
- `-{money amount} subtracted`.

Visible `Balance Changes` rows should not show the previous money amount or new money amount.

Visible `Balance Changes` rows should show both the created date and created time for that entry. A date-only display is not enough. The visible date and time format should be like `July 21, 2026 at 3:45 PM`.

Each visible `Balance Changes` entry should be compact, not too large. The signed money amount and action text should appear in the top-left corner. The visible created date and created time should appear in the bottom-right corner of the same entry, below the money change text and not too far from it. On mobile, the entry should keep the same layout. Text may wrap only as needed to stay readable, but the money change text should remain in the top-left area and the visible date and time should remain in the bottom-right area.

Visible `Balance Changes` rows should not show the internal exact created date and time, seconds, milliseconds, or the internal visible until date and time.

Visible add and subtract history entries should be kept for 30 days.

Visible `Balance Changes` entries should be ordered newest first by their internal exact created date and time. The newest change should appear at the top of the list, and older changes should go lower. If two entries have the exact same internal exact created date and time, the saved list order should decide the tie, with entries earlier in the saved list appearing first. New entries should be saved before older entries in the saved list.

After a successful `Add` or `Subtract` creates a new `Balance Changes` entry, the `Balance Changes` square should automatically scroll to the top so the newest entry is visible, even if the user had previously scrolled lower in the history list.

For visible `Balance Changes` history, one month means 30 days, not a calendar month.

The user should not choose a date when creating an `Add` or `Subtract` entry.

The website should treat both the created date and created time as user-facing `Balance Changes` details.

The visible date and time format should use the full month name, day, year, `at`, and 12-hour time with uppercase `AM` or `PM`, like `July 21, 2026 at 3:45 PM`.

Visible `Balance Changes` dates and times should use the user's browser/device local date and time.

When a `Balance Changes` entry is created, the website should save both the visible created date and visible created time using the user's browser/device local date and time so it can show both values in the visible row. It should also save an internal exact created date and time with seconds and milliseconds using the user's browser/device local date and time so it can order entries and clear them after 30 days.

The visible until date should be the internal exact created date and time plus 30 days using the same browser/device local date and time rule.

The website should run `Balance Changes` cleanup when it opens and loads saved data.

The website should also run `Balance Changes` cleanup after every successful saved user action.

During cleanup, entries at or after their visible until date and time should be deleted from browser storage and removed from the visible history list.

The first version does not need a background timer that checks old `Balance Changes` entries while the website stays open with no user action. If the website stays open past an entry's visible until date and time, that old entry may remain visible until the next website open or successful saved user action runs cleanup.

The current money amount should be saved separately from the visible history list so removing old history entries does not change the current money amount.

`Add`, `Subtract`, and `Balance Changes` entries should not have notes.

Each add or subtract action should include:

- Amount.
- Action type: added or subtracted.
- Created date and created time shown in the visible row using the format `July 21, 2026 at 3:45 PM` for display.
- Internal exact created date and time with seconds and milliseconds for ordering and 30-day clearing.
- Internal visible until date and time.

Each `Modify` action should require:

- New total amount.

`Modify` actions should not be included in visible history.

Each saved `Balance Changes` entry should include these data fields:

- ID.
- Action type: added or subtracted.
- Amount.
- Previous money amount.
- New money amount.
- Difference.
- Created date and created time shown in the visible row using the format `July 21, 2026 at 3:45 PM` for display.
- Internal exact created date and time with seconds and milliseconds for ordering and 30-day clearing.
- Internal visible until date and time.

When saved browser data can be loaded but one saved `Balance Changes` entry is broken, the website should keep the rest of the saved data and remove only the broken entry from `Balance Changes`. It should not show the full saved-data error message, should not show a broken history row, should not change the main money amount, should not change `Savings`, and should show no message to the user.

A saved `Balance Changes` entry should count as broken if it has a missing ID, duplicate ID, missing or invalid action type, missing or invalid money amount, missing or invalid previous money amount, missing or invalid new money amount, missing created date or created time, invalid created date or created time, missing or invalid internal exact created date and time, or missing or invalid visible until date and time. For duplicate IDs, the first matching saved `Balance Changes` entry in the saved list order should stay if it is otherwise valid, and later matching entries should be removed.

The previous money amount, new money amount, internal exact created date and time, seconds, milliseconds, and internal visible until date and time should not be shown in the visible `Balance Changes` row.

The user should be able to delete a `Balance Changes` entry when they make a mistake.

Touch users should start deleting a `Balance Changes` entry by pressing and holding that entry for `600ms`.

Mouse users should start deleting a `Balance Changes` entry by clicking and holding that entry for `600ms`.

Before the `600ms` hold completes, releasing the press or click, moving the pointer or finger, or starting to scroll should cancel the pending delete action. Canceling the pending hold should not open the little square, change anything, save anything, delete anything, create a `Balance Changes` entry, or show a message.

After the completed `600ms` press-and-hold or click-and-hold, the website should show a little square in the middle of the screen with the exact action texts:

`Delete`

`Cancel`

Only one `Balance Changes` delete action square should be open at a time.

Clicking `Cancel` in the little square should close the little square, change nothing, save nothing, and show no message.

Clicking or tapping outside the little square should close the little square, change nothing, save nothing, and show no message.

Using the browser Back button, mobile browser back gesture, or system Back action while the little `Delete` and `Cancel` square is open should close that little square, keep the user on the dashboard, change nothing, save nothing, delete nothing, create no `Balance Changes` entry, and show no message.

Moving the pointer or finger should not close the little square after it is open.

Scrolling should not close the little square after it is open.

Clicking `Delete` in the little square should close the little square and should not delete the entry immediately.

Before deleting a `Balance Changes` entry, the website should ask the user to confirm the delete action.

The confirmation message should be exactly `Delete this Balance Change?`.

The confirmation buttons should be exactly `Cancel` and `Delete`.

Clicking `Cancel` in the delete confirmation should close the confirmation, keep the `Balance Changes` entry visible, change nothing, save nothing, and show no message.

Clicking or tapping outside the delete confirmation should close the confirmation, keep the `Balance Changes` entry visible, change nothing, save nothing, and show no message.

Using the browser Back button, mobile browser back gesture, or system Back action while the `Delete this Balance Change?` confirmation is open should close that confirmation, keep the user on the dashboard, keep the `Balance Changes` entry visible, change nothing, save nothing, delete nothing, create no `Balance Changes` entry, and show no message.

Deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

After a `Balance Changes` entry is deleted, the website should not offer undo.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it from the visible history. If the main money amount is wrong, the user should use `Modify` to correct the main money amount.

Deleting a `Balance Changes` entry manually is different from the website deleting an old visible history entry after 30 days. Both actions remove entries from the visible history only. Neither action should change the main money amount.

The `Add`, `Subtract`, and `Modify` actions should appear visually connected to the main money amount so the user understands they directly change the displayed money amount.

## Balance Changes Behavior

`Balance Changes` is the user-facing history list for money amount changes.

`Balance Changes` should include:

- Added money.
- Subtracted money.

`Balance Changes` is a visible history record. It should not control or recalculate the main money amount.

`Balance Changes` should not include `Saving` square changes, because `Saving` square changes do not change the main money amount.

`Balance Changes` should appear directly under the main money amount on the dashboard as a large square.

Under the main money amount, the dashboard page space should be mainly dedicated to the large `Balance Changes` square.

When there are too many entries to fit inside the large `Balance Changes` square, the entries should scroll inside that square.

The dashboard page itself should still scroll down past the large `Balance Changes` square.

On the dashboard, `Savings` should appear below the large `Balance Changes` square, so the user reaches `Savings` by scrolling the dashboard page down past `Balance Changes`.

When the user scrolls inside the large `Balance Changes` square, only the entry list inside that square should scroll. If the entry list is already at the top or bottom, continuing to scroll inside the square should not move the dashboard page. To scroll the dashboard page down to `Savings` or back up, the user should scroll outside the large `Balance Changes` square.

All valid `Balance Changes` entries that are still inside the 30-day visible period should remain available inside the scrollable square. The website should not use a smaller maximum visible-entry limit for valid 30-day `Balance Changes` entries.

`Balance Changes` entries should be ordered newest first by their internal exact created date and time, so older changes go lower. If two entries have the exact same internal exact created date and time, the saved list order should decide the tie, with entries earlier in the saved list appearing first.

After a successful `Add` or `Subtract` creates a new `Balance Changes` entry, the `Balance Changes` square should automatically scroll to the top so the newest entry is visible, even if the user had previously scrolled lower in the history list.

If there are no `Balance Changes` entries, the `Balance Changes` history should simply be empty.

There should not be a separate `Recent Activity` area for the first version.

There should not be a separate `View history` action or button for `Balance Changes`.

If the history list has too many entries to fit inside the large square, the user should be able to scroll inside the `Balance Changes` square to see more entries.

## Dashboard And User Flow

The first screen should look like a simple bank dashboard.

The main money amount should be the most visible information on the dashboard.

The dashboard should show the main money amount with the label `Current Balance`.

On the dashboard, `Savings` should appear under `Balance Changes`.

The website should feel trustworthy, calm, and practical.

The user should not need financial knowledge to use it.

Main actions should be easy to find:

- Click the money amount.
- Add money.
- Subtract money.
- Modify amount.
- Click `Savings`.

The interface should work well on mobile.

Main user flow:

1. User opens the website.
2. If no saved data exists yet, user sees `0.00$` labeled as `Current Balance`.
3. User clicks the main money amount.
4. Website shows `Add` and `Modify`, but not `Subtract`.
5. User uses `Add` to add money.
6. Website saves the added money as `+{money amount} added`.
7. If the money amount is greater than `0.00$` and less than `999,999.99$`, clicking the main money amount shows `Add`, `Subtract`, and `Modify`.
8. If the money amount is exactly `999,999.99$`, clicking the main money amount shows `Subtract` and `Modify`, but not `Add`.
9. User uses `Add` when it is visible for new cash, `Subtract` for spent or removed cash, and `Modify` to correct mistakes without creating history or notifications.
10. User sees money amount change history directly under the main money amount.
11. User scrolls inside the `Balance Changes` square if the history list is longer than the square.
12. User scrolls the dashboard page down past the `Balance Changes` square to reach `Savings`.
13. User checks `Saving` squares.

## Savings Behavior

The website should have a `Savings` section that can be reached from the main dashboard.

`Savings` is the separate planning section of the website.

A `Saving` is one square inside the `Savings` section.

The user should click `Savings` to open the `Savings` section.

There should not be a separate `Open savings` action.

When the user opens `Savings`, the `Savings` view should take the entire screen.

The full-screen `Savings` view should show a small `<` sign in the top-left corner.

When the user clicks `<`, the website should return to the dashboard. This back action should change nothing, save nothing, create no `Balance Changes` entry, and show no message.

When the user uses the browser Back button, mobile browser back gesture, or system Back action while the full-screen `Savings` view is open, the website should close `Savings` and return to the dashboard. This should change nothing, save nothing, create no `Balance Changes` entry, and show no message.

The `Savings` section is a planning view. It helps the user see which `Saving` squares are fully covered, partly covered, or not covered by the main money amount.

The `Savings` section should start from the current money amount.

In the first version, `Saving` squares should not save or show a created date or updated date.

The `Savings` section should show a money amount at the top. This money amount is what remains after the website checks the ordered `Saving` squares against the main money amount. It should never show less than `0.00$`.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`.

The `Savings money amount` label is user-facing website text. In the specs, the concept can still be described as the money amount shown inside `Savings`.

The `Savings money amount` label should not be used for the main money amount. The main money amount should keep the `Current Balance` label.

Under the money amount shown inside `Savings`, the website should show `Saving` squares.

The full-screen `Savings` view should keep showing the money amount inside `Savings` and the `Saving` squares together. The money amount shown inside `Savings` should go to the visible `Saving` squares from top to bottom as a planning display only.

If the money amount shown inside `Savings` is `0.00$`, the website should show it with the same visual style used for nonzero `Savings money amount` values. It should not change color, size, weight, add an icon, or show a warning or error message just because the value is `0.00$`.

If total planned money amount in valid `Saving` squares is greater than the main money amount, the website should show a small top note directly under `Savings money amount`. The note should use the exact format `{money amount} needed`, such as `20.00$ needed`. The needed money amount should equal total planned money amount in valid `Saving` squares minus the main money amount.

If total planned money amount in valid `Saving` squares is equal to or less than the main money amount, the website should not show the top `{money amount} needed` note.

The top of the full-screen `Savings` view should stay fixed while the user scrolls. The fixed top area should include the small `<` sign, the `Savings money amount`, and the optional top needed note. Only the `Saving` squares area should scroll, and it should have enough top spacing so no `Saving` square content is hidden behind the fixed top area.

During `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows, the `Savings money amount`, top needed note, and coverage bars should stay based on the last saved `Saving` squares and the current main money amount. Unsaved typed input should not preview or change those calculated values. After a successful `Save`, the website should recalculate the `Savings money amount`, top needed note, and coverage bars. If the user clicks `Cancel`, those calculated values should stay unchanged.

The visible `Saving` squares should appear in one vertical column on mobile and desktop. The website should not show `Saving` squares in multiple columns or a grid.

When there are no visible `Saving` squares in the `Saving` squares area, the website should show the circle `+` action in the middle of that area.

The centered `+` action is the empty state for no normal `Saving` squares and no broken saved `Saving` squares. The website should not show a separate empty-state text sentence for no `Saving` squares.

After at least one `Saving` square exists, the circle `+` action should appear at the top-left of the `Saving` squares area, above or before the visible squares.

A broken saved `Saving` square should count as a visible square for circle `+` placement only.

If `Savings` contains one or more broken saved `Saving` squares and no normal `Saving` squares, the circle `+` action should appear at the top-left of the `Saving` squares area, above or before the broken saved `Saving` squares.

The circle `+` action should be centered only when there are no normal `Saving` squares and no broken saved `Saving` squares visible.

Broken saved `Saving` squares should still not affect `Savings money amount`, top needed note, or coverage bars.

Broken saved `Saving` squares should stay locked in their current displayed positions until the user fixes or deletes them. The user should not be able to drag or reorder a broken saved `Saving` square. Holding, clicking and holding, moving, or dragging a broken saved `Saving` square should not start reorder, should not show a drag placeholder, should not change anything, should not save anything, should not create a `Balance Changes` entry, and should show no message.

In its default state, each visible `Saving` square should show the `Saving` name in the top-left corner.

In its default state, each visible `Saving` square should show the planned money amount on the right side of the same top row.

In its default state, each visible `Saving` square should show the thin coverage bar at the bottom.

Clicking a `Saving` square should change that same square into its action state.

In the action state, the square should still show the `Saving` name in the top-left corner and the planned money amount on the right side of the same top row.

In the action state, the square should show a `Delete` text action at the bottom center.

In the action state, the square should not show the thin coverage bar.

In the action state, clicking the `Saving` name should open the rename flow for that square.

In the action state, clicking the planned money amount should open the planned-money-amount change flow for that square.

In the action state, clicking `Delete` should start the delete confirmation for that square.

The delete confirmation should appear as a small confirmation square in the middle of the screen.

The delete confirmation should show the exact message `Delete this Saving?`.

The delete confirmation should show buttons exactly named `Cancel` and `Delete`.

The delete confirmation should not appear inside the `Saving` square, as a temporary square state, as a bottom sheet, or as a separate page.

Only one `Saving` square delete confirmation should be open at a time.

Opening the delete confirmation from a normal `Saving` square should clear that square's action state.

Clicking `Cancel` in the delete confirmation should close the confirmation, return the selected normal `Saving` square to its default state, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking or tapping outside the delete confirmation should close the confirmation, return the selected normal `Saving` square to its default state, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking `Delete` in the delete confirmation should close the confirmation and confirm deleting only the selected `Saving` square.

The website should not open a separate action menu or larger action square for rename, planned-money-amount change, and delete.

Opening rename for an existing `Saving` square should replace that same square with a temporary `Saving` input square in the same visible position. The rename input square should ask only for the new `Saving` name. It should not open as a modal, bottom sheet, separate page, or separate floating input.

Opening planned-money-amount change for an existing `Saving` square should replace that same square with a temporary `Saving` input square in the same visible position. The planned-money-amount change input square should ask only for the new total planned money amount. It should not open as a modal, bottom sheet, separate page, or separate floating input.

If a `Saving` square is in action state and the user clicks or taps outside that same square, the website should close the action state and return that square to its default state.

Closing a `Saving` square action state by clicking outside should change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking or tapping inside the open action-state square should not count as an outside click.

If the user clicks another normal default `Saving` square while one square is in action state, the website should close the old action state and open the clicked square in its own action state.

Scrolling the `Saving` squares area by itself should not close an open action state.

Clicking the small `<` sign while a `Saving` square is in action state should clear that action state as part of returning to the dashboard. If the user later opens `Savings` again, all normal `Saving` squares should start in their default state.

The user should add a new `Saving` square with a circle action that contains a `+` sign.

Clicking the `+` action should open the same create flow whether the `+` is centered in the empty `Saving` squares area or placed at the top-left when `Saving` squares already exist.

Opening the create flow should replace the clicked `+` action with a temporary `Saving` input square inside the `Saving` squares area. If the clicked `+` was centered, the temporary input square should be centered in that area. If the clicked `+` was at the top-left, the temporary input square should appear at the top-left before the existing squares. The create flow should not open as a modal, bottom sheet, or separate page.

When the user clicks the `+` action to add a new `Saving` square, the website should ask for:

- `Saving` name.
- Planned money amount.

The `Saving` square should be created only after the user enters a valid name and a planned money amount greater than `0.00$` and not greater than `999,999.99$`.

If the user tries to save a new `Saving` square without a `Saving` name, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a `Saving` name or cancels creating that square.

If the user tries to save a new `Saving` square without a planned money amount, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a planned money amount or cancels creating that square.

The planned money amount may include cents, with up to two digits after the decimal point, but it should not be greater than `999,999.99$`. The user should enter only the number, without typing the `$` sign or comma separators. The user may type a decimal point, such as `14.56`. While typing, the planned money amount input should show the accepted typed value as raw decimal number text. It should accept digits from `0` through `9` and one decimal point. It should block letters, `$` signs, comma separators, a second decimal point, more than two digits after the decimal point, and `Space` with no message. It should not add the `$` sign, comma separators, or automatic two-decimal formatting before `Save`. It should not show a `Cent` button, and the main money amount `Space` key cents behavior should not apply. If there is no decimal point, the typed value should be treated as a whole money amount. If there is one decimal point, digits after the decimal point should be treated as cents. The planned money amount input should request a decimal numeric keyboard on devices that support it, so the user gets number keys `0` through `9` and a decimal point. The planned money amount input should use its own planned-money typing filter, and the first accepted character must be a number from `0` through `9`. The user should not be able to paste into the planned money amount input. If the user tries to paste letters, numbers, symbols, or any other content into the planned money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear. The website should save that money amount as a normalized plain decimal string such as `14.56` or `5895.50`, not as integer cents like `1456` and not with the `$` sign or comma separators. The website should show that money amount with the `$` sign at the end, such as `14.56$`. The zero money amount should show as `0.00$`, nonzero whole money amounts should show with two digits after the decimal point such as `14.00$`, nonzero money amounts with cents should show with two digits after the decimal point such as `14.50$`, and planned money amounts of `1,000.00$` or more should show comma separators such as `5,895.50$`. After `Save`, typing `14` should become `14.00$`, typing `14.5` should become `14.50$`, typing `5898` should become `5,898.00$`, and typing `589.80` should become `589.80$`. If the user tries to save a planned money amount greater than `999,999.99$`, nothing should happen: no message, no new or changed `Saving` square, no saved data change, no `Balance Changes` entry, and the same input flow should stay open.

`Saving` square create, rename, planned-money-amount change, and broken-square fix input flows should show two text actions at the bottom: `Save` and `Cancel`.

In `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows, `Save` should try to save the entered values using the rules for that action.

In `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows, `Cancel` should close the input flow, return to the `Saving` squares view, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

The `Saving` square name must be unique inside `Savings`.

The `Saving` square name should not have a maximum length. The user can enter a one-letter name, a number-only name, a name with numbers before or after words, multiple words, a full sentence, symbols, punctuation, emoji characters, or a very long name.

The website should trim spaces at the beginning and end of the `Saving` square name before saving it. Spaces inside the trimmed name should stay.

Very long `Saving` square names should stay usable in the `Saving` square layout. The website should wrap or contain the name so it does not create horizontal page overflow, overlap other square content, or get rejected only because it is long.

The website should treat names as duplicates when they match after trimming spaces and ignoring uppercase or lowercase letters.

If the user enters a name already used by another `Saving` square, the website should not create the new `Saving` square.

If the user enters a duplicate name while creating a `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message. No new `Saving` square should appear, saved data should stay unchanged, and `Balance Changes` should not get a new entry.

If the user cancels or does not enter both required values, the website should not create the `Saving` square.

If saved browser data can be loaded but one saved `Saving` square is broken, the website should keep the rest of the saved data.

The broken `Saving` square should still appear in `Savings` as an error square with this exact text:

`Saving could not be loaded.`

The broken `Saving` square should show actions exactly named `Fix` and `Delete`.

A broken `Saving` square should not use any money amount inside `Savings`, should not lower the `Savings money amount`, should not show a top needed note, should not show a coverage bar, and should not affect coverage bars for valid `Saving` squares.

Clicking `Fix` on a broken `Saving` square should change that broken square into a temporary `Saving` input square in the same visible position. The fix input square should not open as a modal, bottom sheet, or separate page. It should ask the user for:

- `Saving` name.
- Planned money amount greater than `0.00$` and not greater than `999,999.99$`.

If the user saves valid fix values, the website should turn the broken square into a normal `Saving` square and keep it in the same visible position when possible.

Saving a fixed `Saving` square should save browser storage, recalculate the `Savings money amount`, recalculate the top needed note, recalculate coverage bars, create no `Balance Changes` entry, and show no message.

If the user clicks `Cancel` while fixing a broken `Saving` square, the broken square should stay broken and nothing should change.

Clicking `Delete` on a broken `Saving` square should use the same small centered delete confirmation with the exact message `Delete this Saving?` and buttons exactly named `Cancel` and `Delete`.

Clicking `Cancel` or clicking or tapping outside that small confirmation square while deleting a broken `Saving` square should close the confirmation, keep the broken square visible, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Confirming delete for a broken `Saving` square should close the confirmation, remove only that broken square, save browser storage, recalculate the `Savings money amount`, recalculate the top needed note, recalculate coverage bars, create no `Balance Changes` entry, update no other `Saving` squares, show no message, and offer no undo.

If the user enters `0.00$` or a planned money amount greater than `999,999.99$` while creating a `Saving` square, the website should not create the `Saving` square.

If the user tries to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$`, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, `Balance Changes` should not get a new entry, and the same `Saving` square create input step should stay open until the user enters a planned money amount greater than `0.00$` and not greater than `999,999.99$` or cancels.

When a new `Saving` square create attempt has more than one invalid value, the website should check the create inputs in this order:

1. Planned money amount is missing, `0.00$`, or greater than `999,999.99$`.
2. `Saving` name is missing.
3. `Saving` name is duplicate.

The first matching rule decides what happens. If the user enters a duplicate `Saving` name and also leaves the planned money amount empty, enters `0.00$`, or enters an above-limit planned money amount, the website should keep the same `Saving` square create input step open with no message, no saved data change, no new `Saving` square, and no `Balance Changes` entry. Duplicate-name create behavior should happen only after the planned money amount is greater than `0.00$`, not greater than `999,999.99$`, and the `Saving` name is not empty.

Each `Saving` square stores the planned money amount chosen by the user.

The planned money amount in a `Saving` square does not mean money has moved into a separate place. It does not change the main money amount.

Each `Saving` square should keep the planned money amount chosen by the user. If the main money amount changes, the `Saving` square planned money amount should not change automatically.

When the user renames an existing `Saving` square:

- The user should start renaming by opening the square's action state and clicking the `Saving` name shown in the square.
- The rename flow should replace that same square with a temporary `Saving` input square in the same visible position.
- The website should ask for the new `Saving` name.
- The new name should be required.
- The new name should be unique inside `Savings`.
- The website should treat names as duplicates when they match after trimming spaces and ignoring uppercase or lowercase letters.
- If the user tries to save an empty new name, the website should do nothing.
- If the user tries to save an empty new name, no message should appear, the old name should stay saved, saved data should stay unchanged, and the rename flow should stay open until the user enters a `Saving` name or cancels renaming that square.
- If the new name is already used by another `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message.
- If the new name is already used by another `Saving` square, the `Saving` square should keep its old name, saved data should stay unchanged, and `Balance Changes` should not get a new entry.
- If the new name is valid, the website should replace the old name and save the change in browser storage.
- Renaming a `Saving` square should not change the main money amount.
- Renaming a `Saving` square should not change the money amount shown inside `Savings` or coverage bars.
- Renaming a `Saving` square should not create a `Balance Changes` entry.

When the user changes the planned money amount in an existing `Saving` square:

- The user should start changing the planned money amount by opening the square's action state and clicking the planned money amount shown in the square.
- The planned-money-amount change flow should replace that same square with a temporary `Saving` input square in the same visible position.
- The website should ask for the new total planned money amount.
- If the user tries to save an empty planned money amount, the website should do nothing.
- If the user tries to save an empty planned money amount, no message should appear, the old planned money amount should stay saved, saved data should stay unchanged, and the planned-money-amount change flow should stay open until the user enters a planned money amount or cancels changing that square.
- The entered amount should replace the old planned money amount.
- The website should not add the entered amount to the old planned money amount.
- The website should not subtract the entered amount from the old planned money amount.
- The main money amount should not change.
- The money amount shown inside `Savings` should update.
- The coverage bars should update.
- The change should be saved in browser storage.
- The change should not create a `Balance Changes` entry.
- The change should not display as `+{money amount} added` or `-{money amount} subtracted`.

If the user changes an existing `Saving` square planned money amount to `0.00$`:

- The `Saving` square should disappear from `Savings`.
- The `Saving` square should be removed from browser storage.
- The main money amount should not change.
- The money amount shown inside `Savings` should update.
- Coverage bars for remaining `Saving` squares should update.
- The website should not create a `Balance Changes` entry.

`Saving` squares should have a visible order:

- The first `Saving` square the user creates should stay at the top by default.
- `Saving` squares should appear one after another in a single vertical column.
- The visible order of normal `Saving` squares should be the coverage order.
- The website should check normal `Saving` squares from top to bottom and skip broken saved `Saving` squares.
- Touch users should be able to change the order by holding a normal default `Saving` square for `600ms`, then moving the whole square.
- Mouse users should be able to change the order by clicking and holding a normal default `Saving` square for `600ms`, then dragging the whole square.
- Broken saved `Saving` squares should not be draggable and should not be reordered until fixed.
- Normal default `Saving` squares can still be reordered while broken saved `Saving` squares are visible, as long as no `Saving` square is in action state, input state, or delete confirmation state.
- During normal square reorder, broken saved `Saving` squares should stay in their displayed positions and should be skipped by `Savings money amount`, top needed note, and coverage calculations.
- After the `600ms` hold completes, dragging should start only after the pointer or finger moves at least `8px`.
- Holding or dragging a `Saving` square should not open rename, planned-money-amount change, or delete.
- While dragging, the dragged `Saving` square should follow the user's finger or mouse pointer.
- The old position should show a placeholder the same size as the dragged square.
- Dragging near the top or bottom of the scrollable `Saving` squares area should auto-scroll that area.
- Reordering should be disabled while any `Saving` square is in action state, input state, or delete confirmation state.
- The user should close, cancel, save, or finish any open `Saving` square action, input, or delete confirmation state before reordering.
- If the user moves a normal `Saving` square to the top, the website should check that square first.
- The moved normal square's coverage bar should be calculated before lower normal squares use any remaining money amount.
- When the user finishes moving a `Saving` square and lets go, the website should save the new order in browser storage.
- After the move finishes, the money amount shown inside `Savings` and the coverage bars should update from the new visible order of normal `Saving` squares, skipping any broken saved `Saving` squares.
- Reordering `Saving` squares should not change the main money amount and should not create a `Balance Changes` entry.

When the user deletes a `Saving` square:

- The user should start deleting by opening the square's action state and clicking the `Delete` text action shown at the bottom center of the square.
- The website should ask the user to confirm the delete action.
- The confirmation should appear as a small confirmation square in the middle of the screen.
- The confirmation message should be exactly `Delete this Saving?`.
- The confirmation buttons should be exactly `Cancel` and `Delete`.
- Only one `Saving` square delete confirmation should be open at a time.
- Clicking `Cancel` should close the confirmation, return the selected normal `Saving` square to its default state, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- Clicking or tapping outside the small confirmation square should close the confirmation, return the selected normal `Saving` square to its default state, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- Clicking `Delete` in the small confirmation square should close the confirmation and confirm the delete.
- Only that `Saving` square and the details inside it should be removed from `Savings`.
- The main money amount should not change.
- `Balance Changes` should not get a new entry.
- Other `Saving` squares should keep their names, planned money amounts, and relative order.
- The money amount shown inside `Savings` and the coverage bars should update from the remaining `Saving` squares because they are calculated display values.
- After the `Saving` square is deleted, the website should not offer undo.

The website should calculate `Saving` square coverage like this:

- Start with the main money amount.
- Check the first `Saving` square.
- Use as much of the remaining money amount as needed to cover that square.
- Move to the next `Saving` square with whatever money amount is still remaining.
- Continue until all `Saving` squares have been checked or the remaining money amount is `0.00$`.

Each `Saving` square should show a thin horizontal coverage bar at the very bottom of the square in its default state:

- The bar is grey by default.
- The bar fills green from left to right based on the covered percentage of that square's planned money amount.
- If a square is fully covered, the bar is fully green.
- If a square is partly covered, the left part of the bar is green and the remaining right part is grey.
- If a square is not covered, the bar is fully grey.

If a `Saving` square is 80% covered, the left 80% of the coverage bar should be green and the right 20% should be grey.

If a `Saving` square is not fully covered, the square should show a small `{money amount} needed` note at the top-left of the bottom coverage bar.

The money amount shown inside `Savings` should be calculated as:

- Current money amount minus the total planned money amount in `Saving` squares, with the result stopped at `0.00$`.

Example:

- Current money amount is `350.00$`.
- Total planned money amount in `Saving` squares is `0.00$`.
- Money amount shown inside `Savings` is `350.00$`.

If the user creates a `Rent` `Saving` square with a planned money amount of `40.00$`:

- Current money amount stays `350.00$`.
- `Rent` square shows `40.00$`.
- Total planned money amount in `Saving` squares is `40.00$`.
- Money amount shown inside `Savings` becomes `310.00$`.

This means the user's money amount is still `350.00$`, but the `Savings` section shows that `40.00$` is planned for rent and `310.00$` is still available for other plans.

Example with ordered coverage:

- Current money amount is `85.00$`.
- `Rent` is the first `Saving` square and has `50.00$`.
- `Food` is the second `Saving` square and has `20.00$`.
- `School` is the third `Saving` square and has `30.00$`.
- `Rent` is fully covered, so its bar is 100% green.
- `Food` is fully covered, so its bar is 100% green.
- `School` is partly covered with `15.00$` of `30.00$`, so the left 50% of its bar is green and the right 50% is grey.
- `School` shows `15.00$ needed` at the top-left of its bottom coverage bar.
- Money amount shown inside `Savings` is `0.00$`.

If the user moves `School` to the top in this example:

- `School` is checked first and becomes fully covered.
- `Rent` is checked second and becomes fully covered.
- `Food` is checked third with `5.00$` of `20.00$`, so the left 25% of its bar is green and the right 75% is grey.
- `Food` shows `15.00$ needed` at the top-left of its bottom coverage bar.
- Money amount shown inside `Savings` stays `0.00$`.

Example with top needed note:

- Current money amount is `100.00$`.
- Total planned money amount in valid `Saving` squares is `120.00$`.
- Money amount shown inside `Savings` is `0.00$`.
- The top `Savings` area shows `20.00$ needed` directly under `Savings money amount`.

If current money amount is `100.00$` and total planned money amount in valid `Saving` squares is also `100.00$`, money amount shown inside `Savings` is `0.00$`, but the top `Savings` area should not show `0.00$ needed` or any other top needed note.

Creating a `Saving` square with a planned money amount is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

Changing a `Saving` square planned money amount is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

`Saving` square changes should not display as `+{money amount} added` or `-{money amount} subtracted`.

`Saving` square changes should not appear in `Balance Changes`.

Each `Saving` square should include:

- ID.
- Name.
- Planned money amount greater than `0.00$` and not greater than `999,999.99$`.
- Order.

The user should be able to:

- Create a `Saving` square.
- Rename a `Saving` square.
- Change the planned money amount in a `Saving` square.
- Delete a `Saving` square.
- Reorder `Saving` squares by holding a normal default square for `600ms`, then moving or dragging the whole square after at least `8px` of pointer or finger movement.

If the current money amount becomes lower than the total planned money amount in valid `Saving` squares, the website should not change the `Saving` square planned money amounts automatically. It should update the coverage bars, show the money amount inside `Savings` as `0.00$`, and show the top `{money amount} needed` note with the difference.

## Saved Data User Behavior

The website should restore saved data when the user refreshes, closes, or opens the website again later and the saved data is available.

If no saved data exists, the website should show the dashboard with the money amount set to `0.00$`.

If no saved data exists, the website should not create saved browser data only because it showed the default `0.00$` money amount.

The website should start saving data after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

If browser storage is unavailable, full, blocked, or fails while saving a valid user action, the website should block that action.

When saving fails, the website should show this exact message:

`Changes could not be saved.`

When saving fails, the visible money amount, `Balance Changes`, `Savings`, and saved data should stay on the last successfully saved state.

When saving fails, the website should not create, update, delete, or reorder saved data.

When saving fails, the website should not create a `Balance Changes` entry.

When saving fails, the website should not run `Balance Changes` cleanup as a successful saved user action.

A user action should count as a successful saved user action only after the required browser storage write succeeds.

If saving fails from an `Add`, `Subtract`, or `Modify` input flow, the same money amount input flow should stay open with the same typed value so the user can try again or cancel.

If saving fails from a `Saving` square create, rename, planned-money-amount change, or broken-square fix input flow, the same input flow should stay open with the same typed values so the user can try again or cancel.

If saving fails while confirming a `Saving` square delete, the delete confirmation should stay open and the selected `Saving` square should stay visible.

If saving fails while confirming a `Balance Changes` delete, the delete confirmation should stay open and the selected `Balance Changes` entry should stay visible.

If saving fails after a completed `Saving` square reorder, the visible order should return to the last successfully saved order.

The website should load saved browser data only when the data version value is exactly `1`.

If saved browser data has a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, the website should treat it as broken saved data.

Saved current money amount values such as `5`, `5.0`, `005.00`, `5,000.00`, and `5.000` should be treated as broken saved data because they are not normalized plain decimal strings, even though some of them represent in-range money amount values.

If saved data is broken or cannot be read, the website should show this message:

`Saved data could not be loaded.`

The website should show a `Start again` action with that message.

When the user clicks `Start again`:

- The website should delete the broken saved data from browser storage.
- The website should immediately create fresh browser storage data with the saved money amount set to `0.00` and data version `1`.
- `Balance Changes` should be empty.
- `Savings` should have no saved `Saving` squares.

If the saved browser data file can be read and has data version `1`, but one saved `Saving` square inside it is broken, the website should not show the full saved-data error message just because of that one broken square. It should load the rest of the saved data and show the broken `Saving` square error state in `Savings`.

If the saved browser data file can be read and has data version `1`, but one saved `Balance Changes` entry inside it is broken, the website should not show the full saved-data error message just because of that one broken history entry. It should load the rest of the saved data and remove only the broken `Balance Changes` entry from visible and saved history.

The website should not show user-facing text that explains where saved data is stored.

The website should not show user-facing text that says saved data belongs only to the same browser or device.

The website should not show user-facing text that warns saved data may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

Technical browser storage rules are defined in `docs/specs/technical/technical1.md`.
