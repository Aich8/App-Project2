# Business Spec: Cash Money Organizer Website

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
- Recorded cash movements.
- Manual balance corrections.

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
3. User modifies their cash amount when it changes.
4. User checks saving goals and cash envelopes.
5. User reviews history to understand balance changes.

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
