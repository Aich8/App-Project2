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

A new user should see the money amount as `0$` the first time they open the website.

The website should not ask for a starting amount before showing the dashboard.

The website should not create saved browser data only because it shows the default `0$` money amount.

The default `0$` money amount should not create a `Balance Changes` entry.

When the money amount is `0$`, clicking the main money amount should show only the `Add` action.

After the user adds money and the money amount is greater than `0$`, clicking the main money amount should show `Add`, `Subtract`, and `Modify`.

The website should make the main money actions easy to understand without using real banking language.

### Storage Scope

The first version should save the user's data only in browser storage.

The implementation should use the stable storage key `cash-money-organizer-website-data` and the first data version value `1`.

When no saved data exists yet, the website should show the default `0$` money amount without saving it just because the website opened. The website should start saving data after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

Browser storage is an internal implementation detail. The website should not tell the user where saved information is stored.

The website should not tell the user that saved information belongs to the same browser or device.

The website should not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

### Add and Subtract Money Amount

The user can quickly add money to the money amount or subtract money from the money amount when their real cash changes.

This is for normal cash movements, such as receiving cash, spending cash, or removing cash from the tracked amount. It is not the same as correcting the full money amount after the displayed amount is wrong.

The website should support users who update their money amount rarely and users who update it many times in one day.

The money amount should never become negative.

The `Add` action should be available when the money amount is `0$` or more.

The `Subtract` action should be available only when the money amount is greater than `0$`.

`Add` and `Subtract` should accept only money amounts greater than `0$`.

Negative money amounts should be blocked for all money actions.

Money inputs should allow cents, with up to two digits after the decimal point.

The user should enter only the number. The user may type a decimal point, such as `14.56`.

Money amount inputs should request a decimal numeric keyboard on devices that support it, so the user gets number keys `0` through `9` and a decimal point.

Money amount inputs should filter typing while the user types. The first accepted character must be a number from `0` through `9`. If the field is empty and the user types a letter or decimal point, the field should not change. After a number is entered, the input should accept only numbers and one decimal point, while still blocking more than two digits after the decimal point.

The website should save and show that money amount with the `$` sign at the end, such as `14.56$`.

The zero money amount should continue to show as `0$`.

Nonzero whole money amounts should be saved and shown with one digit after the decimal point. For example, `14` should be saved and shown as `14.0$`.

Nonzero money amounts with cents should be saved and shown with two digits after the decimal point. For example, `14.50` should be saved and shown as `14.50$`.

Money amounts should be saved as decimal money amounts with the `$` sign at the end, not as integer cents. For example, save `14.56$`, not `1456`.

Inputs with more than two digits after the decimal point should be blocked.

If the user subtracts more than the current money amount, the website should set the money amount to `0$`.

When a subtract action is larger than the current money amount, the `Balance Changes` entry should show only the actual amount removed from the money amount.

If the money amount is already `0$`, subtracting should keep the money amount at `0$` and should not create a `Balance Changes` entry.

### Money Change History

The website should help users review recent money changes using the user-facing label `Balance Changes`.

`Balance Changes` helps the user understand how the money amount changed over time.

`Add` and `Subtract` entries should stay separate in `Balance Changes` and remain visible for 30 days.

The newest `Balance Changes` entry should appear first.

Old visible history entries should be deleted from browser storage after 30 days without changing the saved money amount.

For `Balance Changes`, one month means 30 days, not a calendar month.

The user should not choose a date for an `Add` or `Subtract` entry.

Dates should matter only for clearing old visible `Balance Changes` entries after 30 days. When a `Balance Changes` entry is created, the website should save the created date and time internally and calculate its visible-until date and time as 30 days later. At or after the visible-until date and time, the entry should be deleted from browser storage and removed from the visible history list.

The user can delete a `Balance Changes` entry when they make a mistake.

Before deleting a `Balance Changes` entry, the website should ask the user to confirm the delete action.

The confirmation message should be exactly `Delete this Balance Change?`.

The confirmation buttons should be exactly `Cancel` and `Delete`.

Deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

After a `Balance Changes` entry is deleted, the website should not offer undo.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it from the visible history. If the main money amount is wrong, the user should use `Modify` to correct the main money amount.

`Balance Changes` entries should not have notes. `Add` and `Subtract` should ask only for the money amount to add or subtract.

### Silent Modify Correction

The user can manually correct the total money amount by using `Modify` after the money amount is greater than `0$`.

This is for setting the displayed total to the correct real cash amount when the website's current money amount no longer matches reality. The correction should not be confused with normal added or subtracted money.

`Modify` should allow `0$` or more, but should not allow a negative money amount.

### Savings

`Savings` is a separate planning section of the website.

A `Saving` is one user-named square inside the `Savings` section.

A `Saving` stores a planned money amount chosen by the user.

Planned money amounts in `Saving` squares should use the same money amount display format: `0$` for zero, one decimal digit for nonzero whole money amounts such as `14.0$`, and two decimal digits for nonzero money amounts with cents such as `14.50$`.

If the user tries to save a new `Saving` square without a planned money amount, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a planned money amount or cancels creating that square.

A `Saving` square name is required and must be unique inside `Savings`.

If the user tries to save a new `Saving` square without a `Saving` name, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a `Saving` name or cancels creating that square.

Two `Saving` squares cannot use the same name. Names that match after trimming spaces and ignoring uppercase or lowercase letters should count as the same name.

If the user tries to create or rename a `Saving` square with a duplicate name, the website should not save that duplicate name.

If the user enters a duplicate `Saving` name while creating or renaming a `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message. Nothing should change: no new `Saving` square should be created, an existing `Saving` square should keep its old name, saved data should stay unchanged, and `Balance Changes` should not get a new entry.

A `Saving` square should exist only when its planned money amount is greater than `0$`.

The user can use `Saving` squares to see what planned money is fully covered, partly covered, or not covered by the main money amount.

The money amount shown inside `Savings` should start from the main money amount and subtract the planned money amounts in `Saving` squares in their visible order. It should never go below `0$`.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`.

The `Savings money amount` label is only for the money amount shown inside `Savings`. The main money amount should still use the exact user-facing label `Current Balance`.

Each `Saving` square keeps the planned money amount chosen by the user. If the main money amount changes, the `Saving` square planned money amounts should not change automatically.

After a `Saving` square is created, the user can change that square's planned money amount by entering a new total planned money amount.

If the user tries to change an existing `Saving` square's planned money amount with an empty planned money amount, the website should do nothing. No message should appear, the old planned money amount should stay saved, saved data should stay unchanged, and the planned-money-amount change flow should stay open until the user enters a planned money amount or cancels changing that square.

If the user tries to rename an existing `Saving` square with an empty name, the website should do nothing. No message should appear, the old name should stay saved, saved data should stay unchanged, and the rename flow should stay open until the user enters a `Saving` name or cancels renaming that square.

Changing a `Saving` square's planned money amount should replace the old planned money amount. It should not add to or subtract from the old planned money amount.

Changing a `Saving` square's planned money amount does not change the main money amount and should not create a `Balance Changes` entry.

If a `Saving` square's planned money amount becomes `0$`, the `Saving` square should disappear from `Savings` and should not stay saved as a `0$` square.

Removing a `Saving` square because its planned money amount became `0$` should not change the main money amount and should not create a `Balance Changes` entry.

`Saving` squares should have an order. The first `Saving` square the user creates should stay at the top by default. The user should be able to delete `Saving` squares and change their order by holding and moving them.

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

Each `Saving` square should show a thin horizontal coverage bar at the very bottom of the square. The bar should be grey by default and fill green from left to right based on how much of that square's planned money amount is covered.

If a `Saving` square is 80% covered, the left 80% of its coverage bar should be green and the right 20% should stay grey.

If a `Saving` square is not fully covered, the square should show a small `{money amount} needed` note at the top-left of the bottom coverage bar.

The user should add a new `Saving` square using a circle action with a `+` sign.

Before a new `Saving` square is created, the user should enter the `Saving` name and a planned money amount greater than `0$` for that square.

If the user tries to save without a planned money amount, the website should do nothing until the user enters a planned money amount or cancels.

If the user does not enter both a valid name and a planned money amount greater than `0$`, the `Saving` square should not be created.

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

- Users can add money whenever they need to, and can use `Modify` or `Subtract` after the money amount is greater than `0$`.
- Users can make many changes in one day without hitting a usage limit.
- Users can return after a long time and still understand or update their money amount.
- Users can review `Balance Changes` to understand how their money amount changed.
- Users can use `Savings` to plan money visually without lowering the main money amount.
- Users can understand that `Savings` is a planning section, not a real account or separate goals feature.
- Users can understand that the website is a manual cash tracker, not a real bank.

## Remaining Grey Zones And Mistakes

- Mistake: The active plan still says invalid amounts are blocked with useful feedback. This does not match the newer decisions where some invalid actions show no message and letters simply do not change money amount inputs.
- Grey zone: The exact result for trying to save an empty money amount in `Add`, `Subtract`, or `Modify` is not fully defined. The specs say invalid money amounts are blocked, but they do not say whether the flow stays open, closes, or shows no message.
- Grey zone: The exact result for entering `0$` in `Add` or `Subtract` is not fully defined. The specs say `Add` and `Subtract` need a money amount greater than `0$`, but they do not say what the website should do when the user tries to save `0$`.
- Grey zone: Creating a new `Saving` square with a planned money amount of `0$` is blocked, but the exact behavior is not fully defined. The specs do not say whether the create flow stays open with no message, closes, or returns to the `Saving` squares view.
- Grey zone: The visible `Balance Changes` row format is not fully defined. The specs define examples like `+56.0$ added` and `-34.0$ subtracted`, but they do not say whether each row should also show previous money amount, new money amount, date, or time.
- Grey zone: Old `Balance Changes` entries must be deleted at or after their visible-until date and time, but the exact cleanup trigger is not defined. The specs do not say whether cleanup runs only when the website opens, after every saved action, while the website stays open, or all of those.
- Grey zone: Saved data with a missing, wrong, or future data version is not fully defined. The specs now define storage key `cash-money-organizer-website-data` and data version `1`, but they do not say whether another version should be treated as broken saved data or handled another way.
- Mistake: The global spec says the website helps users "understand where their cash is going," but the first version does not have notes or categories, so it can show money amount changes but not exactly where money went.
