# Global Spec: Cash Money Organizer Website

## For who?

This website is for people who keep or manage money in cash and have difficulty organizing it, tracking it, and keeping it under control.

Primary users include:

- People who want to manually track their money amount.
- Students or young adults managing pocket money.
- Small informal sellers or service providers.
- People who do not use a bank account regularly.
- People who want a simple, visual way to update and understand how much cash they have.

## What?

The product is a personal cash management website that looks and feels similar to an online bank account, but it does not hold real money, does not connect to banks, and does not collect personal information.

Users manually enter and update their own money amount. The website displays a bank-style dashboard with:

- Main money amount shown with the label `Current Balance`.
- `Balance Changes` directly under the main money amount.
- Separate add and subtract history entries.
- `Savings` section with the label `Savings money amount`, ordered user-named `Saving` squares, and coverage bars.

The main money amount is clickable. When the money amount is `0.00$`, clicking it shows `Add` and `Modify`, but not `Subtract`. When the money amount is greater than `0.00$` and less than `999,999.99$`, clicking it shows `Add`, `Subtract`, and `Modify`. When the money amount is exactly `999,999.99$`, clicking it shows `Subtract` and `Modify`, but not `Add`. The visible main money actions should be buttons lined together horizontally near the main money amount. When `Subtract` is hidden at `0.00$`, the visible order should be `Add`, then `Modify`. When all three actions are visible, the left-to-right order should be `Add`, `Subtract`, and `Modify`. When `Add` is hidden at `999,999.99$`, the visible order should be `Subtract`, then `Modify`. The main money action buttons should not appear as a modal, bottom sheet, separate page, or large expanded area. If the main money action buttons are visible and the user clicks or taps the main money amount again, the buttons should go away without changing the money amount, saving anything, creating a `Balance Changes` entry, or showing a message. If the main money action buttons are visible and the user clicks or taps outside the main money amount and outside the visible action buttons, the buttons should go away without changing the money amount, saving anything, creating a `Balance Changes` entry, or showing a message. Clicking `Add`, `Subtract`, or `Modify` should start that selected money amount input flow and should not be treated as an outside click.

The user opens `Savings` by clicking `Savings`. There should not be a separate `Open savings` action. When opened, `Savings` should take the entire screen and show the `Savings money amount` with the `Saving` squares. A small `<` sign should appear in the top-left corner of the full-screen `Savings` view. Clicking `<` should return the user to the dashboard without changing anything, saving anything, creating a `Balance Changes` entry, or updating dates.

`Savings` is the separate planning section of the website. A `Saving` is one user-named square inside the `Savings` section.

In its default state, each visible `Saving` square should show the `Saving` name in the top-left corner, the planned money amount on the right side of the same top row, and the thin coverage bar at the bottom. Clicking a `Saving` square should change that same square into its action state. In the action state, the square should still show the `Saving` name and planned money amount, should show `Delete` at the bottom center, and should hide the thin coverage bar. In the action state, clicking the `Saving` name should open rename, clicking the planned money amount should open planned-money-amount change, and clicking `Delete` should start delete confirmation. The website should not use a separate action menu or larger action square for those three actions.

The user creates a `Saving` square by clicking the circle `+` action in `Savings`, then entering the square name and planned money amount before the square appears.

When no `Saving` squares exist, the circle `+` action should appear centered in the `Saving` squares area and serve as the empty state. The website should not show a separate empty-state sentence for no `Saving` squares. After at least one `Saving` square exists, the circle `+` action should appear at the top-left of the `Saving` squares area, above or before the squares, so the user can add more `Saving` squares.

If the user tries to save a new `Saving` square without a planned money amount, the website should do nothing: no message, no new square, no saved data change, and the create flow stays open until the user enters a planned money amount or cancels.

A `Saving` square name is required and must be unique inside `Savings`. The website should not create or rename a `Saving` square with the same name as another `Saving` square, using trimmed names and ignoring uppercase or lowercase letters when checking for duplicates.

`Saving` square names should not have a maximum length. The user can use one letter, only numbers, numbers before or after words, multiple words, full sentences, symbols, punctuation, emoji characters, or very long names. The website should trim spaces at the beginning and end before saving the name, but spaces inside the trimmed name should stay.

Very long `Saving` square names should wrap or otherwise stay contained inside the square layout. A long name should not create horizontal page overflow, overlap other square content, or be rejected only because of its length.

If the user tries to save a new `Saving` square without a `Saving` name, the website should do nothing: no message, no new square, no saved data change, and the create flow stays open until the user enters a name or cancels.

If the user tries to rename an existing `Saving` square with an empty name, the website should do nothing: no message, no saved data change, and the rename flow stays open until the user enters a name or cancels.

If the user enters a duplicate `Saving` name while creating or renaming a `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message. Nothing should change.

If saved browser data can be loaded but one saved `Saving` square is broken, the website should keep the rest of the saved data and show that broken square in `Savings` with the exact text `Saving could not be loaded.` and actions exactly named `Fix` and `Delete`. A broken `Saving` square should not use money amount inside `Savings`, should not lower the `Savings money amount`, should not show a coverage bar, and should not affect coverage bars for valid `Saving` squares until fixed. `Fix` should ask for a valid `Saving` name and planned money amount greater than `0.00$` and not greater than `999,999.99$`. A fixed `Saving` square should stay in the same visible position when possible and get a valid unique ID and valid order if needed. `Delete` should ask for confirmation with `Delete this Saving?`, `Cancel`, and `Delete`. Fixing or deleting a broken `Saving` square should save the change, recalculate the `Savings money amount` and coverage bars, create no `Balance Changes` entry, and update no other `Saving` squares.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`. This label is separate from `Current Balance`, which is only for the main money amount.

The `Savings` section checks `Saving` squares from top to bottom against the main money amount. In easy terms, the money amount shown inside `Savings` goes to the visible `Saving` squares in order until no more money amount is left to plan. Each square keeps the planned money amount chosen by the user and shows a thin bottom coverage bar in its default state so the user can see whether that square is fully covered, partly covered, or not covered. The bar is grey by default and fills green from left to right as coverage increases. If a square is not fully covered, its `{money amount} needed` note appears at the top-left of the bottom coverage bar.

The visible order is the coverage order. If the user moves a `Saving` square to the top, that square is checked first, so its coverage bar fills from the main money amount before lower squares use what remains. Touch users should reorder by holding and moving the whole `Saving` square. Mouse users should reorder by clicking, holding, and dragging the whole `Saving` square. Holding or dragging a `Saving` square should not open rename, planned-money-amount change, or delete. When the user finishes moving a square and lets go, the website saves the new order and recalculates the `Savings` display from that order.

The planned money amount in a `Saving` square does not mean money has moved into a separate place. It does not lower the main money amount.

After a `Saving` square is created, changing its planned money amount replaces the old planned money amount with a new total planned money amount. It is not an add or subtract action and does not change the main money amount.

If the user tries to change an existing `Saving` square's planned money amount with an empty planned money amount or a planned money amount greater than `999,999.99$`, the website should do nothing: no message, no saved data change, and the change flow stays open until the user enters a valid planned money amount or cancels.

A `Saving` square should not be created or saved with a `0.00$` planned money amount or a planned money amount greater than `999,999.99$`. If the user tries to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$`, nothing should happen: no message, no new square, no saved data change, no `Balance Changes` entry, and the same `Saving` square create input step stays open until the user enters a planned money amount greater than `0.00$` and not greater than `999,999.99$` or cancels. If an existing `Saving` square's planned money amount becomes `0.00$`, the square should disappear from `Savings`.

When a new `Saving` square create attempt has more than one invalid value, the website should check the planned money amount first, the missing `Saving` name second, and the duplicate `Saving` name third. If the planned money amount is missing, `0.00$`, or greater than `999,999.99$`, the same `Saving` square create input step should stay open with no message, no saved data change, no new square, and no `Balance Changes` entry, even if the entered name is also duplicate.

Deleting a `Saving` square removes only that square and its saved details. It does not change the main money amount, `Balance Changes`, or the other `Saving` squares. The money amount shown inside `Savings` and coverage bars update from the remaining squares because they are calculated display values.

Deleting a `Saving` square should ask for confirmation with the exact message `Delete this Saving?` and buttons exactly named `Cancel` and `Delete`.

There is no separate `View history` action. Money amount change history stays under the main money amount, and the user can scroll down if there are too many entries to fit on the screen.

Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`. Visible money amounts of `1,000.00$` or more should use comma separators every three digits before the decimal point, starting from the right, such as `5,895.50$` and `999,999.99$`.

Saved browser storage money amount values should use normalized plain decimal strings without the `$` sign or comma separators, such as `0.00`, `5.00`, `5895.50`, and `999999.99`. User-facing website display should still add comma separators and the `$` sign, such as `5,895.50$`.

Money amount values can include cents, but the maximum allowed money amount is `999,999.99$`. The main money amount and each `Saving` square planned money amount should never be saved above `999,999.99$`. Examples of valid visible money amounts include `5.05$`, `0.05$`, and `999,999.99$`. Values above `999,999.99$` are invalid.

When no saved money amount exists yet, the website starts with the money amount at `0.00$` and shows the dashboard. This default `0.00$` amount does not create a `Balance Changes` entry.

Opening the website with the default `0.00$` money amount should not create saved browser data by itself. The website should start saving after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The user clicks the money amount and uses `Add` to add money while the money amount is below `999,999.99$`. Users can use `Modify` at `0.00$` or any other valid money amount. After the money amount is greater than `0.00$` and less than `999,999.99$`, users can update the amount with `Add`, `Subtract`, and `Modify`. At exactly `999,999.99$`, users can still use `Subtract` and `Modify`, but cannot use `Add`.

Main money action inputs for `Add`, `Subtract`, and `Modify` should open as a horizontal money amount input square in the middle of the screen, with the main money amount circle still visible in the background. The square should start by showing `0.00` without the `$` sign. The user should type only the money amount, without the `$` sign. Main money amount inputs should request a mobile keyboard suitable for digit entry on supported devices. All users should be able to enter cents with `Space` or with a rectangular `Cent` button shown as part of the main money amount input controls while a main money amount input is open. On mobile, the `Cent` button should appear directly above the mobile keyboard when possible. The `Cent` button should behave the same as pressing `Space`, should not add visible text to the input, and should not cancel or close the input flow. The user should not enter cents in main money amount inputs by typing a decimal point.

Main money amount inputs should accept only numbers from `0` through `9` and separator input from the `Space` key or `Cent` button. Typed decimal points, comma separators, `$` signs, letters, minus signs, starting separators, consecutive separators, additional separators after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, paste, and other blocked input should not change the field and should show no message. The decimal point and comma separators in the displayed input should be generated by the website only.

For accepted input without separator input, all digits are whole money amount digits: typing `5` should show `5.00`, `58` should show `58.00`, `589` should show `589.00`, `5895` should show `5,895.00`, `58955` should show `58,955.00`, and `589550` should show `589,550.00`. `0` is a normal digit, not a starting zero-position skip, and unneeded leading zeros in the whole money amount should be normalized away, so typing `0005` should show `5.00`.

The `Space` key and `Cent` button should act as a non-visible separator between digit groups. A separator should be accepted only after at least one digit and only when the previous accepted input is a digit. If the input ends with a separator, all completed digit groups should be treated as the whole money amount and cents should show as `00`. If the final group after a separator has one or two digits, that final group should be treated as cents and should be left-padded with `0` when it has one digit. All earlier groups should be joined together as the whole money amount. If there is only one accepted separator and the final group grows to three or more digits, that final group should be treated as another whole money amount group and cents should show as `00`. Once the input has two accepted separators, the final group is the cents group and should accept at most two digits. Additional `Space` key presses or `Cent` taps after two accepted separators should be blocked with no message.

Examples: typing `5`, then `Space`, keeps the display at `5.00`; typing `5`, then `Space`, then `5` shows `5.05`; typing `5`, then `Space`, then `50` shows `5.50`; typing `58`, then `Space`, then `430` shows `58,430.00`; typing `58`, then `Space`, then `430`, then `Space`, then `88` shows `58,430.88`; typing `0`, then `Space`, then `5` shows `0.05`; typing `999999`, then `Space`, then `99` shows `999,999.99`; and typing `Space`, `Space`, `Space`, then `5` blocks the three `Space` key presses and then shows `5.00` after the `5` is typed. Tapping or clicking `Cent` should work the same as pressing `Space`, so typing `5`, then choosing `Cent`, then typing `50` should show `5.50`.

If the user deletes one typed character, the website should remove the last accepted digit or accepted separator, then format the remaining accepted input again with two digits after the decimal point, automatic comma separators when needed, and no `$` sign. If the user deletes all typed numbers, the square should return to `0.00` and should not become empty.

The main money amount input is append-only: focus is allowed, but users should not move the cursor into the formatted value, select part of it, replace selected text, or edit generated comma separators or the generated decimal point. Accepted typing goes to the end, and delete removes only the last accepted digit or accepted separator.

If an `Add` action would make the main money amount greater than `999,999.99$`, the website should do nothing: no message, no saved data change, no `Balance Changes` entry, and the same money amount input step stays open. Money amounts up to `999,999.99$` should stay editable in the horizontal input square with required comma separators while typing and readable on the dashboard with required comma separators after saving, without horizontal page overflow, overlapping other content, or hiding actions. Under the square, the website should show `Save Changes`, with buttons exactly named `Yes` and `Cancel`. `Yes` should try to apply the selected action, `Cancel` should close without changing anything, and clicking or tapping outside the horizontal input square, `Save Changes`, `Yes`, `Cancel`, and the `Cent` button should act like `Cancel`.

If the user clicks `Yes` while the main money action input is `0.00` in `Add` or `Subtract`, nothing should happen. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a valid money amount greater than `0.00$` or cancels.

`Saving` square planned money amount inputs may use a decimal point for cents, such as `14.56`, but the planned money amount should not be greater than `999,999.99$`. `Saving` square planned money amount inputs should request a decimal numeric keyboard on devices that support it, so the user gets number keys `0` through `9` and a decimal point. `Saving` square planned money amount inputs should ignore letters while the user types, and the first accepted character must be a number from `0` through `9`. The user should not type the `$` sign or comma separators into `Saving` square planned money amount inputs. Blocked typed characters should not appear in the field, and no message should appear for those blocked typed characters. The user should not be able to paste into `Saving` square planned money amount inputs. If the user tries to paste letters, numbers, symbols, or any other content into those inputs, the pasted content should not appear, the input should keep its previous value, and no message should appear. The website should save that planned money amount as a normalized plain decimal string such as `14.56` or `5895.50`, not as integer cents like `1456` and not with the `$` sign or comma separators. The website should show the planned money amount as `14.56$`, or as `5,895.50$` for thousands and larger values. If the user tries to save a planned money amount greater than `999,999.99$`, nothing should happen: no message, no new or changed `Saving` square, no saved data change, and no `Balance Changes` entry.

`Saving` square create, rename, planned-money-amount change, and broken-square fix input flows should show two text actions at the bottom: `Save` and `Cancel`. `Save` should try to save the entered values using the rules for that action. `Cancel` should close the input flow, return to the view the user was already using, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

`Add` and `Subtract` actions should appear as separate visible history entries for 30 days. They should not be combined into only one net result.

If there are no `Balance Changes` entries, the history list should simply be empty. The website should not show a sentence, placeholder, icon, or other empty-state content for empty `Balance Changes`.

Each visible `Balance Changes` row should show `+{money amount} added` or `-{money amount} subtracted`, plus both the created date and created time for that entry. A date-only display is not enough. The visible date and time format should be like `July 21, 2026 at 3:45 PM`. It should not show the previous money amount or new money amount.

Each visible `Balance Changes` entry should be compact. The signed money amount and action text should sit in the top-left corner. The visible created date and created time should sit in the bottom-right corner, below the money change text and not too far from it. Mobile should keep the same layout, with text wrapping only as needed to stay readable while keeping those same top-left and bottom-right positions.

Visible `Balance Changes` dates and times should use the user's browser/device local date and time.

The newest `Balance Changes` entry should appear first.

Deleting a `Balance Changes` entry should start when touch users press and hold the entry, or when mouse users click and hold the entry. A little square should pop up on the screen with the exact action texts `Delete` and `Cancel`. Clicking `Cancel` should close the little square without changing anything. Clicking `Delete` should ask for confirmation with the exact message `Delete this Balance Change?` and buttons exactly named `Cancel` and `Delete`.

`Add`, `Subtract`, and `Balance Changes` entries should not have notes.

For visible `Balance Changes` history, one month means 30 days, not a calendar month. The user should not choose dates for `Add` or `Subtract` entries. The website should show each entry's created date and created time to the user in the format `July 21, 2026 at 3:45 PM`. Created dates, created times, and internal visible-until dates and times should use the user's browser/device local date and time. The website should use the internal visible-until date and time only to delete visible `Balance Changes` entries from browser storage after 30 days. Each visible-until date should be calculated from the created date and time plus 30 days.

The website should clean old `Balance Changes` entries when it opens and loads saved data, and after every successful saved user action. During cleanup, entries at or after their visible-until date and time should be deleted from browser storage and removed from the visible history list. The first version does not need a background timer while the website stays open with no user action.

`Modify` silently corrects the current money amount when the money amount is from `0.00$` through `999,999.99$`. It should allow a corrected money amount from `0.00$` through `999,999.99$`, should not allow values above `999,999.99$`, and should not create history or notification entries. If the user clicks `Yes` in `Modify` with the same money amount that is already shown, the website should do nothing: no message, no money amount change, no saved data change, no browser storage creation or update, no `Balance Changes` entry, no `Balance Changes` cleanup, and the same money amount input step stays open until the user enters a different valid money amount or cancels. If `Modify` changes the main money amount to `0.00$`, the next click on the main money amount should show `Add` and `Modify`, but not `Subtract`. If `Modify` changes the main money amount to `999,999.99$`, the next click on the main money amount should show `Subtract` and `Modify`, but not `Add`.

The money amount should never become negative. If the user subtracts more than the current money amount, the website should set the money amount to `0.00$`.

The first version saves the user's money amount and money changes without requiring an email account, login, server database, or online sync.

Saved browser data should load only when the data version value is exactly `1`. A saved current money amount should load only when it is a normalized plain decimal string from `0.00` through `999999.99`, with no `$` sign, no comma separators, exactly two digits after the decimal point, and no unneeded leading zeros before the decimal point except the single `0` in `0.00`. If saved browser data has a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside that range or not in that exact saved format, the website should treat it as broken saved data, show `Saved data could not be loaded.`, and show a `Start again` action. Examples of broken saved current money amount values include `5`, `5.0`, `005.00`, `5,000.00`, and `5.000`. When the user chooses `Start again`, the website should delete the broken saved data and immediately create fresh browser storage data with the saved money amount set to `0.00`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`.

The website should not tell the user where saved information is stored, and should not warn that saved information may disappear after changing browser, changing device, using private browsing, or clearing browser data.

The website must clearly communicate that it is a personal tracking tool, not a real bank account.

## Why?

Cash is easy to spend because it is not automatically tracked. Users may forget how much money they received, how much they spent, and what cash they meant to keep aside.

The website helps users:

- See their money amount with a familiar bank-style `Current Balance` label.
- Manually modify their money amount at any valid money amount, using any corrected value from `0.00$` through `999,999.99$`.
- Add money and subtract money while keeping each change visible on its own.
- Correct mistakes with `Modify` at any valid money amount, without saving or showing a correction entry.
- Correct their money amount with `Modify` at any valid money amount, including correcting it to `999,999.99$`.
- Plan savings with user-named `Saving` squares that show covered and needed money without lowering the main money amount.
- Understand where their cash is going.

The goal is to make cash feel organized, visible, and easier to control.

## Where?

The website is used in a browser on desktop and mobile.

Users should be able to open it quickly when they want to check or modify their money amount, especially from a phone.

Users may update their money rarely or many times in one day. The website should not require a specific update schedule.

If the user closes the website and opens it again later, their saved money amount and 30-day visible history should still appear when the saved data is available.
