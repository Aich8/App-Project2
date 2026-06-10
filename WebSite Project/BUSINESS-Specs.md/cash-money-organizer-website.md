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

The user can quickly add cash to the balance or subtract cash from the balance when their real cash changes.

This is for normal cash movements, such as receiving cash, spending cash, or removing cash from the tracked amount. It is not the same as correcting the full balance after the displayed amount is wrong.

The user can add, subtract, or modify their money amount as often or as rarely as they want. The app should support many updates in one day and should also work if the user only updates it after a long time.

### Balance Adjustment History

The website should save each added amount, subtracted amount, and manual correction so the user can understand how the balance changed over time.

### Manual Balance Correction

The user can manually correct the total balance whenever they want.

This is for setting the displayed total to the correct real cash amount when the app balance no longer matches reality. The website should save this as an adjustment entry so the history stays clear.

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

The website should not ask for personal information from the user. It is a manual cash tracking tool, not a bank account or financial account.

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

### Corrections Already Clarified

The starting amount is part of the first setup flow. A new user should be able to set an initial cash balance, and later changes should be handled through add, subtract, or manual correction entries so the history stays clear.

The first version stores data locally in the browser. Cloud sync, login, online accounts, and personal information collection are outside the MVP scope unless a later spec adds them.

Activity history is the main history list. Added money, subtracted money, manual balance corrections, and envelope changes should all appear there. Balance adjustment history is a filtered subset of activity history, not a separate competing record.

Envelope totals do not need to use the full cash balance. Users can leave cash unassigned, and the envelope area should show total cash, assigned cash, and unassigned cash.

Subtracting more than the available cash balance should automatically bring the cash balance down to `0`. The balance must not go below `0` or show a negative cash amount.

The MVP does not need a PIN lock, login, or local privacy feature because the app is not a real bank account and should not collect personal information.

The app should not require or expect regular usage. A user can modify their cash amount many times in one day, or only once after a long time, and both patterns are valid.

### Still Open

No known business-spec grey zones remain.

## Success Criteria

- Users can add, subtract, or manually correct their money amount whenever they need to.
- Users can make many changes in one day without hitting a usage limit.
- Users can return after a long time and still understand or update their cash balance.
- Users can review history to understand how their balance changed.
