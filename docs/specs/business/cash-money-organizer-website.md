# Business Spec: Cash Money Organizer Website

## Business Goals

- Help users organize cash without needing a real bank account.
- Help users plan cash visually in `Savings` without creating a separate target-tracking system.
- Reduce forgotten spending.
- Make cash tracking simple enough for daily use when the user wants it, without requiring daily updates.
- Build trust with a clean, bank-like interface.

## Core Features

### Money Amount

The user can view their total money amount with the user-facing label `Current Balance`.

A new user should see the money amount as `0.00$` the first time they open the website.

Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`. Visible money amounts of `1,000.00$` or more should use comma separators every three digits before the decimal point, starting from the right, such as `5,895.50$` and `999,999.99$`.

Saved browser storage money amount values are internal and should use normalized plain decimal strings without the `$` sign or comma separators, such as `0.00`, `5.00`, `5895.50`, and `999999.99`. User-facing website display should still add comma separators and the `$` sign, such as `5,895.50$`.

Money amount values can include cents, but the maximum allowed money amount is `999,999.99$`. The main money amount and each `Saving` square planned money amount should never be saved above `999,999.99$`. Examples of valid visible money amounts include `5.05$`, `0.05$`, and `999,999.99$`. Values above `999,999.99$` are invalid.

The website should not ask for a starting amount before showing the dashboard.

The website should not create saved browser data only because it shows the default `0.00$` money amount.

The default `0.00$` money amount should not create a `Balance Changes` entry.

When the money amount is `0.00$`, clicking the main money amount should show `Add` and `Modify`, but should not show `Subtract`.

When the money amount is greater than `0.00$` and less than `999,999.99$`, clicking the main money amount should show `Add`, `Subtract`, and `Modify`.

When the money amount is exactly `999,999.99$`, clicking the main money amount should show `Subtract` and `Modify`, but should not show `Add`.

The main money actions should appear as buttons lined together horizontally near the main money amount. When `Subtract` is hidden at `0.00$`, the visible order should be `Add`, then `Modify`. When all three actions are visible, the left-to-right order should be `Add`, `Subtract`, and `Modify`. When `Add` is hidden at `999,999.99$`, the visible order should be `Subtract`, then `Modify`.

The main money action buttons should not appear as a modal, bottom sheet, separate page, or large expanded area.

If the main money action buttons are visible and the user clicks or taps the main money amount again, the buttons should go away without changing the money amount, saving anything, creating a `Balance Changes` entry, or showing a message.

If the main money action buttons are visible and the user clicks or taps outside the main money amount and outside the visible action buttons, the buttons should go away without changing the money amount, saving anything, creating a `Balance Changes` entry, or showing a message.

Clicking `Add`, `Subtract`, or `Modify` should start that selected money amount input flow and should not be treated as an outside click.

When a selected main money amount input flow starts, the visible main money action buttons should go away. The selected action should be remembered internally only. The open input flow should not show a visible reminder of whether the user selected `Add`, `Subtract`, or `Modify`.

The website should make the main money actions easy to understand without using real banking language.

### Storage Scope

The first version should save the user's data only in browser storage.

The implementation should use the stable storage key `cash-money-organizer-website-data` and the first data version value `1`.

The first version should load saved browser data only when the data version value is exactly `1`. A saved current money amount is valid only when it is a normalized plain decimal string from `0.00` through `999999.99`, with no `$` sign, no comma separators, exactly two digits after the decimal point, and no unneeded leading zeros before the decimal point except the single `0` in `0.00`. Saved browser data with a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside that range or not in that exact saved format, should be treated as broken saved data. Examples of broken saved current money amount values include `5`, `5.0`, `005.00`, `5,000.00`, `5.000`, `1000000.00`, and `-5.00`.

If saved browser data is broken or cannot be read, the website should show `Saved data could not be loaded.` with a `Start again` action. When the user chooses `Start again`, the website should delete the broken saved data and immediately create fresh browser storage data with the saved money amount set to `0.00`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`.

When no saved data exists yet, the website should show the default `0.00$` money amount without saving it just because the website opened. The website should start saving data after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

Browser storage is an internal implementation detail. The website should not tell the user where saved information is stored.

The website should not tell the user that saved information belongs to the same browser or device.

The website should not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

### Add and Subtract Money Amount

The user can quickly add money to the money amount or subtract money from the money amount when their real cash changes.

This is for normal cash movements, such as receiving cash, spending cash, or removing cash from the tracked amount. It is not the same as correcting the full money amount after the displayed amount is wrong.

The website should support users who update their money amount rarely and users who update it many times in one day.

The money amount should never become negative.

The `Add` action should be available only when the money amount is less than `999,999.99$`, including when the money amount is `0.00$`.

The `Subtract` action should be available only when the money amount is greater than `0.00$`.

The `Modify` action should be available when the money amount is from `0.00$` through `999,999.99$`.

`Add` and `Subtract` should accept only money amounts greater than `0.00$` and not greater than `999,999.99$`.

Negative money amounts should be blocked for all money actions.

Main money amount actions should not make the main money amount go above `999,999.99$`. If the user tries to `Add` a money amount that would make the main money amount greater than `999,999.99$`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a valid money amount or cancels.

When the user starts a main money amount input for `Add`, `Subtract`, or `Modify`, the website should show a horizontal money amount input square in the middle of the screen.

The main money amount circle should stay visible in the background while the horizontal input square is open.

The horizontal input square should start by showing `0.00` without the `$` sign.

When the horizontal input square opens, it should be focused and ready for typing immediately. On supported mobile devices, opening the input flow should request the mobile keyboard immediately.

The user should type only the money amount into the horizontal input square, without the `$` sign. If the typed money amount reaches `1,000.00` or more, the input should add comma separators automatically while the user is typing.

Main money amount inputs should request a mobile keyboard suitable for digit entry on supported devices. All users should be able to enter cents with `Space` or with a rectangular `Cent` button shown as part of the main money amount input controls while a main money amount input is open. On mobile, the `Cent` button should appear directly above the mobile keyboard when possible. The `Cent` button should behave the same as pressing `Space`, should not add visible text to the input, and should not cancel or close the input flow. The user should not enter cents in main money amount inputs by typing a decimal point.

Main money amount inputs should accept only numbers from `0` through `9` and separator input from the `Space` key or `Cent` button. The first accepted character should be a number from `0` through `9`. Typed decimal points, comma separators, `$` signs, letters, minus signs, and other blocked characters should not change the field and should show no message. The decimal point and comma separators in the displayed input should be generated by the website only.

Digits typed before an accepted separator should be whole money amount digits. Typed digits should not automatically become cents because more digits were typed. `0` should be a normal digit, not a starting zero-position skip. Unneeded leading zeros in the whole money amount should be normalized away, so typing `0005` should show `5.00`, not `00.05`.

The `Space` key and `Cent` button should act as a non-visible separator between digit groups. A separator should be accepted only after at least one digit and only when the previous accepted input is a digit. Starting `Space` key presses, starting `Cent` taps, consecutive `Space` key presses, consecutive `Cent` taps, or mixed consecutive separator inputs should be blocked with no message. Typing `Space`, `Space`, `Space`, then `5` should block the three `Space` key presses and then show `5.00` after the `5` is typed. Tapping `Cent`, `Cent`, `Cent`, then typing `5` should block the three `Cent` taps and then show `5.00` after the `5` is typed.

Main money amount inputs should always show the typed value with two digits after the decimal point, automatic comma separators for thousands and larger values, and no `$` sign while the user is typing. After the user saves, browser storage should save the money amount as a normalized plain decimal string without the `$` sign or comma separators, and the displayed money amount should show two digits after the decimal point, comma separators for thousands and larger values, and the `$` sign at the end.

For accepted input without separator input, all digits are whole money amount digits: typing `5` should show `5.00`, `58` should show `58.00`, `589` should show `589.00`, `5895` should show `5,895.00`, `58955` should show `58,955.00`, and `589550` should show `589,550.00`.

For accepted input with separator input from `Space` or `Cent`, the website should split the accepted digits into groups at each accepted separator. If the input ends with a separator, all completed digit groups should be treated as the whole money amount and cents should show as `00`. If the final group after a separator has one or two digits, that final group should be treated as cents and should be left-padded with `0` when it has one digit. All earlier groups should be joined together as the whole money amount. If there is only one accepted separator and the final group grows to three or more digits, that final group should be treated as another whole money amount group and cents should show as `00`. Once the input has two accepted separators, the final group is the cents group and should accept at most two digits. Additional separator input after two accepted separators should be blocked with no message.

Examples: typing `5`, then `Space`, should keep the display at `5.00`; typing `5`, then `Space`, then `5` should show `5.05`; typing `5`, then `Space`, then `50` should show `5.50`; typing `58`, then `Space`, then `430` should show `58,430.00`; typing `58`, then `Space`, then `430`, then `Space`, then `88` should show `58,430.88`; typing `0`, then `Space`, then `5` should show `0.05`; and typing `999999`, then `Space`, then `99` should show `999,999.99`. Tapping or clicking `Cent` should work the same as pressing `Space`, so typing `5`, then choosing `Cent`, then typing `50` should show `5.50`.

If the user deletes one typed character from the horizontal input square, the remaining accepted input should be formatted again with two digits after the decimal point, automatic comma separators when needed, and no `$` sign while the input is still open. Deleting should remove the last accepted digit or accepted separator. For example, deleting from `5.50` after typing `5`, `Space`, `50` or typing `5`, tapping `Cent`, then typing `50` should return to `5.05`; deleting again should return to `5.00`.

Main money amount inputs should be append-only. Clicking, tapping, or focusing the horizontal input square should not let the user move the cursor into the middle of the formatted money amount. The cursor should stay at the end when shown. The user should not be able to select part of the displayed money amount, replace selected text, or edit generated comma separators or the generated decimal point. If the browser or device shows a text selection anyway, the website should ignore that selection for money amount input behavior. Accepted typing should still be added to the end of the accepted input sequence, and delete should still remove only the last accepted digit or accepted separator.

If the user deletes all typed numbers from the horizontal input square, the square should return to `0.00`. The horizontal input square should not become empty.

If the user types letters, a minus sign, a decimal point, the `$` sign, a comma separator, a starting `Space`, a consecutive `Space`, an additional `Space` after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, or any other blocked character into a main money amount input, the field should not change and no message should appear. If the user taps `Cent` when a separator would be blocked by the same rules, the field should not change and no message should appear.

Main money amount inputs should allow any valid money amount from `0.00` through `999,999.99`. A maximum-sized money amount should stay editable in the horizontal input square without making the page overflow horizontally, overlapping `Save Changes`, overlapping `Yes` or `Cancel`, or hiding those controls. The layout may contain, wrap, break, shrink within readable limits, or internally scroll the typed money amount as needed, but it should preserve the full typed money amount.

The dashboard, `Balance Changes`, and `Savings` displays should handle money amounts up to `999,999.99$` without rejecting them, creating horizontal page overflow, overlapping other content, or hiding actions. The layout may wrap, break long number strings, or adjust text size within readable limits, but it should preserve the full visible money amount with required comma separators.

The user should not be able to paste into main money amount inputs. If the user tries to paste letters, numbers, symbols, or any other content into a main money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear.

Under the horizontal input square, the website should show the exact text `Save Changes`.

Under `Save Changes`, the website should show two buttons exactly named `Yes` and `Cancel`.

Clicking `Yes` should try to apply the typed money amount using the selected action. For `Add`, the typed money amount should be added to the main money amount. For `Subtract`, the typed money amount should be subtracted from the main money amount. For `Modify`, the typed money amount should replace the main money amount.

After a successful `Add`, `Subtract`, or `Modify` that changes the money amount, the website should close the horizontal input square, reset the temporary typed input so the next main money amount input starts at `0.00`, return to the dashboard money amount view, hide the main money action buttons, show the updated money amount, save only the data required by that action, and show no message.

Clicking `Cancel` should close the horizontal input square, return to the dashboard money amount view, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking or tapping outside the horizontal input square, `Save Changes`, `Yes`, `Cancel`, and the `Cent` button should act like `Cancel`.

If the user clicks `Yes` while the input is `0.00` in `Add` or `Subtract`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a money amount greater than `0.00$` or cancels.

If the user clicks `Yes` in `Modify` with the same money amount that is already shown, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, browser storage should not be created or updated, no `Balance Changes` entry should be created, `Balance Changes` cleanup should not run, and the same money amount input step should stay open until the user enters a different valid money amount or cancels.

If the user clicks `Yes` while the input is `0.00` in `Modify` and the current money amount is greater than `0.00$`, the website should replace the main money amount with `0.00$`.

If the user subtracts more than the current money amount, the website should set the money amount to `0.00$`.

When a subtract action is larger than the current money amount, the `Balance Changes` entry should show only the actual amount removed from the money amount.

If the money amount is already `0.00$`, subtracting should keep the money amount at `0.00$` and should not create a `Balance Changes` entry.

### Money Change History

The website should help users review recent money changes using the user-facing label `Balance Changes`.

`Balance Changes` helps the user understand how the money amount changed over time.

`Add` and `Subtract` entries should stay separate in `Balance Changes` and remain visible for 30 days.

If `Balance Changes` has no entries, the history list should simply be empty. The website should not show a sentence, placeholder, icon, or any other empty-state content for empty `Balance Changes`.

Each visible `Balance Changes` row should show the signed money amount, action text, and both the created date and created time for that entry: `+{money amount} added` or `-{money amount} subtracted`, plus the visible created date and created time. A date-only display is not enough. The visible date and time format should use the full month name, day, year, `at`, and 12-hour time with uppercase `AM` or `PM`, like `July 21, 2026 at 3:45 PM`.

Each visible `Balance Changes` entry should look compact, not too large. The signed money amount and action text should be placed in the top-left corner of the entry. The visible created date and created time should be placed in the bottom-right corner of the same entry, below the money change text and not too far away from it. Mobile should use the same layout. On small screens, the text may wrap only as needed to stay readable, but the money change text should remain in the top-left area and the visible date and time should remain in the bottom-right area.

Visible `Balance Changes` rows should not show the previous money amount or new money amount.

Visible `Balance Changes` rows should show both the created date and created time for that entry using the format `July 21, 2026 at 3:45 PM`.

Visible `Balance Changes` dates and times should use the user's browser/device local date and time.

Visible `Balance Changes` rows should not show the internal visible-until date and time.

The newest `Balance Changes` entry should appear first.

Old visible history entries should be deleted from browser storage during cleanup after 30 days without changing the saved money amount.

For `Balance Changes`, one month means 30 days, not a calendar month.

The user should not choose a date for an `Add` or `Subtract` entry.

Dates and times should matter for showing when visible `Balance Changes` entries were created and for clearing old visible `Balance Changes` entries after 30 days. When a `Balance Changes` entry is created, the website should save both the created date and created time using the user's browser/device local date and time, show both values in the visible row, and calculate its internal visible-until date and time as 30 days later using the same browser/device local date and time rule.

The website should run `Balance Changes` cleanup when it opens and loads saved data, and after every successful saved user action. During cleanup, entries at or after their visible-until date and time should be deleted from browser storage and removed from the visible history list.

The first version does not need a background timer that checks old `Balance Changes` entries while the website stays open with no user action. If the website stays open past an entry's visible-until date and time, that old entry may remain visible until the next website open or successful saved user action runs cleanup.

The user can delete a `Balance Changes` entry when they make a mistake.

Touch users should open the delete action by pressing and holding the `Balance Changes` entry.

Mouse users should open the delete action by clicking and holding the `Balance Changes` entry.

After the press-and-hold or click-and-hold, a little square should pop up on the screen with the exact action texts `Delete` and `Cancel`.

Clicking `Delete` in the little square should not delete the entry immediately. Before deleting a `Balance Changes` entry, the website should ask the user to confirm the delete action.

Clicking `Cancel` in the little square should close the little square without changing anything.

The confirmation message should be exactly `Delete this Balance Change?`.

The confirmation buttons should be exactly `Cancel` and `Delete`.

Deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

After a `Balance Changes` entry is deleted, the website should not offer undo.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it from the visible history. If the main money amount is wrong, the user should use `Modify` to correct the main money amount.

`Balance Changes` entries should not have notes. `Add` and `Subtract` should ask only for the money amount to add or subtract.

### Silent Modify Correction

The user can manually correct the total money amount by using `Modify` when the money amount is from `0.00$` through `999,999.99$`.

This is for setting the displayed total to the correct real cash amount when the website's current money amount no longer matches reality. The correction should not be confused with normal added or subtracted money.

`Modify` should allow a corrected money amount from `0.00$` through `999,999.99$`. It should not allow a negative money amount or a money amount above `999,999.99$`.

If the corrected money amount entered in `Modify` is the same as the money amount already shown, the website should treat it as no action. It should not close the input flow, change the money amount, save browser storage, create browser storage, create a `Balance Changes` entry, run `Balance Changes` cleanup, create a notification, or show a message.

If `Modify` changes the main money amount to `0.00$`, the next click on the main money amount should show `Add` and `Modify`, but should not show `Subtract`.

If `Modify` changes the main money amount to `999,999.99$`, the next click on the main money amount should show `Subtract` and `Modify`, but should not show `Add`.

### Savings

`Savings` is a separate planning section of the website.

A `Saving` is one user-named square inside the `Savings` section.

A `Saving` stores a planned money amount chosen by the user.

The user should open `Savings` by clicking `Savings` on the dashboard. When `Savings` opens, it should take the entire screen.

The full-screen `Savings` view should show a small `<` sign in the top-left corner. Clicking `<` should return the user to the dashboard, change nothing, save nothing, create no `Balance Changes` entry, and update no dates.

The full-screen `Savings` view should show the `Savings money amount` and the `Saving` squares. The money amount shown inside `Savings` should still start from the main money amount and go to the visible `Saving` squares from top to bottom as planning display only.

When `Savings` has no visible `Saving` squares yet, the `Saving` squares area should show only the circle `+` action centered in that area. The centered `+` action is the empty state for no `Saving` squares. The website should not show a separate empty-state sentence for no `Saving` squares.

After at least one `Saving` square exists, the circle `+` action should move to the top-left of the `Saving` squares area, above or before the visible squares, so the user can add more `Saving` squares.

In its default state, each visible `Saving` square should show the `Saving` name in the top-left corner, the planned money amount on the right side of the same top row, and the thin coverage bar at the bottom.

Clicking a `Saving` square should change that same square into its action state. In the action state, the square should still show the `Saving` name in the top-left corner and the planned money amount on the right side of the same top row, and it should show a `Delete` text action at the bottom center. The thin coverage bar should not be shown while the square is in the action state.

In the action state, clicking the `Saving` name should open the rename flow for that square. Clicking the planned money amount should open the planned-money-amount change flow for that square. Clicking `Delete` should start the delete confirmation for that square. The website should not open a separate action menu or larger action square for rename, planned-money-amount change, and delete.

Planned money amounts in `Saving` squares should use the planned-money amount display format: `0.00$` for zero, two decimal digits for nonzero whole money amounts such as `14.00$`, two decimal digits for nonzero money amounts with cents such as `14.50$`, and comma separators for planned money amounts of `1,000.00$` or more such as `5,895.50$`. Browser storage should save `Saving` square planned money amounts as normalized plain decimal strings without the `$` sign or comma separators, such as `14.50` or `5895.50`. A planned money amount should not be greater than `999,999.99$`.

If the user tries to save a `Saving` square planned money amount greater than `999,999.99$`, the website should do nothing. No message should appear, the `Saving` square should not be created or changed, saved data should stay unchanged, and `Balance Changes` should not get a new entry.

If the user tries to save a new `Saving` square without a planned money amount, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a planned money amount or cancels creating that square.

A `Saving` square name is required and must be unique inside `Savings`.

`Saving` square names should not have a maximum length. The user can enter a one-letter name, a number-only name such as `2026`, a name with numbers before or after words such as `2 Trip` or `Trip 2`, multiple words, a full sentence, symbols, punctuation, emoji characters, or a very long name.

The website should trim spaces at the beginning and end of a `Saving` square name before saving it. Spaces inside the trimmed name should stay because the user may write multiple words or sentences.

Very long `Saving` square names should be handled by the square layout. The website should wrap or contain long names so they do not create horizontal page overflow, overlap other square content, or break the `Saving` square layout. The website should not reject a valid `Saving` name only because it is long.

If the user tries to save a new `Saving` square without a `Saving` name, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a `Saving` name or cancels creating that square.

Two `Saving` squares cannot use the same name. Names that match after trimming spaces and ignoring uppercase or lowercase letters should count as the same name.

If the user tries to create or rename a `Saving` square with a duplicate name, the website should not save that duplicate name.

If the user enters a duplicate `Saving` name while creating or renaming a `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message. Nothing should change: no new `Saving` square should be created, an existing `Saving` square should keep its old name, saved data should stay unchanged, and `Balance Changes` should not get a new entry.

A `Saving` square should exist only when its planned money amount is greater than `0.00$` and not greater than `999,999.99$`.

If the user tries to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$`, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, `Balance Changes` should not get a new entry, and the same `Saving` square create input step should stay open until the user enters a planned money amount greater than `0.00$` and not greater than `999,999.99$` or cancels.

When a new `Saving` square create attempt has more than one invalid value, the website should check the create inputs in this order:

1. Planned money amount is missing, `0.00$`, or greater than `999,999.99$`.
2. `Saving` name is missing.
3. `Saving` name is duplicate.

The first matching rule decides what happens. This means a duplicate `Saving` name with a missing, `0.00$`, or above-limit planned money amount should keep the same `Saving` square create input step open with no message, no saved data change, no new `Saving` square, and no `Balance Changes` entry. The website should check duplicate names only after the planned money amount is greater than `0.00$`, not greater than `999,999.99$`, and the `Saving` name is not empty.

The user can use `Saving` squares to see what planned money is fully covered, partly covered, or not covered by the main money amount.

The money amount shown inside `Savings` should start from the main money amount and subtract the planned money amounts in `Saving` squares in their visible order. It should never go below `0.00$`.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`.

The `Savings money amount` label is only for the money amount shown inside `Savings`. The main money amount should still use the exact user-facing label `Current Balance`.

Each `Saving` square keeps the planned money amount chosen by the user. If the main money amount changes, the `Saving` square planned money amounts should not change automatically.

After a `Saving` square is created, the user can change that square's planned money amount by entering a new total planned money amount.

If the user tries to change an existing `Saving` square's planned money amount with an empty planned money amount or a planned money amount greater than `999,999.99$`, the website should do nothing. No message should appear, the old planned money amount should stay saved, saved data should stay unchanged, and the planned-money-amount change flow should stay open until the user enters a valid planned money amount or cancels changing that square.

If the user tries to rename an existing `Saving` square with an empty name, the website should do nothing. No message should appear, the old name should stay saved, saved data should stay unchanged, and the rename flow should stay open until the user enters a `Saving` name or cancels renaming that square.

If saved browser data can be loaded but one saved `Saving` square is broken, the website should keep the rest of the saved data and show that broken square in `Savings` with the exact text `Saving could not be loaded.` and the actions `Fix` and `Delete`.

A saved `Saving` square should count as broken if it has a missing or empty name, duplicate name, missing or invalid planned money amount, planned money amount of `0.00$` or less, planned money amount greater than `999,999.99$`, missing or invalid order, duplicate order, missing ID, or duplicate ID.

A broken `Saving` square should not use any money amount inside `Savings`, should not lower the `Savings money amount`, should not show a coverage bar, and should not affect coverage bars for valid `Saving` squares until the user fixes it.

Clicking `Fix` on a broken `Saving` square should ask the user for a valid `Saving` name and planned money amount greater than `0.00$` and not greater than `999,999.99$`. Saving a fixed `Saving` square should turn the broken square into a normal `Saving` square, keep it in the same visible position when possible, save the repaired data in browser storage, recalculate the `Savings money amount` and coverage bars, create no `Balance Changes` entry, and show no message.

Clicking `Delete` on a broken `Saving` square should ask for confirmation with the exact message `Delete this Saving?` and buttons exactly named `Cancel` and `Delete`. Confirming delete should remove only that broken square, save the change in browser storage, recalculate the `Savings money amount` and coverage bars, create no `Balance Changes` entry, update no other `Saving` squares, and offer no undo.

`Saving` square create, rename, planned-money-amount change, and broken-square fix input flows should show two text actions at the bottom: `Save` and `Cancel`.

In `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows, `Save` should try to save the entered values using the `Saving` square rules. `Cancel` should close the input flow, return to the `Saving` squares view, change nothing, save nothing, create no `Balance Changes` entry, update no dates, and show no message.

Changing a `Saving` square's planned money amount should replace the old planned money amount. It should not add to or subtract from the old planned money amount.

Changing a `Saving` square's planned money amount does not change the main money amount and should not create a `Balance Changes` entry.

If a `Saving` square's planned money amount becomes `0.00$`, the `Saving` square should disappear from `Savings` and should not stay saved as a `0.00$` square.

Removing a `Saving` square because its planned money amount became `0.00$` should not change the main money amount and should not create a `Balance Changes` entry.

`Saving` squares should have an order. The first `Saving` square the user creates should stay at the top by default. The user should be able to delete `Saving` squares and change their order by holding and moving the whole `Saving` square. Mouse users should be able to reorder by clicking, holding, and dragging the whole `Saving` square. Holding or dragging a `Saving` square should not open rename, planned-money-amount change, or delete.

The visible order is the coverage order. The top `Saving` square is checked first and gets first priority for coverage. If the user moves a `Saving` square to the top, that square is checked before the squares below it.

When the user finishes moving a `Saving` square and lets go, the website should save the new order in browser storage.

After the move finishes, the money amount shown inside `Savings` and the coverage bars should recalculate from the new visible order.

Reordering `Saving` squares should not change the main money amount and should not create a `Balance Changes` entry.

When the user deletes a `Saving` square:

- The website should ask the user to confirm the delete action before deleting the `Saving` square.
- The confirmation message should be exactly `Delete this Saving?`.
- The confirmation buttons should be exactly `Cancel` and `Delete`.
- Only that `Saving` square and the details inside it should be removed.
- The main money amount should not change.
- `Balance Changes` should not get a new entry.
- Other `Saving` squares should keep their names, planned money amounts, and relative order.
- The money amount shown inside `Savings` and the coverage bars should update from the remaining `Saving` squares because they are calculated display values.
- After the `Saving` square is deleted, the website should not offer undo.

The website should check `Saving` squares from top to bottom using the main money amount:

- If enough money is available for a `Saving` square, that square is fully covered.
- If only part of the money is available for a `Saving` square, that square is partly covered.
- If no money is available for a `Saving` square, that square is not covered.

Each `Saving` square should show a thin horizontal coverage bar at the very bottom of the square in its default state. The bar should be grey by default and fill green from left to right based on how much of that square's planned money amount is covered.

If a `Saving` square is 80% covered, the left 80% of its coverage bar should be green and the right 20% should stay grey.

If a `Saving` square is not fully covered, the square should show a small `{money amount} needed` note at the top-left of the bottom coverage bar.

The user should add a new `Saving` square by clicking the circle action with a `+` sign. Clicking the `+` should open the same create flow whether the `+` is centered in an empty `Saving` squares area or placed at the top-left when `Saving` squares already exist.

Before a new `Saving` square is created, the user should enter the `Saving` name and a planned money amount greater than `0.00$` and not greater than `999,999.99$` for that square.

If the user tries to save without a planned money amount, the website should do nothing until the user enters a planned money amount or cancels.

If the user does not enter both a valid name and a planned money amount greater than `0.00$` and not greater than `999,999.99$`, the `Saving` square should not be created.

If the planned money amount is `0.00$` or greater than `999,999.99$` while creating a new `Saving` square, the same `Saving` square create input step should stay open with no message, no saved data change, and no `Balance Changes` entry.

If the planned money amount is missing, `0.00$`, or greater than `999,999.99$` while creating a new `Saving` square, that planned-money-amount rule should happen before any duplicate-name rule.

The user should not be able to paste into `Saving` square planned money amount inputs. If the user tries to paste letters, numbers, symbols, or any other content into a planned money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear.

The planned money amount in a `Saving` square does not mean money has moved into a separate place. It does not subtract money and does not lower the main money amount. It only changes planning inside the `Savings` section.

`Savings` is not a separate real account and not a separate goals feature.

## User Experience Requirements

- The first screen should look like a simple bank dashboard.
- The current money amount should be the most visible information.
- Main money actions and `Savings` should be easy to find.
- The website should feel trustworthy, calm, and practical.
- The interface should work well on mobile.
- The user should not need financial knowledge to use it.

## Important Trust Requirement

The website may use a bank-account style interface, but it must not pretend to be a real bank.

The website should not ask for personal information from the user. It is a manual cash tracking tool, not a bank account or financial account.

The MVP should not include online accounts, personal information collection, or real banking capabilities.

It should avoid misleading wording such as:

- Deposit to bank.
- Withdraw from bank.
- Real account number.
- Card balance.
- Bank transfer.

Better wording:

- Add money.
- Subtract money.
- Modify amount.
- Current Balance.
- Savings money amount.
- Saved cash.
- Manual cash tracker.

## Success Criteria

- Users can add money when the money amount is less than `999,999.99$`, can use `Modify` at any valid money amount, and can use `Subtract` after the money amount is greater than `0.00$`.
- Users can make many changes in one day without hitting a usage limit.
- Users can return after a long time and still understand or update their money amount.
- Users can review `Balance Changes` to understand how their money amount changed.
- Users can use `Savings` to plan money visually without lowering the main money amount.
- Users can understand that `Savings` is a planning section, not a real account or separate goals feature.
- Users can understand that the website is a manual cash tracker, not a real bank.

## Remaining Grey Zones

- Grey zone: The `Saving` square create and fix input display is not fully defined. The specs say the website should ask for a `Saving` name and planned money amount, but they do not define whether that input appears inside a square, in a modal, in a bottom sheet, or in another layout.
- Grey zone: The exact `Saving` squares layout across mobile and desktop is not fully defined. The specs define what goes inside each square and say coverage uses top-to-bottom order, but they do not define whether the squares should appear in one column, multiple columns, or how coverage order should work if a grid layout is used.
- Grey zone: The circle `+` position when `Savings` contains only broken saved `Saving` squares is not fully defined. The specs say the `+` is centered when there are no visible `Saving` squares and moves to the top-left after at least one `Saving` square exists, but they do not define whether a broken error square counts as a visible `Saving` square for this placement rule.
- Grey zone: The saved `Saving` square created date and updated date are not fully defined. The data model includes created date and updated date, but the specs do not fully define whether those dates are needed in the first version, whether they are visible or internal only, and exactly when they should update for create, rename, planned-money-amount change, reorder, broken-square fix, or delete.
- Grey zone: The exact website name and trust label are not fully defined. The active plan suggests wording such as `Manual cash tracker`, but the specs do not define the exact website name or the exact short trust label shown to the user.
- Grey zone: Browser back behavior from full-screen `Savings` is not fully defined. The specs define returning to the dashboard with the small `<` sign, but they do not define what should happen if the user uses the browser back button while `Savings` is open.

### Main Money Amount Grey Zones

- Grey zone: The main money amount circle remains visible behind the input flow, but its behavior while the input flow is open is not fully defined. The outside-cancel rule may imply that tapping the visible main money amount circle acts like `Cancel`, but the specs do not explicitly say whether that tap cancels, does nothing, or opens the action buttons again.
- Grey zone: The all-users `Cent` button placement outside mobile is not fully defined. The specs say the `Cent` button shows for all users while a main money amount input is open and appears directly above the mobile keyboard when possible, but they do not define where the button should appear on desktop or large screens.
