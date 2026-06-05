# Global Spec: Cash Money Organizer Website

## For who?

This website is for people who keep or manage money in cash and have difficulty organizing it, tracking it, and saving it.

Primary users include:

- People who want to manually track their money amount.
- Students or young adults managing pocket money.
- Small informal sellers or service providers.
- People who do not use a bank account regularly.
- People who want a simple, visual way to update and understand how much cash they have.

## What?

The product is a personal cash management website that looks and feels similar to an online bank account, but it does not hold real money and does not connect to banks.

Users manually enter and update their own money amount. The website displays a bank-style dashboard with:

- Main money amount shown with the label `Current Balance`.
- Recent money activity.
- Separate add and subtract history entries.
- `Savings` section with user-named folders or squares.
- Cash categories or envelopes.
- Monthly overview.

The first ever money amount is saved as the starting amount and shown in history as `+{money amount} added`. After that, users update the amount with `Modify`, `Add`, and `Subtract`.

`Add` and `Subtract` actions should appear as separate visible history entries for one month. They should not be combined into only one net result.

`Modify` silently corrects the current money amount. It should not create history, activity, or notification entries.

The money amount should never become negative. If the user subtracts more than the current money amount, the website should set the money amount to `$0`.

The first version saves the user's money amount and money changes in browser storage on the same device and browser. It does not require an email account, login, server database, or online sync.

The website must clearly communicate that it is a personal tracking tool, not a real bank account.

## Why?

Cash is easy to spend because it is not automatically tracked. Users may forget how much money they received, how much they spent, and how much they wanted to save.

The website helps users:

- See their money amount with a familiar bank-style `Current Balance` label.
- Manually modify their money amount.
- Add money and subtract money while keeping each change visible on its own.
- Correct mistakes with `Modify` without saving or showing a correction entry.
- Correct their money amount whenever they want.
- Plan savings with user-named folders or squares that reduce only the savings available amount.
- Understand where their cash is going.

The goal is to make cash feel organized, visible, and easier to control.

## Where?

The website is used in a browser on desktop and mobile.

Users should be able to open it quickly when they want to check or modify their money amount, especially from a phone.

Users may update their money rarely or many times in one day. The website should not require a specific update schedule.

If the user closes the website and opens it again later in the same browser, their saved money amount and one-month visible history should still appear.

If the user changes browser, changes device, uses private browsing, or clears browser data, the saved information may not be available.
