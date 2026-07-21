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

Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`.

The website should not ask for a starting amount before showing the dashboard.

The website should not create saved browser data only because it shows the default `0.00$` money amount.

The default `0.00$` money amount should not create a `Balance Changes` entry.

When the money amount is `0.00$`, clicking the main money amount should show only the `Add` action.

After the user adds money and the money amount is greater than `0.00$`, clicking the main money amount should show `Add`, `Subtract`, and `Modify`.

The main money actions should appear as buttons lined together horizontally near the main money amount. When all three actions are visible, the left-to-right order should be `Add`, `Subtract`, and `Modify`.

The main money action buttons should not appear as a modal, bottom sheet, separate page, or large expanded area.

If the main money action buttons are visible and the user clicks or taps anywhere else on the screen, the buttons should go away without changing the money amount, saving anything, creating a `Balance Changes` entry, or showing a message.

The website should make the main money actions easy to understand without using real banking language.

### Storage Scope

The first version should save the user's data only in browser storage.

The implementation should use the stable storage key `cash-money-organizer-website-data` and the first data version value `1`.

The first version should load saved browser data only when the data version value is exactly `1`. Saved browser data with a missing, wrong, future, unreadable, or unrecognized data version should be treated as broken saved data.

If saved browser data is broken or cannot be read, the website should show `Saved data could not be loaded.` with a `Start again` action. When the user chooses `Start again`, the website should delete the broken saved data and immediately create fresh browser storage data with the money amount set to `0.00$`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`.

When no saved data exists yet, the website should show the default `0.00$` money amount without saving it just because the website opened. The website should start saving data after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

Browser storage is an internal implementation detail. The website should not tell the user where saved information is stored.

The website should not tell the user that saved information belongs to the same browser or device.

The website should not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

### Add and Subtract Money Amount

The user can quickly add money to the money amount or subtract money from the money amount when their real cash changes.

This is for normal cash movements, such as receiving cash, spending cash, or removing cash from the tracked amount. It is not the same as correcting the full money amount after the displayed amount is wrong.

The website should support users who update their money amount rarely and users who update it many times in one day.

The money amount should never become negative.

The `Add` action should be available when the money amount is `0.00$` or more.

The `Subtract` action should be available only when the money amount is greater than `0.00$`.

`Add` and `Subtract` should accept only money amounts greater than `0.00$`.

Negative money amounts should be blocked for all money actions.

When the user starts a main money amount input for `Add`, `Subtract`, or `Modify`, the website should show a horizontal money amount input square in the middle of the screen.

The main money amount circle should stay visible in the background while the horizontal input square is open.

The horizontal input square should start by showing `0.00$`.

The user should type only numbers from `0` through `9` into the horizontal input square. The user should not type the decimal point or the `$` sign.

The horizontal input square should automatically format the typed number as a whole money amount with `.00$` at the end. For example, if the user types `5`, the square should show `5.00$`. If the user types `50`, the square should show `50.00$`.

If the user deletes one typed number from the horizontal input square, the remaining typed numbers should be formatted again with `.00$`. For example, if the square shows `50.00$` and the user deletes the `0`, the square should show `5.00$`.

If the user deletes all typed numbers from the horizontal input square, the square should return to `0.00$`. The horizontal input square should not become empty.

If the user types letters, a minus sign, a decimal point, the `$` sign, or any other blocked character into a main money amount input, the field should not change and no message should appear.

The user should not be able to paste into main money amount inputs. If the user tries to paste letters, numbers, symbols, or any other content into a main money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear.

Under the horizontal input square, the website should show the exact text `Save Changes`.

Under `Save Changes`, the website should show two buttons exactly named `Yes` and `Cancel`.

Clicking `Yes` should try to apply the typed money amount using the selected action. For `Add`, the typed money amount should be added to the main money amount. For `Subtract`, the typed money amount should be subtracted from the main money amount. For `Modify`, the typed money amount should replace the main money amount.

Clicking `Cancel` should close the horizontal input square, return to the dashboard money amount view, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking or tapping outside the horizontal input square and its `Save Changes`, `Yes`, and `Cancel` controls should act like `Cancel`.

If the user clicks `Yes` while the input is `0.00$` in `Add` or `Subtract`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a money amount greater than `0.00$` or cancels.

If the user clicks `Yes` while the input is `0.00$` in `Modify`, the website should replace the main money amount with `0.00$`.

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

The user can manually correct the total money amount by using `Modify` after the money amount is greater than `0.00$`.

This is for setting the displayed total to the correct real cash amount when the website's current money amount no longer matches reality. The correction should not be confused with normal added or subtracted money.

`Modify` should allow `0.00$` or more, but should not allow a negative money amount.

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

Planned money amounts in `Saving` squares should use the planned-money amount display format: `0.00$` for zero, two decimal digits for nonzero whole money amounts such as `14.00$`, and two decimal digits for nonzero money amounts with cents such as `14.50$`.

If the user tries to save a new `Saving` square without a planned money amount, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a planned money amount or cancels creating that square.

A `Saving` square name is required and must be unique inside `Savings`.

`Saving` square names should not have a maximum length. The user can enter a one-letter name, a number-only name such as `2026`, a name with numbers before or after words such as `2 Trip` or `Trip 2`, multiple words, a full sentence, symbols, punctuation, emoji characters, or a very long name.

The website should trim spaces at the beginning and end of a `Saving` square name before saving it. Spaces inside the trimmed name should stay because the user may write multiple words or sentences.

Very long `Saving` square names should be handled by the square layout. The website should wrap or contain long names so they do not create horizontal page overflow, overlap other square content, or break the `Saving` square layout. The website should not reject a valid `Saving` name only because it is long.

If the user tries to save a new `Saving` square without a `Saving` name, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a `Saving` name or cancels creating that square.

Two `Saving` squares cannot use the same name. Names that match after trimming spaces and ignoring uppercase or lowercase letters should count as the same name.

If the user tries to create or rename a `Saving` square with a duplicate name, the website should not save that duplicate name.

If the user enters a duplicate `Saving` name while creating or renaming a `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message. Nothing should change: no new `Saving` square should be created, an existing `Saving` square should keep its old name, saved data should stay unchanged, and `Balance Changes` should not get a new entry.

A `Saving` square should exist only when its planned money amount is greater than `0.00$`.

If the user tries to save a new `Saving` square with a planned money amount of `0.00$`, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, `Balance Changes` should not get a new entry, and the same `Saving` square create input step should stay open until the user enters a planned money amount greater than `0.00$` or cancels.

When a new `Saving` square create attempt has more than one invalid value, the website should check the create inputs in this order:

1. Planned money amount is missing or `0.00$`.
2. `Saving` name is missing.
3. `Saving` name is duplicate.

The first matching rule decides what happens. This means a duplicate `Saving` name with a missing or `0.00$` planned money amount should keep the same `Saving` square create input step open with no message, no saved data change, no new `Saving` square, and no `Balance Changes` entry. The website should check duplicate names only after the planned money amount is greater than `0.00$` and the `Saving` name is not empty.

The user can use `Saving` squares to see what planned money is fully covered, partly covered, or not covered by the main money amount.

The money amount shown inside `Savings` should start from the main money amount and subtract the planned money amounts in `Saving` squares in their visible order. It should never go below `0.00$`.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`.

The `Savings money amount` label is only for the money amount shown inside `Savings`. The main money amount should still use the exact user-facing label `Current Balance`.

Each `Saving` square keeps the planned money amount chosen by the user. If the main money amount changes, the `Saving` square planned money amounts should not change automatically.

After a `Saving` square is created, the user can change that square's planned money amount by entering a new total planned money amount.

If the user tries to change an existing `Saving` square's planned money amount with an empty planned money amount, the website should do nothing. No message should appear, the old planned money amount should stay saved, saved data should stay unchanged, and the planned-money-amount change flow should stay open until the user enters a planned money amount or cancels changing that square.

If the user tries to rename an existing `Saving` square with an empty name, the website should do nothing. No message should appear, the old name should stay saved, saved data should stay unchanged, and the rename flow should stay open until the user enters a `Saving` name or cancels renaming that square.

If saved browser data can be loaded but one saved `Saving` square is broken, the website should keep the rest of the saved data and show that broken square in `Savings` with the exact text `Saving could not be loaded.` and the actions `Fix` and `Delete`.

A saved `Saving` square should count as broken if it has a missing or empty name, duplicate name, missing or invalid planned money amount, planned money amount of `0.00$` or less, missing or invalid order, duplicate order, missing ID, or duplicate ID.

A broken `Saving` square should not use any money amount inside `Savings`, should not lower the `Savings money amount`, should not show a coverage bar, and should not affect coverage bars for valid `Saving` squares until the user fixes it.

Clicking `Fix` on a broken `Saving` square should ask the user for a valid `Saving` name and planned money amount greater than `0.00$`. Saving a fixed `Saving` square should turn the broken square into a normal `Saving` square, keep it in the same visible position when possible, save the repaired data in browser storage, recalculate the `Savings money amount` and coverage bars, create no `Balance Changes` entry, and show no message.

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

Before a new `Saving` square is created, the user should enter the `Saving` name and a planned money amount greater than `0.00$` for that square.

If the user tries to save without a planned money amount, the website should do nothing until the user enters a planned money amount or cancels.

If the user does not enter both a valid name and a planned money amount greater than `0.00$`, the `Saving` square should not be created.

If the planned money amount is `0.00$` while creating a new `Saving` square, the same `Saving` square create input step should stay open with no message, no saved data change, and no `Balance Changes` entry.

If the planned money amount is missing or `0.00$` while creating a new `Saving` square, that planned-money-amount rule should happen before any duplicate-name rule.

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

- Users can add money whenever they need to, and can use `Modify` or `Subtract` after the money amount is greater than `0.00$`.
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

## Main Money Amount Grey Zones

- Grey zone: Very large main money amount inputs are not fully defined. The specs do not define whether there is a maximum number of typed digits or how the horizontal input and dashboard should behave if the user types a very large money amount.
