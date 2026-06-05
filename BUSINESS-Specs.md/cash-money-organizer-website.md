# Business Spec: Cash Money Organizer Website

## Business Goals

- Help users organize cash without needing a real bank account.
- Encourage saving by making planned money visible.
- Reduce forgotten spending.
- Make cash tracking simple enough for daily use.
- Build trust with a clean, bank-like interface.

## Core Features

These are the main features the website needs for the first version.

### Money Amount

The website tracks the user's money amount.

The main money amount should be shown in the website with the user-facing label `Current Balance`.

The `Current Balance` label gives the website a familiar bank-style feeling, but it must not make the website look like a real bank account.

The user should be able to set their first money amount and then update it with `Modify`, `Add`, and `Subtract`.

### Money Change History

The website should help the user understand recent visible money changes.

Added money and subtracted money should be easy to read and should not feel mixed together.

`Modify` should feel like a quiet correction, not like money was added or subtracted.

### Savings

The website should have a `Savings` section that helps the user plan money.

The `Savings` section should appear under the main money amount and recent add or subtract entries.

The `Savings` section should start with the same amount as the main money amount.

The `Savings` section should help the user see what money would be left if they put some money aside.

Inside `Savings`, the user can create small folders or squares with a name and a money amount.

Example folders or squares:

- Rent: `$40`.
- Phone number: `$10`.
- Trip: `$50`.
- Emergency: `$25`.

Folder or square amounts should subtract only from the savings available amount inside the `Savings` section.

Folder or square amounts should not change the main money amount shown as `Current Balance`.

Example:

- Main money amount is `$350`.
- `Savings` starts at `$350`.
- User creates `Rent: $40`.
- Savings available amount becomes `$310`.
- Main money amount still shows `$350`.

Savings folders or squares should help the user remember money that is planned for something, without making it look like the money was spent, removed, added, or subtracted.

### Cash Envelopes

The user can divide cash into virtual envelopes, such as:

- Spending.
- Savings.
- Bills.
- Emergency.

This helps users decide how much cash is safe to spend.

### Activity History

The user can review recent visible activity, such as:

- Added money.
- Subtracted money.
- Savings folder or square changes.

Activity history should help the user understand what changed recently.

### Browser Storage

The first version should work without an email account, login, online sync, or server database.

The website should save enough data in the user's browser so the user can close the website and come back later in the same browser.

The website should explain in simple words that browser storage is not the same as an online account or cloud backup.

## Important Trust Requirement

The website may use a bank-account style interface, but it must not pretend to be a real bank.

It should avoid misleading wording such as:

- Deposit to bank.
- Withdraw from bank.
- Real account number.
- Card-related money wording.
- Bank transfer.

Better wording:

- Add money.
- Subtract money.
- Modify amount.
- `Current Balance` for the main money amount.
- Saved cash.

## Open Questions

Cash envelope behavior still needs allocation rules.

The spec should still decide whether cash envelope amounts reduce an available amount, only label parts of the same money amount, or work like a separate planning view.

## Success Metrics

- Users can modify their money amount whenever they need, whether rarely or many times per day.
- The website should not require or expect a specific number of money updates.
- The current money amount stays correct after `Add`, `Subtract`, and `Modify`.
- Add and subtract history stays clear, separate, and easy to understand.
- Saved browser data appears again when the user closes and reopens the website in the same browser.

## Grey Zones And Possible Mistakes

- Cash envelope behavior is still unclear. The spec should decide whether envelopes reduce an available amount, only label parts of the same money amount, or work like a separate planning view.
- `Saved cash` may create naming confusion because the spec mostly uses `money amount`, `Savings`, and `Current Balance`.
- Some success metrics describe whether the app works correctly, not business success. Later, the spec may need business-focused success wording such as trust, clarity, or reduced forgotten spending.
