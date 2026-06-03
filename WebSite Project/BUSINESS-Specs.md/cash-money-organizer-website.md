# Business Spec: Cash Money Organizer Website

## Business Goals

- Help users organize cash without needing a real bank account.
- Help users separate cash into clear categories without creating a separate target-tracking system.
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

### Balance Adjustment History

The website should save each manual balance change so the user can understand how the amount changed over time.

## Modify Balance

The user can manually correct the total balance whenever they want.

The website should save this as an adjustment entry so the history stays clear.

### Cash Categories / Envelopes

The user can divide their total cash balance into simple virtual envelopes. An envelope is a labeled portion of the same real cash, not a separate bank account and not a target-tracking feature.

Example envelopes could include:

- Daily spending.
- Food.
- Transport.
- Bills.
- Set aside cash.
- Emergency.

In the app, this could feel like looking at labeled pockets inside one wallet. The dashboard can still show one main cash balance, while a category area shows how that cash is currently divided.

Each envelope could appear as a compact row or small card with:

- Envelope name.
- Amount currently assigned to that envelope.
- A simple color swatch or icon.
- Short status text, such as "safe to spend" or "reserved".
- Small actions to add, subtract, or adjust the envelope amount.

The envelope area should make these totals clear:

- Total cash balance.
- Cash assigned to envelopes.
- Cash not yet assigned to an envelope.

Moving cash between envelopes should not change the total cash balance. Only adding cash, subtracting cash, or manually correcting the balance should change the main cash balance.

### Activity History

The user can see a list of all cash movements:

- Added money.
- Spent money.
- Envelope moves or category adjustments.
- Balance corrections.

## User Experience Requirements

- The first screen should look like a simple bank dashboard.
- The current cash balance should be the most visible information.
- Main actions should be easy to find: modify amount, organize envelopes, view history.
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
- Set aside cash.
- Manual correction.

## User Flow

1. User opens the website.
2. User sees their current cash balance.
3. User modifies their cash amount when it changes.
4. User checks cash categories or envelopes.
5. User reviews history to understand balance changes.

## Known Grey Zones and Corrections

The business spec currently uses "Modify Money Amount" and "Modify Balance" in overlapping ways, so the intended difference should be clarified as quick add or subtract actions versus a full manual balance correction.

The "Modify Balance" heading is placed at a higher level than the surrounding core feature headings, which may be a formatting mistake rather than a separate business section.

The spec mentions a starting amount but does not yet explain how a new user sets that starting amount or whether it can be edited later.

The spec says the website should save history, but it does not yet define whether data is stored only in the browser, synced online, or connected to a user account.

The relationship between balance adjustment history and activity history is a grey zone, because adjustment history may be a subset of the broader activity history rather than a separate list.

The spec does not yet define whether subtracting more than the available cash balance should be blocked or allowed as a negative balance.

The spec should define whether envelope totals must exactly match the cash balance or whether users can leave some cash unassigned.

The spec does not yet define privacy expectations such as a PIN lock, login, or warnings about storing sensitive personal cash information.

The success metrics describe useful outcomes, but they do not yet define measurable targets for regular use, reduced cash loss, or return visits.

## Success Metrics

- Users modify their money amount regularly.
- Users reduce unexplained cash loss.
- Users return to the website to check or update their balance.
