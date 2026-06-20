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

The website should make the main money actions easy to understand without using real banking language.

### Add and Subtract Money Amount

The user can quickly add money to the money amount or subtract money from the money amount when their real cash changes.

This is for normal cash movements, such as receiving cash, spending cash, or removing cash from the tracked amount. It is not the same as correcting the full money amount after the displayed amount is wrong.

The website should support users who update their money amount rarely and users who update it many times in one day.

### Money Change History

The website should help users review recent money changes using the user-facing label `Balance Changes`.

`Balance Changes` helps the user understand how the money amount changed over time.

`Add` and `Subtract` entries should stay separate in `Balance Changes` and remain visible for one month.

Old visible history entries can be hidden or removed after one month without changing the saved money amount.

The user can delete a `Balance Changes` entry when they make a mistake.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it and record the correct `Add` or `Subtract` action.

### Silent Modify Correction

The user can manually correct the total money amount whenever they want by using `Modify`.

This is for setting the displayed total to the correct real cash amount when the website's current money amount no longer matches reality. The correction should not be confused with normal added or subtracted money.

### Savings

`Savings` is a separate planning section of the website.

A `Saving` is one user-named square inside the `Savings` section.

A `Saving` can hold a money amount.

The user can use `Saving` squares to see what money would be left in the `Savings` section.

The money amount shown inside `Savings` should equal the main money amount minus the total money set aside in `Saving` squares.

Putting money into a `Saving` square does not subtract money and does not lower the main money amount. It only lowers the money amount shown inside the `Savings` section.

The user should not be allowed to put more money into `Saving` squares than the available money shown inside `Savings`.

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
- Users can use `Savings` to plan money set aside without lowering the money amount.
- Users can understand that `Savings` is a planning section, not a real account or separate goals feature.
- Users can understand that the website is a manual cash tracker, not a real bank.