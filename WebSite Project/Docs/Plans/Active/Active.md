# Cash Money Organizer MVP Plan

Status: Active

Source specs:
- `BUSINESS-Specs.md/cash-money-organizer-website.md`
- `Docs/Specs/Global-Spec.md`
- `Docs/Specs/functional-map.md`
- `Docs/Specs/Functional/functional1.md`

## Goal

Build the first usable version of the Cash Money Organizer website: a browser-based personal cash tracking tool that feels calm and bank-like, while clearly communicating that it is not a real bank account.

The MVP should help a user:
- See their current cash balance.
- Add or subtract cash manually.
- Correct the balance when needed.
- Track balance history.
- Divide cash into simple categories or virtual envelopes.
- Review recent activity.

## MVP Scope

Included in the first version:
- Dashboard with current cash balance as the main focus.
- Small `+` and `-` actions directly under the main balance.
- Add money flow with amount, date, and optional note.
- Subtract money flow with amount, date, and optional note.
- Manual balance correction flow with new total amount, date, and optional note.
- Balance adjustment history showing previous amount, new amount, difference, date, type, and note.
- Cash envelopes for categories like Daily Spending, Food, Transport, Bills, Set Aside Cash, and Emergency.
- Envelope summaries showing assigned cash and unassigned cash.
- Activity history for added money, subtracted money, envelope changes, and corrections.
- Edit and delete support for activity entries where practical.
- Responsive layout for mobile and desktop.
- Clear trust wording that this is a manual cash tracking tool, not a real bank.

Out of scope for the first version:
- Bank connections.
- Payments or transfers.
- Credit cards or debit cards.
- Real account numbers.
- Loans.
- Investments.
- Multi-user business accounting.
- Cloud sync or login.
- Personal information collection.
- PIN lock or app passcode.
- Multiple currencies.

## Product Principles

- The balance should always be easy to find and understand.
- Manual cash changes should feel quick, low-friction, and safe.
- Users can add, subtract, or correct their cash amount as often or as rarely as they want.
- The interface can look bank-like, but the wording must not imply real banking.
- The website should use plain language: "Cash balance", "Add to balance", "Subtract from balance", "Modify amount", "Manual correction", and "Set aside cash".
- The website should not ask for personal information because it is only a manual cash tracking tool.
- The first screen should be useful immediately, especially on a phone.
- The user should not need financial knowledge to use the app.

## Information Architecture

Primary areas:
- Dashboard
- Activity History
- Cash Categories / Envelopes
- Settings or Data Management

Suggested first-screen layout:
- Top area: app name and trust label such as "Manual cash tracker".
- Main balance area: current cash balance.
- Quick actions: small `+` and `-` icon buttons under the balance.
- Secondary actions: modify amount, manage envelopes.
- Overview area: recent activity, envelope summary, monthly overview.

## Core Data Model

Balance:
- Current amount.
- Starting amount.
- Last updated date.

Activity entry:
- ID.
- Type: added, subtracted, correction, envelope change.
- Amount.
- Previous balance.
- New balance.
- Difference.
- Date.
- Optional note.
- Created date.
- Updated date.

Cash envelope:
- ID.
- Name.
- Amount.
- Optional color or icon.
- Optional description.
- Created date.
- Updated date.

## Milestones

### Milestone 1: Project Foundation

Tasks:
- Choose the app stack and folder structure.
- Create the main application shell.
- Add responsive layout foundations.
- Add basic local data persistence.
- Define shared data types for balance, activities, and envelopes.

Acceptance criteria:
- The app opens in a browser.
- The layout works on desktop and mobile widths.
- Data can be saved locally and restored after refresh.

### Milestone 2: Cash Dashboard

Tasks:
- Build the dashboard screen.
- Display the current cash balance prominently.
- Add small `+` and `-` icon buttons under the balance.
- Add clear manual-tracker wording.
- Add empty states for no history and no envelopes.

Acceptance criteria:
- The balance is the most visible item on the first screen.
- The `+` and `-` actions are visually connected to the balance.
- The interface does not use misleading bank wording.

### Milestone 3: Manual Balance Changes

Tasks:
- Build add money flow.
- Build subtract money flow.
- Build modify/correct balance flow.
- Validate amount inputs.
- Save each change to activity history.
- Recalculate the current balance after each action.

Acceptance criteria:
- Adding money increases the balance and creates a positive activity entry.
- Subtracting money decreases the balance and creates a negative activity entry.
- Subtracting more than the current balance sets the balance to `0` automatically.
- There is no limit on how many times a user can add, subtract, or correct the balance.
- Correcting the balance stores previous amount, new amount, and difference.
- Invalid amounts are blocked with useful feedback.

### Milestone 4: Activity History

Tasks:
- Build activity history list.
- Show type, amount, date, balance change, and note.
- Add filtering or grouping if needed for readability.
- Add edit support for activity entries where safe.
- Add delete support with balance recalculation.

Acceptance criteria:
- The user can understand how their cash balance changed over time.
- Editing or deleting an entry keeps the balance consistent.
- Recent activity is visible from the dashboard.

### Milestone 5: Cash Categories / Envelopes

Tasks:
- Build envelope list.
- Add default envelope suggestions: Daily Spending, Food, Transport, Bills, Set Aside Cash, Emergency.
- Build create, edit, and delete envelope actions.
- Allow assigning or adjusting envelope amounts.
- Show total allocated cash and unallocated cash.
- Allow moving cash between envelopes without changing the main balance.

Acceptance criteria:
- The user can divide their cash into virtual envelopes.
- Envelope totals are understandable.
- The app helps the user see how much cash is safe to spend.
- Envelope changes do not imply real bank transfers or a separate target-tracking feature.

### Milestone 6: Monthly Overview and Polish

Tasks:
- Add a simple monthly overview.
- Show money added, money subtracted, corrections, and net change.
- Improve mobile spacing and touch targets.
- Add helpful empty states.
- Review wording for trust and clarity.
- Add final visual polish.

Acceptance criteria:
- The monthly overview gives a quick sense of cash movement.
- The app feels calm, trustworthy, and practical.
- The interface is usable on phone-sized screens.

### Milestone 7: Verification and Handoff

Tasks:
- Test the main user flow end to end.
- Test data persistence after refresh.
- Test mobile and desktop layouts.
- Test invalid inputs.
- Test edit and delete behavior.
- Confirm out-of-scope banking features are not implied.
- Move this plan from `Docs/Plans/Active` to `Docs/Plans/Done` after completion.

Acceptance criteria:
- A user can open the app, set a balance, add cash, subtract cash, create an envelope, and review history.
- No real banking language or fake account behavior is present.
- The MVP satisfies the business spec's first-version goals.

## Main User Flow

1. User opens the website.
2. User sees their current cash balance.
3. User taps `+` to add money or `-` to subtract money.
4. User enters an amount and optional note.
5. App updates the cash balance.
6. App saves the action in history.
7. User reviews envelopes.
8. User checks activity history to understand changes.

## Validation Rules

- Amounts must be numeric.
- Amounts must be greater than zero for add and subtract actions.
- Subtracting more than the current balance should set the balance to `0` automatically; the balance must never go below `0`.
- Envelope amount should not be negative.
- Dates should default to the current date but remain editable if needed.
- Notes should be optional.

## Trust and Wording Checklist

Avoid:
- Deposit to bank.
- Withdraw from bank.
- Real account number.
- Card balance.
- Bank transfer.
- Payment.

Use:
- Add to balance.
- Subtract from balance.
- Modify amount.
- Cash balance.
- Set aside cash.
- Manual correction.
- Manual cash tracker.

## Testing Checklist

- New user can set or modify starting balance.
- Add money updates balance correctly.
- Subtract money updates balance correctly.
- Subtracting more than the current balance sets the balance to `0`.
- Repeated balance changes work without a daily, weekly, monthly, or yearly limit.
- Manual correction records previous amount and new amount.
- Activity history shows correct differences.
- Editing an activity updates derived balance correctly.
- Deleting an activity updates derived balance correctly.
- Envelopes can be created, edited, and deleted.
- App reload keeps saved data.
- Layout works on mobile.
- Layout works on desktop.
- Wording does not imply real banking.

## Open Questions

- Should the first version support editing dates, or only use the current date?

## Done Criteria

This plan can move to `Docs/Plans/Done` when:
- The MVP features listed in scope are implemented.
- The main user flow works end to end.
- Manual balance changes are saved in history.
- Envelopes are usable.
- Mobile and desktop layouts have been checked.
- The trust wording has been reviewed.
- Any remaining known limitations are documented.
