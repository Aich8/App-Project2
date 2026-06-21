# Business Spec: Cash Money Organizer Website

## Business Goals

- Help users organize cash without needing a real bank account.
- Help users plan cash set aside in `Savings` without creating a separate target-tracking system.
- Reduce forgotten spending.
- Make cash tracking simple enough for daily use when the user wants it, without requiring daily updates.
- Build trust with a clean, bank-like interface.

## Core Features

### Current Money Amount

The user can view their total money amount with the user-facing label `Current Balance`.

A new user should be able to set their first money amount before using the dashboard. After setup, the user should be able to keep that money amount accurate with simple manual actions.

The first money amount should be saved in history as `+{money amount} added`.

This first history entry should follow the same visible history rules as a normal `Add` entry. It should stay visible in `Balance Changes` for one month, and the user can delete it from the visible history without changing the main money amount.

The website should make the main money actions easy to understand without using real banking language.

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

`Add` and `Subtract` entries should stay separate in `Balance Changes` and remain visible for one month.

Old visible history entries can be hidden or removed after one month without changing the saved money amount.

The user can delete a `Balance Changes` entry when they make a mistake.

Deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it from the visible history. If the main money amount is wrong, the user should use `Modify` to correct the main money amount.

### Silent Modify Correction

The user can manually correct the total money amount whenever they want by using `Modify`.

This is for setting the displayed total to the correct real cash amount when the website's current money amount no longer matches reality. The correction should not be confused with normal added or subtracted money.

### Savings

`Savings` is a separate planning section of the website.

A `Saving` is one user-named square inside the `Savings` section.

A `Saving` can hold a money amount.

The user can use `Saving` squares to see what planned money is fully covered, partly covered, or not covered by the main money amount.

The money amount shown inside `Savings` should start from the main money amount and subtract `Saving` squares in their visible order. It should never go below `$0`.

Each `Saving` square keeps the money amount chosen by the user. If the main money amount changes, the `Saving` square amounts should not change automatically.

`Saving` squares should have an order. The first `Saving` square the user creates should stay at the top by default. The user should be able to delete `Saving` squares and change their order by holding and moving them.

The website should check `Saving` squares from top to bottom using the main money amount:

- If enough money is available for a `Saving` square, that square is fully covered.
- If only part of the money is available for a `Saving` square, that square is partly covered.
- If no money is available for a `Saving` square, that square is not covered.

Each `Saving` square should show a thin vertical coverage bar. The bar should be grey by default and fill green based on how much of that square's money amount is covered.

If a `Saving` square is 80% covered, its coverage bar should be 80% green and 20% grey.

If a `Saving` square is not fully covered, the square should show a small `{money amount} needed` note near the top-left of the grey part of the coverage bar.

The user should add a new `Saving` square using a circle action with a `+` sign.

Putting money into a `Saving` square does not subtract money and does not lower the main money amount. It only changes planning inside the `Savings` section.

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
- Saved cash.
- Manual cash tracker.

## Success Criteria

- Users can add, subtract, or manually correct their money amount whenever they need to.
- Users can make many changes in one day without hitting a usage limit.
- Users can return after a long time and still understand or update their money amount.
- Users can review `Balance Changes` to understand how their money amount changed.
- Users can use `Savings` to plan money set aside without lowering the main money amount.
- Users can understand that `Savings` is a planning section, not a real account or separate goals feature.
- Users can understand that the website is a manual cash tracker, not a real bank.

## Mistakes And Grey Zones To Correct

These notes are review items for this business spec. They should be corrected or removed after the final product decisions are made.

### Delete Behavior For `Balance Changes`

Resolved: `Balance Changes` is the user-facing label for visible history entries created by `Add` and `Subtract` actions.

Resolved rule: `Modify` does not create a `Balance Changes` entry.

Resolved rule: deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

Status: Corrected in the business spec, functional spec, technical spec, and active plan.

### First Money Amount History Entry

Resolved: The first money amount history entry follows the same visible history rules as a normal `Add` entry.

Resolved rule: The first money amount history entry stays visible in `Balance Changes` for one month and can be deleted from the visible history.

Resolved rule: Deleting the first money amount history entry does not change the main money amount.

Status: Corrected in the business spec, functional spec, global spec, technical spec, and active plan.

### Subtract Edge Cases

Resolved: The business spec now includes the subtract edge cases.

Resolved rules:

- Subtracting more than the current money amount should set the money amount to `$0`.
- The history entry should use the actual amount removed from the money amount.
- Subtracting when the money amount is already `$0` should not create a history entry.

Status: Corrected in the business spec.

### `Savings` When Money Amount Drops

Resolved: If the main money amount becomes lower than the total money amount in `Saving` squares, the `Saving` square amounts should not change automatically.

Resolved rule: The website should check `Saving` squares from top to bottom. Covered squares show green coverage bars, partly covered squares show partly green bars, and uncovered squares show grey bars.

Resolved rule: The money amount shown inside `Savings` should stop at `$0` instead of becoming negative.

Status: Corrected in the business spec, functional spec, global spec, technical spec, and active plan.

### `Savings` Success Wording

Resolved: The success criteria now say users can use `Savings` to plan money set aside without lowering the main money amount.

Status: Corrected in the business spec.

### Browser Storage Scope

Grey zone: The business spec says the MVP should not include online accounts, personal information collection, or real money capabilities, but it does not clearly say that saved data only belongs to the same browser and device.

Decision needed: Add a business-level rule explaining that the first version saves data only in browser storage, and that data may not appear if the user changes device, changes browser, uses private browsing, or clears browser data.

### One-Month History Meaning

Grey zone: The business spec says `Add` and `Subtract` entries remain visible for one month, but it does not define whether one month means 30 days or a calendar month.

Decision needed: Decide how the website should calculate the visible-until date for `Balance Changes` entries.
