# Business Spec: Cash Money Organizer Website

## Business Goals

- Help users organize cash without needing a real bank account.
- Help users separate cash into clear categories without creating a separate target-tracking system.
- Reduce forgotten spending.
- Make cash tracking simple enough for daily use.
- Build trust with a clean, bank-like interface.

## Core Features

### Current Money Amount

The user can view their total money amount with the user-facing label `Current Balance`.

A new user should be able to set their first money amount before using the dashboard. After setup, the user should be able to keep that money amount accurate with simple manual actions.

The website should make the main money actions easy to understand without using real banking language.

### Add and Subtract Money Amount

The user can quickly add money to the money amount or subtract money from the money amount when their real cash changes.

This is for normal cash movements, such as receiving cash, spending cash, or removing cash from the tracked amount. It is not the same as correcting the full money amount after the displayed amount is wrong.

The website should support users who update their money amount rarely and users who update it many times in one day.

### Money Change History

The website should help users review recent money changes using the user-facing label `Balance Changes`.

`Balance Changes` helps the user understand how the money amount changed over time.

### Silent Modify Correction

The user can manually correct the total money amount whenever they want by using `Modify`.

This is for setting the displayed total to the correct real cash amount when the website's current money amount no longer matches reality. The correction should not be confused with normal added or subtracted money.

### Savings

`Savings` is a separate planning section of the website.

A `Saving` is one user-named square inside the `Savings` section.

A `Saving` can hold a money amount.

The user can use `Saving` squares to see what money would be left in the `Savings` section.

Putting money into a `Saving` square does not subtract money and does not lower the main money amount. It only lowers the money amount shown inside the `Savings` section.

`Savings` is not a separate real account and not a separate goals feature.

### Cash Categories / Envelopes

The user can divide their total money amount into simple virtual envelopes. An envelope is a labeled portion of the same real cash, not a separate bank account and not a target-tracking feature.

Example envelopes could include:

- Daily spending.
- Food.
- Transport.
- Bills.
- Emergency.

In the website, this should feel like looking at labeled pockets inside one wallet. The dashboard can still show one main money amount, while a category area helps the user understand how that cash is currently divided.

Cash categories or envelopes are separate from `Savings`. Envelopes help organize cash into everyday categories, while `Savings` helps plan money set aside in `Saving` squares.

### Recent Activity

The user can see recent cash organization activity. Money changes should be understood through `Balance Changes`.

## User Experience Requirements

- The first screen should look like a simple bank dashboard.
- The current money amount should be the most visible information.
- Main money actions, `Savings`, and envelope organization should be easy to find.
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
- Users can understand the difference between `Savings` and cash envelopes.
- Users can understand that the website is a manual cash tracker, not a real bank.
