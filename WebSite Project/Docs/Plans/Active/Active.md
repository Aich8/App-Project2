# Cash Money Organizer MVP Plan

Status: Active

Source specs:
- `BUSINESS-Specs.md/cash-money-organizer-website.md`
- `Docs/Specs/global-Spec.md`
- `Docs/Specs/functional-map.md`
- `Docs/Specs/Functional/functional1.md`
- `Docs/Specs/Technical/technical1.md`

## Goal

Build the first usable version of the Cash Money Organizer website: a browser-based personal cash tracking tool that feels calm and bank-like, while clearly communicating that it is not a real bank account.

The MVP should help a user:
- See their money amount with the user-facing label `Current Balance`.
- Set their first money amount.
- Add or subtract money manually after setup.
- Correct mistakes with a modify action when needed.
- Track one-month visible activity history.
- Keep saved money data after closing and reopening the website in the same browser.
- Organize savings into user-named folders or squares.
- See what money would be left after setting money aside in `Savings`.
- Divide cash into simple virtual envelopes.
- Review recent activity.

## MVP Scope

Included in the first version:
- Dashboard with the main money amount as the main focus.
- First money amount setup that saves the first entry as `+{money amount} added`.
- `Modify`, `Add`, and `Subtract` actions directly connected to the main money amount.
- Browser storage for saved money amount, one-month visible activity history, savings folders or squares, envelopes, and basic app settings.
- Add money flow with amount, date, and optional note.
- Subtract money flow with amount, date, and optional note.
- Silent modify flow with corrected total amount.
- One-month visible activity history where each add and subtract action stays as its own entry.
- Savings planning section with available savings amount and user-named folders or squares for money set aside.
- Cash envelopes for categories like Spending, Savings, Bills, and Emergency.
- Activity history for added money, subtracted money, savings folder or square changes, and envelope changes.
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
- Email account creation.
- Server database persistence.
- Multiple currencies.

## Product Principles

- The money amount should always be easy to find and understand.
- Manual money changes should feel quick, low-friction, and safe.
- The interface can look bank-like, but the wording must not imply real banking.
- The website should use plain language: "Current Balance", "Add money", "Subtract money", "Modify amount", and "Saved cash".
- Use `Current Balance` as the user-facing label for the main money amount.
- Use `money amount` in specs and implementation planning when explaining what the value means.
- The first version should save data only in the user's browser storage.
- The app should clearly explain that browser storage is not an online account or cloud backup.
- The first screen should be useful immediately, especially on a phone.
- The user should not need financial knowledge to use the app.
- The app should support rare use and very frequent use without requiring a specific update schedule.

## Information Architecture

Primary areas:
- Dashboard
- Activity History
- Savings
- Cash Envelopes
- Settings or Data Management

Suggested first-screen layout:
- Top area: app name and trust label such as "Manual cash tracker".
- Main money amount area: user-facing label `Current Balance`.
- Quick actions after setup: `Modify`, `Add`, and `Subtract` controls near the main money amount.
- Secondary actions: open savings, manage envelopes, view history.
- Overview area: recent activity, savings summary, envelope summary, monthly overview.

## Core Data Model

Money amount:
- Current money amount.
- Starting amount.
- Last updated date.

Browser storage:
- Storage key.
- Data version.
- Current money amount data.
- One-month visible activity entries.
- Savings folders or squares.
- Cash envelopes.
- Basic app settings if needed.
- Last saved date.

Activity entry:
- ID.
- Type: added, subtracted, savings folder or square change, envelope change.
- Amount.
- Previous money amount.
- New money amount.
- Difference.
- Date.
- Visible until date.
- Optional note.
- Created date.
- Updated date.

Savings folder or square:
- ID.
- Name.
- Amount set aside.
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
- Add browser storage persistence.
- Define one stable storage key for app data.
- Add a data version number for saved data.
- Define shared data types for money amount, activities, savings folders or squares, and envelopes.

Acceptance criteria:
- The app opens in a browser.
- The layout works on desktop and mobile widths.
- Data can be saved in browser storage and restored after refresh.
- Data can be restored after closing and reopening the website in the same browser.
- If no saved data exists, the app shows first money amount setup.

### Milestone 2: Cash Dashboard

Tasks:
- Build the dashboard screen.
- Display the main money amount prominently with the label `Current Balance`.
- Add `Modify`, `Add`, and `Subtract` controls near the main money amount.
- Add clear manual-tracker wording.
- Add empty states for no history, no savings folders or squares, and no envelopes.

Acceptance criteria:
- The main money amount is the most visible item on the first screen.
- The `Modify`, `Add`, and `Subtract` actions are visually connected to the main money amount.
- The interface does not use misleading bank wording.

### Milestone 3: Manual Money Amount Changes

Tasks:
- Build first money amount setup.
- Build add money flow.
- Build subtract money flow.
- Build silent modify/correct money amount flow.
- Validate amount inputs.
- Save each `Add` and `Subtract` action to activity history as its own separate entry.
- Do not combine separate `Add` and `Subtract` actions into one net history result.
- Do not save `Modify` actions to activity history.
- Save each successful money change to browser storage.
- Recalculate the current money amount after each action.

Acceptance criteria:
- The first ever money amount creates a `+{money amount} added` history entry.
- Adding money increases the money amount and creates a positive activity entry.
- Subtracting money decreases the money amount and creates a negative activity entry.
- Subtracting more than the current money amount sets the money amount to `$0` instead of creating a negative money amount.
- Subtracting when the current money amount is `$0` keeps the money amount at `$0` and does not create a history entry.
- Modifying the money amount replaces the current money amount without creating history, activity, or notification entries.
- Separate `Add` and `Subtract` actions stay separate in history.
- Money changes are still visible after page refresh.
- Invalid amounts are blocked with useful feedback.

### Milestone 4: Activity History

Tasks:
- Build activity history list.
- Show type, amount, date, money amount change, and note.
- Keep each add and subtract action as its own visible entry.
- Do not replace separate entries with only a combined net result.
- Hide or remove visible history entries after one month.
- Keep the current money amount unchanged when old history entries expire.
- Add edit support for activity entries where safe.
- Add delete support with money amount recalculation.

Acceptance criteria:
- The user can understand how their money amount changed over time.
- The user sees separate entries such as `+$56 added` and `-$34 subtracted`.
- The activity history does not show only a combined result such as `+$22 net change`.
- Activity history entries older than one month are no longer shown.
- Removing old history entries does not change the current money amount.
- Editing or deleting an entry keeps the money amount consistent.
- Recent activity is visible from the dashboard.

### Milestone 5: Savings Section and Folders

Tasks:
- Build the Savings section or page.
- Show the savings available amount at the top.
- Start savings available amount from the current money amount.
- Calculate savings available amount as current money amount minus total money set aside in savings folders or squares.
- Build create, rename, and delete folder or square actions.
- Build set-aside-in-folder and remove-from-folder flows.
- Record savings organization changes in activity history if activity history includes savings entries.
- Save savings folder or square changes to browser storage.

Acceptance criteria:
- Savings available amount starts equal to the current money amount when no folders exist.
- Creating a folder or square and setting money aside inside it reduces only the savings available amount.
- Setting money aside in a folder or square does not reduce the main money amount.
- The Savings section helps the user see what money would be left after reserving money for future needs.
- Savings folder or square changes do not show as added or subtracted cash.
- A user can create at least one savings folder or square.
- Savings changes do not make the total money amount confusing.

### Milestone 6: Cash Envelopes

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
- Show recent money activity without replacing separate add and subtract entries.
- Improve mobile spacing and touch targets.
- Add helpful empty states.
- Review wording for trust and clarity.
- Add final visual polish.

Acceptance criteria:
- The monthly overview gives a quick sense of money movement.
- The app feels calm, trustworthy, and practical.
- The interface is usable on phone-sized screens.

### Milestone 7: Verification and Handoff

Tasks:
- Test the main user flow end to end.
- Test data persistence after refresh.
- Test data persistence after closing and reopening the website in the same browser.
- Test first load with no browser storage data.
- Test unreadable or broken browser storage data.
- Test mobile and desktop layouts.
- Test invalid inputs.
- Test edit and delete behavior.
- Confirm out-of-scope banking features are not implied.
- Move this plan from `Docs/Plans/Active` to `Docs/Plans/Done` after completion.

Acceptance criteria:
- A user can open the app, set a money amount, add cash, subtract cash, create a savings folder or square, create an envelope, and review history.
- No real banking language or fake account behavior is present.
- The MVP satisfies the business spec's first-version goals.

## Main User Flow

1. User opens the website.
2. If no money amount exists yet, user enters their first money amount.
3. App saves the first amount as `+{money amount} added`.
4. User sees their money amount labeled as `Current Balance`.
5. User chooses `Add`, `Subtract`, or `Modify`.
6. User enters an amount and optional note.
7. App updates the money amount.
8. If the action is `Add` or `Subtract`, app saves it as its own history entry with the correct display type.
9. If the action is `Modify`, app updates the current money amount without saving history or showing a notification.
10. App saves the updated data in browser storage.
11. User reviews savings folders or squares and envelopes.
12. User checks activity history to understand recent visible changes.
13. User can close the website and open it again later in the same browser to see the saved data.

## Validation Rules

- Amounts must be numeric.
- Amounts must be greater than zero for add and subtract actions.
- Subtracting more than the current money amount should set the current money amount to `$0`.
- Subtracting more than the current money amount should save history using the actual amount removed from the money amount.
- Subtracting when the current money amount is `$0` should not create a history entry.
- The current money amount should never be negative.
- Savings folder or square name is required.
- Savings folder or square amount should not be negative.
- Total money set aside in savings folders or squares should not be greater than the current money amount.
- Envelope amount should not be negative.
- Dates should default to the current date but remain editable if needed.
- Notes should be optional.
- Browser storage data should be checked before use so broken saved data does not crash the app.
- Visible history entries should be shown for one month.
- Expiring old history entries should not change the current money amount.

## Browser Storage Rules

- Save app data in browser storage on the user's device.
- The first version should not use email accounts, login, cloud sync, or a server database.
- Save after every successful money amount change, savings folder or square change, envelope change, or history edit.
- Save `Modify` changes only as an updated current money amount, not as a history entry.
- Load saved browser data before showing the dashboard.
- If saved browser data exists, restore the money amount, one-month visible activity history, savings folders or squares, and cash envelopes.
- Hide or remove visible history entries older than one month without changing the saved current money amount.
- If no saved browser data exists, show first money amount setup.
- If saved browser data is broken or unreadable, show a clear error and let the user choose whether to start again.
- Use one stable storage key for the app data.
- Include a data version number in the saved data so future versions can upgrade old data safely.
- Tell the user that the data is saved only in the same browser and device.
- Tell the user that data may disappear if they clear browser data, use private browsing, change device, change browser, or uninstall the browser.

## Trust and Wording Checklist

Avoid:
- Deposit to bank.
- Withdraw from bank.
- Real account number.
- Card-related money wording.
- Bank transfer.
- Payment.

Use:
- Add money.
- Subtract money.
- Modify amount.
- Current Balance.
- Saved cash.
- Manual cash tracker.

## Testing Checklist

- New user can set the first money amount.
- First money amount appears in history as `+{money amount} added`.
- Add money updates the money amount correctly.
- Subtract money updates the money amount correctly.
- Subtracting more than the current money amount sets the money amount to `$0`.
- Subtracting more than the current money amount saves the actual removed amount in history.
- Subtracting when the current money amount is `$0` does not create a history entry.
- The app never shows a negative money amount.
- Modify updates the current money amount without creating history, activity, or notification entries.
- Add and subtract entries stay separate in history.
- History does not replace separate entries with only a net result.
- Activity history entries older than one month are hidden or removed.
- Expiring old history entries does not change the current money amount.
- Activity history shows correct differences.
- Editing an activity updates the derived money amount correctly.
- Deleting an activity updates the derived money amount correctly.
- Browser storage saves the current money amount and history.
- Browser storage restores the current money amount and history after refresh.
- Browser storage restores data after closing and reopening the website in the same browser.
- First money amount setup appears when there is no saved browser data.
- Broken browser storage data shows a clear recovery message.
- The app works whether the user updates money rarely or many times in one day.
- Savings folders or squares can be created, renamed, updated, and deleted.
- Setting aside `$40` in a `Rent` savings folder or square changes savings available from `$350` to `$310` when the main money amount is `$350`.
- Setting money aside in a savings folder or square does not change the main money amount.
- Envelopes can be created, edited, and deleted.
- App reload keeps saved data.
- Layout works on mobile.
- Layout works on desktop.
- Wording does not imply real banking.

## Open Questions

- Should envelope totals be required to match the money amount?
- Should the first version support editing dates, or only use the current date?

## Done Criteria

This plan can move to `Docs/Plans/Done` when:
- The MVP features listed in scope are implemented.
- The main user flow works end to end.
- Add and subtract changes are saved in history as separate entries.
- Modify changes are saved only as the current money amount, not as history.
- Savings folders or squares and envelopes are usable.
- Mobile and desktop layouts have been checked.
- The trust wording has been reviewed.
- Any remaining known limitations are documented.
