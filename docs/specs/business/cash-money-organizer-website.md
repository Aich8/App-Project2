# Business Spec: Cash Money Organizer Website

## Business Goals

- Help users organize cash without needing a real bank account.
- Help users plan cash visually in `Savings` without creating a separate target-tracking system.
- Reduce forgotten spending.
- Make cash tracking simple enough for daily use when the user wants it, without requiring daily updates.
- Build trust with a clean, bank-like interface.

## Core Features

### Current Money Amount

The user can view their total money amount with the user-facing label `Current Balance`.

A new user should be able to set their first money amount before using the dashboard. After setup, the user should be able to keep that money amount accurate with simple manual actions.

The first money amount should be saved in history as `+{money amount} added`.

This first history entry should follow the same visible history rules as a normal `Add` entry. It should stay visible in `Balance Changes` for 30 days, and the user can delete it from the visible history without changing the main money amount.

The website should make the main money actions easy to understand without using real banking language.

### Storage Scope

The first version should save the user's data only in browser storage.

Browser storage is an internal implementation detail. The website should not tell the user where saved information is stored.

The website should not tell the user that saved information belongs to the same browser or device.

The website should not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

### Add and Subtract Money Amount

The user can quickly add money to the money amount or subtract money from the money amount when their real cash changes.

This is for normal cash movements, such as receiving cash, spending cash, or removing cash from the tracked amount. It is not the same as correcting the full money amount after the displayed amount is wrong.

The website should support users who update their money amount rarely and users who update it many times in one day.

The money amount should never become negative.

If the user subtracts more than the current money amount, the website should set the money amount to `$0`.

When a subtract action is larger than the current money amount, the `Balance Changes` entry should show only the actual amount removed from the money amount.

If the money amount is already `$0`, subtracting should keep the money amount at `$0` and should not create a `Balance Changes` entry.

### Money Change History

The website should help users review recent money changes using the user-facing label `Balance Changes`.

`Balance Changes` helps the user understand how the money amount changed over time.

`Add` and `Subtract` entries should stay separate in `Balance Changes` and remain visible for 30 days.

Old visible history entries can be hidden or removed after 30 days without changing the saved money amount.

For `Balance Changes`, one month means 30 days, not a calendar month.

The user should not choose a date for an `Add` or `Subtract` entry.

Dates should matter only for clearing old visible `Balance Changes` entries after 30 days. When a `Balance Changes` entry is created, the website should save the created date and time internally and calculate its visible-until date and time as 30 days later. At or after the visible-until date and time, the entry may be hidden or removed from the visible history list.

The user can delete a `Balance Changes` entry when they make a mistake.

Deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it from the visible history. If the main money amount is wrong, the user should use `Modify` to correct the main money amount.

`Balance Changes` entries should not have notes. `Add` and `Subtract` should ask only for the money amount to add or subtract.

### Silent Modify Correction

The user can manually correct the total money amount whenever they want by using `Modify`.

This is for setting the displayed total to the correct real cash amount when the website's current money amount no longer matches reality. The correction should not be confused with normal added or subtracted money.

### Savings

`Savings` is a separate planning section of the website.

A `Saving` is one user-named square inside the `Savings` section.

A `Saving` stores a planned money amount chosen by the user.

A `Saving` square name is required and must be unique inside `Savings`.

Two `Saving` squares cannot use the same name. Names that match after trimming spaces and ignoring uppercase or lowercase letters should count as the same name.

If the user tries to create or rename a `Saving` square with a duplicate name, the website should not save that duplicate name.

A `Saving` square should exist only when its planned money amount is greater than `$0`.

The user can use `Saving` squares to see what planned money is fully covered, partly covered, or not covered by the main money amount.

The money amount shown inside `Savings` should start from the main money amount and subtract the planned money amounts in `Saving` squares in their visible order. It should never go below `$0`.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`.

The `Savings money amount` label is only for the money amount shown inside `Savings`. The main money amount should still use the exact user-facing label `Current Balance`.

Each `Saving` square keeps the planned money amount chosen by the user. If the main money amount changes, the `Saving` square planned money amounts should not change automatically.

After a `Saving` square is created, the user can change that square's planned money amount by entering a new total planned money amount.

Changing a `Saving` square's planned money amount should replace the old planned money amount. It should not add to or subtract from the old planned money amount.

Changing a `Saving` square's planned money amount does not change the main money amount and should not create a `Balance Changes` entry.

If a `Saving` square's planned money amount becomes `$0`, the `Saving` square should disappear from `Savings` and should not stay saved as a `$0` square.

Removing a `Saving` square because its planned money amount became `$0` should not change the main money amount and should not create a `Balance Changes` entry.

`Saving` squares should have an order. The first `Saving` square the user creates should stay at the top by default. The user should be able to delete `Saving` squares and change their order by holding and moving them.

The visible order is the coverage order. The top `Saving` square is checked first and gets first priority for coverage. If the user moves a `Saving` square to the top, that square is checked before the squares below it.

When the user finishes moving a `Saving` square and lets go, the website should save the new order in browser storage.

After the move finishes, the money amount shown inside `Savings` and the coverage bars should recalculate from the new visible order.

Reordering `Saving` squares should not change the main money amount and should not create a `Balance Changes` entry.

When the user deletes a `Saving` square:

- Only that `Saving` square and the details inside it should be removed.
- The main money amount should not change.
- `Balance Changes` should not get a new entry.
- Other `Saving` squares should keep their names, planned money amounts, and relative order.
- The money amount shown inside `Savings` and the coverage bars should update from the remaining `Saving` squares because they are calculated display values.

The website should check `Saving` squares from top to bottom using the main money amount:

- If enough money is available for a `Saving` square, that square is fully covered.
- If only part of the money is available for a `Saving` square, that square is partly covered.
- If no money is available for a `Saving` square, that square is not covered.

Each `Saving` square should show a thin horizontal coverage bar at the very bottom of the square. The bar should be grey by default and fill green from left to right based on how much of that square's planned money amount is covered.

If a `Saving` square is 80% covered, the left 80% of its coverage bar should be green and the right 20% should stay grey.

If a `Saving` square is not fully covered, the square should show a small `{money amount} needed` note at the top-left of the bottom coverage bar.

The user should add a new `Saving` square using a circle action with a `+` sign.

Before a new `Saving` square is created, the user should enter the `Saving` name and a planned money amount greater than `$0` for that square.

If the user does not enter both a valid name and a planned money amount greater than `$0`, the `Saving` square should not be created.

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

- Users can add, subtract, or manually correct their money amount whenever they need to.
- Users can make many changes in one day without hitting a usage limit.
- Users can return after a long time and still understand or update their money amount.
- Users can review `Balance Changes` to understand how their money amount changed.
- Users can use `Savings` to plan money visually without lowering the main money amount.
- Users can understand that `Savings` is a planning section, not a real account or separate goals feature.
- Users can understand that the website is a manual cash tracker, not a real bank.

## Remaining Grey Zones

Grey zone: The active plan and technical spec still say saved data may include `Basic website settings if needed`, but the first version does not define any settings. Decide whether to remove that line completely or define the exact settings that should exist.
