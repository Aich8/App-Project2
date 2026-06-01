# Business Spec: Cash Money Organizer Website

## For who?

This website is for people who keep or manage money in cash and have difficulty organizing it, tracking it, and saving it.

Primary users include:

- People who want to manually track their cash amount.
- Students or young adults managing pocket money.
- Small informal sellers or service providers.
- People who do not use a bank account regularly.
- People who want a simple, visual way to update and understand how much cash they have.

## Why?

Cash is easy to spend because it is not automatically tracked. Users may forget how much money they received, how much they spent, and how much they wanted to save.

The website helps users:

- See their cash like a bank account balance.
- Manually modify their money amount.
- Correct their balance whenever they want.
- Separate money for saving goals.
- Understand where their cash is going.

The goal is to make cash feel organized, visible, and easier to control.

## What?

The product is a personal cash management website that looks and feels similar to an online bank account, but it does not hold real money and does not connect to banks.

Users manually enter and update their own cash amount. The website displays a bank-style dashboard with:

- Current cash balance.
- Recent cash activity.
- Manual balance changes.
- Saving goals.
- Cash categories or envelopes.
- Monthly overview.

The website must clearly communicate that it is a personal tracking tool, not a real bank account.

## Where?

The website is used in a browser on desktop and mobile.

Users should be able to open it quickly when they want to check or modify their money amount, especially from a phone.

## Business Goals

- Help users organize cash without needing a real bank account.
- Encourage saving by making goals visible.
- Reduce forgotten spending.
- Make cash tracking simple enough for daily use.
- Build trust with a clean, bank-like interface.

## Core Features

### Cash Balance

The user can view their total available cash balance.

The balance is calculated from:

- Starting amount.
- Money added with the plus action.
- Money subtracted with the minus action.
- Manual balance corrections.

### Quick Add and Subtract Controls

Under the main cash balance, the website should show two small icon actions:

- `+` for adding money to the main amount.
- `-` for subtracting money from the main amount.

These controls are manual balance actions. They do not represent a real bank deposit, bank withdrawal, or payment.

When the user taps the `+` icon:

- The website asks for the amount to add.
- The entered amount is added to the main cash balance.
- The action is saved in the balance history as a positive adjustment.

When the user taps the `-` icon:

- The website asks for the amount to subtract.
- The entered amount is subtracted from the main cash balance.
- The action is saved in the balance history as a negative adjustment.

Each add or subtract action should include:

- Amount.
- Date.
- Action type: added or subtracted.
- Optional note.

The `+` and `-` icons should appear visually connected to the main amount so the user understands they directly change the displayed balance.

### Modify Money Amount

The user can manually change the amount of money shown in the website.

Each modification should include:

- New total amount.
- Date.
- Optional note.

### Balance Adjustment History

The website should save each manual balance change so the user can understand how the amount changed over time.

Each entry should include:

- Previous amount.
- New amount.
- Difference.
- Date.
- Optional note.

### Modify Balance

The user can manually correct the total balance whenever they want.

The website should save this as an adjustment entry so the history stays clear.

### Saving Goals

The user can create saving goals, such as:

- Phone.
- Rent.
- Emergency money.
- Trip.
- School supplies.

Each saving goal should include:

- Goal name.
- Target amount.
- Current saved amount.
- Progress indicator.

### Cash Envelopes

The user can divide cash into virtual envelopes, such as:

- Spending.
- Savings.
- Bills.
- Emergency.

This helps users decide how much cash is safe to spend.

### Activity History

The user can see a list of all cash movements:

- Added money.
- Spent money.
- Savings transfers.
- Balance corrections.

The user should be able to edit or delete entries when they make a mistake.

## User Experience Requirements

- The first screen should look like a simple bank dashboard.
- The current cash balance should be the most visible information.
- Small `+` and `-` icons should appear under the main balance for quick manual changes.
- Main actions should be easy to find: modify amount, save money, manage envelopes, view history.
- The website should feel trustworthy, calm, and practical.
- The interface should work well on mobile.
- The user should not need financial knowledge to use it.

## Important Trust Requirement

The website may use a bank-account style interface, but it must not pretend to be a real bank.

It should avoid misleading wording such as:

- Deposit to bank.
- Withdraw from bank.
- Real account number.
- Card balance.
- Bank transfer.

Better wording:

- Add to balance.
- Subtract from balance.
- Modify amount.
- Cash balance.
- Saved cash.
- Manual correction.

## User Flow

1. User opens the website.
2. User sees their current cash balance.
3. User taps the `+` icon to add money or the `-` icon to subtract money.
4. User enters the amount to add or subtract.
5. User optionally adds a note.
6. User checks saving goals and cash envelopes.
7. User reviews history to understand balance changes.

## Success Metrics

- Users modify their money amount regularly.
- Users create at least one saving goal.
- Users reduce unexplained cash loss.
- Users return to the website to check or update their balance.

## Out of Scope for First Version

- Real bank connections.
- Payments or money transfers.
- Credit cards or debit cards.
- Real account numbers.
- Loans.
- Investment features.
- Multi-user business accounting.

## Future Ideas

- Login and cloud sync.
- PIN or biometric lock.
- Charts for monthly spending.
- Reminders to count cash.
- Export to PDF or CSV.
- Multiple currencies.
- Budget warnings when spending too fast.
