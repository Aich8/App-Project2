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
- `Savings` section with ordered user-named `Saving` squares and coverage bars.

The main money amount is clickable. When the user clicks it, the website shows `Modify`, `Add`, and `Subtract` actions.

The user opens `Savings` by clicking `Savings`. There should not be a separate `Open savings` action.

`Savings` is the separate planning section of the website. A `Saving` is one user-named square inside the `Savings` section.

The user creates a `Saving` square by clicking the `+` action in `Savings`, then entering the square name and planned money amount before the square appears.

The `Savings` section checks `Saving` squares from top to bottom against the main money amount. Each square keeps the planned money amount chosen by the user and shows a thin bottom coverage bar so the user can see whether that square is fully covered, partly covered, or not covered. The bar is grey by default and fills green from left to right as coverage increases. If a square is not fully covered, its `{money amount} needed` note appears at the top-left of the bottom coverage bar.

The planned money amount in a `Saving` square does not mean money has moved into a separate place. It does not lower the main money amount.

After a `Saving` square is created, changing its planned money amount replaces the old planned money amount with a new total planned money amount. It is not an add or subtract action and does not change the main money amount.

A `Saving` square should not be created or saved with a `$0` planned money amount. If an existing `Saving` square's planned money amount becomes `$0`, the square should disappear from `Savings`.

Deleting a `Saving` square removes only that square and its saved details. It does not change the main money amount, `Balance Changes`, or the other `Saving` squares. The money amount shown inside `Savings` and coverage bars update from the remaining squares because they are calculated display values.

There is no separate `View history` action. Money amount change history stays under the main money amount, and the user can scroll down if there are too many entries to fit on the screen.

The first ever money amount is saved as the starting amount and shown in history as `+{money amount} added`. After that, users update the amount with `Modify`, `Add`, and `Subtract`.

The first money amount history entry follows the same visible history rules as later `Add` entries. It stays visible in `Balance Changes` for 30 days and can be deleted from visible history without changing the main money amount.

`Add` and `Subtract` actions should appear as separate visible history entries for 30 days. They should not be combined into only one net result.

For visible `Balance Changes` history, one month means 30 days, not a calendar month. Each visible-until date should be calculated from the entry date and time plus 30 days.

`Modify` silently corrects the current money amount. It should not create history or notification entries.

The money amount should never become negative. If the user subtracts more than the current money amount, the website should set the money amount to `$0`.

The first version saves the user's money amount and money changes in browser storage on the same device and browser. It does not require an email account, login, server database, or online sync.

The website must clearly communicate that it is a personal tracking tool, not a real bank account.

## Why?

Cash is easy to spend because it is not automatically tracked. Users may forget how much money they received, how much they spent, and what cash they meant to keep aside.

The website helps users:

- See their money amount with a familiar bank-style `Current Balance` label.
- Manually modify their money amount.
- Add money and subtract money while keeping each change visible on its own.
- Correct mistakes with `Modify` without saving or showing a correction entry.
- Correct their money amount whenever they want.
- Plan savings with user-named `Saving` squares that show covered and needed money without lowering the main money amount.
- Understand where their cash is going.

The goal is to make cash feel organized, visible, and easier to control.

## Where?

The website is used in a browser on desktop and mobile.

Users should be able to open it quickly when they want to check or modify their money amount, especially from a phone.

Users may update their money rarely or many times in one day. The website should not require a specific update schedule.

If the user closes the website and opens it again later in the same browser, their saved money amount and 30-day visible history should still appear.

If the user changes browser, changes device, uses private browsing, or clears browser data, the saved information may not be available.
