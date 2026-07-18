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

The main money amount is clickable. When the money amount is `0$`, clicking it shows only `Add`. When the money amount is greater than `0$`, clicking it shows `Add`, `Subtract`, and `Modify`.

The user opens `Savings` by clicking `Savings`. There should not be a separate `Open savings` action.

`Savings` is the separate planning section of the website. A `Saving` is one user-named square inside the `Savings` section.

The user creates a `Saving` square by clicking the `+` action in `Savings`, then entering the square name and planned money amount before the square appears.

If the user tries to save a new `Saving` square without a planned money amount, the website should do nothing: no message, no new square, no saved data change, and the create flow stays open until the user enters a planned money amount or cancels.

A `Saving` square name is required and must be unique inside `Savings`. The website should not create or rename a `Saving` square with the same name as another `Saving` square, using trimmed names and ignoring uppercase or lowercase letters when checking for duplicates.

If the user tries to save a new `Saving` square without a `Saving` name, the website should do nothing: no message, no new square, no saved data change, and the create flow stays open until the user enters a name or cancels.

If the user tries to rename an existing `Saving` square with an empty name, the website should do nothing: no message, no saved data change, and the rename flow stays open until the user enters a name or cancels.

If the user enters a duplicate `Saving` name while creating or renaming a `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message. Nothing should change.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`. This label is separate from `Current Balance`, which is only for the main money amount.

The `Savings` section checks `Saving` squares from top to bottom against the main money amount. Each square keeps the planned money amount chosen by the user and shows a thin bottom coverage bar so the user can see whether that square is fully covered, partly covered, or not covered. The bar is grey by default and fills green from left to right as coverage increases. If a square is not fully covered, its `{money amount} needed` note appears at the top-left of the bottom coverage bar.

The visible order is the coverage order. If the user moves a `Saving` square to the top, that square is checked first, so its coverage bar fills from the main money amount before lower squares use what remains. When the user finishes moving a square and lets go, the website saves the new order and recalculates the `Savings` display from that order.

The planned money amount in a `Saving` square does not mean money has moved into a separate place. It does not lower the main money amount.

After a `Saving` square is created, changing its planned money amount replaces the old planned money amount with a new total planned money amount. It is not an add or subtract action and does not change the main money amount.

If the user tries to change an existing `Saving` square's planned money amount with an empty planned money amount, the website should do nothing: no message, no saved data change, and the change flow stays open until the user enters a planned money amount or cancels.

A `Saving` square should not be created or saved with a `0$` planned money amount. If the user tries to save a new `Saving` square with a planned money amount of `0$`, nothing should happen: no message, no new square, no saved data change, no `Balance Changes` entry, and the same `Saving` square create input step stays open until the user enters a planned money amount greater than `0$` or cancels. If an existing `Saving` square's planned money amount becomes `0$`, the square should disappear from `Savings`.

When a new `Saving` square create attempt has more than one invalid value, the website should check the planned money amount first, the missing `Saving` name second, and the duplicate `Saving` name third. If the planned money amount is missing or `0$`, the same `Saving` square create input step should stay open with no message, no saved data change, no new square, and no `Balance Changes` entry, even if the entered name is also duplicate.

Deleting a `Saving` square removes only that square and its saved details. It does not change the main money amount, `Balance Changes`, or the other `Saving` squares. The money amount shown inside `Savings` and coverage bars update from the remaining squares because they are calculated display values.

Deleting a `Saving` square should ask for confirmation with the exact message `Delete this Saving?` and buttons exactly named `Cancel` and `Delete`.

There is no separate `View history` action. Money amount change history stays under the main money amount, and the user can scroll down if there are too many entries to fit on the screen.

When no saved money amount exists yet, the website starts with the money amount at `0$` and shows the dashboard. This default `0$` amount does not create a `Balance Changes` entry.

Opening the website with the default `0$` money amount should not create saved browser data by itself. The website should start saving after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The user clicks the money amount and uses `Add` to add money. After the money amount is greater than `0$`, users can update the amount with `Add`, `Subtract`, and `Modify`.

Money inputs may use a decimal point for cents, such as `14.56`. Money amount inputs should request a decimal numeric keyboard on devices that support it, so the user gets number keys `0` through `9` and a decimal point. Money amount inputs should ignore letters while the user types, and the first accepted character must be a number from `0` through `9`. Blocked typed characters should not appear in the field, and no message should appear for those blocked typed characters. The user should not be able to paste into money amount inputs or `Saving` square planned money amount inputs. If the user tries to paste letters, numbers, symbols, or any other content into those inputs, the pasted content should not appear, the input should keep its previous value, and no message should appear. The website should save and show that money amount as `14.56$`, not as integer cents like `1456`. The zero money amount should show as `0$`, nonzero whole money amounts should show with one digit after the decimal point such as `14.0$`, and nonzero money amounts with cents should show with two digits after the decimal point such as `14.50$`.

Normal input flows should show two text actions at the bottom: `Save` and `Cancel`. This applies to `Add`, `Subtract`, `Modify`, new `Saving` square creation, `Saving` square rename, and `Saving` square planned-money-amount change. `Save` should try to save the entered values using the rules for that action. `Cancel` should close the input flow, return to the view the user was already using, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

If the user tries to save an empty money amount in `Add`, `Subtract`, or `Modify`, nothing should happen. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a valid money amount or cancels.

If the user tries to save `0$` in `Add` or `Subtract`, nothing should happen. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a money amount greater than `0$` or cancels.

`Add` and `Subtract` actions should appear as separate visible history entries for 30 days. They should not be combined into only one net result.

Each visible `Balance Changes` row should show only `+{money amount} added` or `-{money amount} subtracted`. It should not show the previous money amount, new money amount, date, or time.

The newest `Balance Changes` entry should appear first.

Deleting a `Balance Changes` entry should ask for confirmation with the exact message `Delete this Balance Change?` and buttons exactly named `Cancel` and `Delete`.

`Add`, `Subtract`, and `Balance Changes` entries should not have notes.

For visible `Balance Changes` history, one month means 30 days, not a calendar month. The user should not choose dates for `Add` or `Subtract` entries. The website should use dates only internally to delete visible `Balance Changes` entries from browser storage after 30 days. Each visible-until date should be calculated from the internal created date and time plus 30 days.

The website should clean old `Balance Changes` entries when it opens and loads saved data, and after every successful saved user action. During cleanup, entries at or after their visible-until date and time should be deleted from browser storage and removed from the visible history list. The first version does not need a background timer while the website stays open with no user action.

`Modify` silently corrects the current money amount after the money amount is greater than `0$`. It should not create history or notification entries.

The money amount should never become negative. If the user subtracts more than the current money amount, the website should set the money amount to `0$`.

The first version saves the user's money amount and money changes without requiring an email account, login, server database, or online sync.

Saved browser data should load only when the data version value is exactly `1`. If saved browser data has a missing, wrong, future, unreadable, or unrecognized data version, the website should treat it as broken saved data, show `Saved data could not be loaded.`, and show a `Start again` action. When the user chooses `Start again`, the website should delete the broken saved data and immediately create fresh browser storage data with the money amount set to `0$`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`.

The website should not tell the user where saved information is stored, and should not warn that saved information may disappear after changing browser, changing device, using private browsing, or clearing browser data.

The website must clearly communicate that it is a personal tracking tool, not a real bank account.

## Why?

Cash is easy to spend because it is not automatically tracked. Users may forget how much money they received, how much they spent, and what cash they meant to keep aside.

The website helps users:

- See their money amount with a familiar bank-style `Current Balance` label.
- Manually modify their money amount after the money amount is greater than `0$`.
- Add money and subtract money while keeping each change visible on its own.
- Correct mistakes with `Modify` after the money amount is greater than `0$`, without saving or showing a correction entry.
- Correct their money amount with `Modify` after the money amount is greater than `0$`.
- Plan savings with user-named `Saving` squares that show covered and needed money without lowering the main money amount.
- Understand where their cash is going.

The goal is to make cash feel organized, visible, and easier to control.

## Where?

The website is used in a browser on desktop and mobile.

Users should be able to open it quickly when they want to check or modify their money amount, especially from a phone.

Users may update their money rarely or many times in one day. The website should not require a specific update schedule.

If the user closes the website and opens it again later, their saved money amount and 30-day visible history should still appear when the saved data is available.
