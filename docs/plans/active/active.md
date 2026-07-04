# Cash Money Organizer MVP Plan

Status: Active

Source specs:
- `docs/specs/business/cash-money-organizer-website.md`
- `docs/specs/global.md`
- `docs/specs/functional-map.md`
- `docs/specs/functional/functional1.md`
- `docs/specs/technical/technical1.md`

## Goal

Build the first usable version of the Cash Money Organizer website: a browser-based personal cash tracking tool that feels calm and bank-like, while clearly communicating that it is not a real bank account.

The MVP should help a user:
- See their money amount with the user-facing label `Current Balance`.
- Start with the money amount shown as `0$` when no saved data exists.
- Click the main money amount at `0$` to show only `Add`.
- Add money manually, then use `Modify`, `Add`, and `Subtract` after the money amount is greater than `0$`.
- Correct mistakes with `Modify` after the money amount is greater than `0$`.
- See 30-day visible money amount change history directly under the main money amount.
- Keep saved money data after closing and reopening the website when saved data is available.
- Organize the `Savings` section into user-named `Saving` squares.
- See which `Saving` squares are fully covered, partly covered, or not covered.
- See what money amount is left inside `Savings` with the user-facing label `Savings money amount`.
- Review `Balance Changes`.

## MVP Scope

Included in the first version:
- Dashboard with the main money amount as the main focus.
- Default `0$` money amount when no saved data exists.
- Clickable main money amount that shows only `Add` at `0$`, then shows `Modify`, `Add`, and `Subtract` when the money amount is greater than `0$`.
- Browser storage for saved money amount, 30-day visible `Balance Changes` history, and `Saving` squares.
- No saved website settings in the first version because no settings exist yet.
- Add money flow with amount.
- Subtract money flow with amount.
- Silent modify flow with corrected total amount.
- 30-day visible money amount change history shown directly under the main money amount, where each add and subtract action stays as its own entry.
- Newest `Balance Changes` entries shown first.
- `Savings` planning section with a money amount shown inside `Savings` using the label `Savings money amount`, ordered user-named `Saving` squares, and coverage bars.
- `Balance Changes` history for added money and subtracted money.
- Delete support for `Balance Changes` entries that asks for confirmation with `Delete this Balance Change?`, shows `Cancel` and `Delete`, removes only the visible history entry, and does not offer undo.
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
- Personal information collection.
- PIN lock or website passcode.
- Multiple currencies.

## Product Principles

- The money amount should always be easy to find and understand.
- Manual money changes should feel quick, low-friction, and safe.
- The interface can look bank-like, but the wording must not imply real banking.
- The website should use plain language: "Current Balance", "Add money", "Subtract money", "Modify amount", and "Saved cash".
- Use `Current Balance` as the user-facing label for the main money amount.
- Use `Savings money amount` as the user-facing label for the money amount shown at the top of `Savings`.
- Use `money amount` in specs and implementation planning when explaining what the value means.
- The first version should save data only in the user's browser storage.
- Browser storage is an internal implementation detail and should not be explained to the user.
- The website should not tell the user where saved information is stored.
- The website should not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.
- The website should not ask for personal information because it is only a manual cash tracking tool.
- The first screen should be useful immediately, especially on a phone.
- The user should not need financial knowledge to use the website.
- The website should support rare use and very frequent use without requiring a specific update schedule.

## Information Architecture

Primary areas:
- Dashboard
- Money amount change history under the main money amount
- Savings

Suggested first-screen layout:
- Top area: website name and trust label such as "Manual cash tracker".
- Main money amount area: user-facing label `Current Balance`; clicking the money amount reveals `Add` at `0$`, or `Modify`, `Add`, and `Subtract` when the money amount is greater than `0$`.
- Money amount change history directly under the main money amount.
- Secondary action: click `Savings`.
- Overview area: `Balance Changes` and savings summary.

## Core Data Model

Money amount:
- Money amount.
- Last updated date.

Browser storage:
- Storage key.
- Data version.
- Money amount data.
- 30-day visible `Balance Changes` entries.
- `Saving` squares.
- Last saved date.

The default `0$` starting state is displayed when no saved data exists. It should not write browser storage just because the website opened.

Browser storage should be created after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The first version should not save website settings because no settings exist yet. Add saved settings later only after a real setting is named and specified.

`Balance Changes` entry:
- ID.
- Type: added or subtracted.
- Amount.
- Previous money amount.
- New money amount.
- Difference.
- Internal created date and time used only for 30-day clearing.
- Internal visible until date and time.

`Saving` square:
- ID.
- Name.
- Planned money amount greater than `0$`.
- Order.
- Created date.
- Updated date.

## Milestones

### Milestone 1: Project Foundation

Tasks:
- Choose the website stack and folder structure.
- Create the main application shell.
- Add responsive layout foundations.
- Add browser storage persistence.
- Define one stable storage key for website data.
- Add a data version number for saved data.
- Define shared data types for money amount, `Balance Changes` entries, and `Saving` squares.

Acceptance criteria:
- The website opens in a browser.
- The layout works on desktop and mobile widths.
- Data can be saved in browser storage and restored after refresh.
- Data can be restored after closing and reopening the website in the same browser.
- If no saved data exists, the website shows the dashboard with the money amount set to `0$`.
- Showing the default `0$` money amount does not create saved browser data by itself.
- The first view does not explain where saved data is stored.

### Milestone 2: Cash Dashboard

Tasks:
- Build the dashboard screen.
- Display the main money amount prominently with the label `Current Balance`.
- Make the main money amount clickable.
- Show only `Add` when the user clicks the main money amount at `0$`.
- Show `Modify`, `Add`, and `Subtract` when the user clicks the main money amount above `0$`.
- Show money amount change history directly under the main money amount.
- Add clear manual-tracker wording.
- Add empty states for no history and no `Saving` squares.

Acceptance criteria:
- The main money amount is the most visible item on the first screen.
- Clicking the main money amount at `0$` reveals only `Add`.
- Clicking the main money amount above `0$` reveals `Modify`, `Add`, and `Subtract`.
- The visible money actions are visually connected to the main money amount.
- Money amount change history appears directly under the main money amount.
- There is no separate `View history` action for money amount change history.
- The interface does not use misleading bank wording.

### Milestone 3: Manual Money Amount Changes

Tasks:
- Build the default `0$` starting state.
- Build add money flow.
- Build subtract money flow.
- Build silent modify/correct money amount flow.
- Validate amount inputs.
- Save each `Add` and `Subtract` action to `Balance Changes` as its own separate entry.
- Do not combine separate `Add` and `Subtract` actions into one net history result.
- Do not save `Modify` actions to `Balance Changes`.
- Save each successful money change to browser storage.
- Recalculate the current money amount after each action.

Acceptance criteria:
- The default `0$` starting state does not create a `Balance Changes` entry.
- At `0$`, the main money amount shows only `Add` when clicked.
- Adding money increases the money amount and creates a positive `Balance Changes` entry.
- After adding money above `0$`, the main money amount shows `Modify`, `Add`, and `Subtract` when clicked.
- Subtracting money decreases the money amount and creates a negative `Balance Changes` entry.
- Subtracting more than the current money amount sets the money amount to `0$` instead of creating a negative money amount.
- Subtracting when the current money amount is `0$` keeps the money amount at `0$` and does not create a history entry.
- Modifying the money amount replaces the current money amount without creating history or notification entries.
- Separate `Add` and `Subtract` actions stay separate in history.
- Money changes are still visible after page refresh.
- Invalid amounts are blocked with useful feedback.

### Milestone 4: Balance Changes

Tasks:
- Build the `Balance Changes` list directly under the main money amount.
- Show type, amount, and money amount change.
- Keep each add and subtract action as its own visible entry.
- Show newest `Balance Changes` entries first.
- Do not ask the user to choose a date for `Add` or `Subtract`.
- Do not replace separate entries with only a combined net result.
- Calculate each `Balance Changes` internal visible until date and time as the internal created date and time plus 30 days.
- Delete visible history entries from browser storage at or after their visible until date.
- Keep the current money amount unchanged when old history entries expire.
- Let the user scroll down when the history list is longer than the screen.
- Add delete support that asks for confirmation with `Delete this Balance Change?`, shows `Cancel` and `Delete`, removes only the visible history entry, does not change the current money amount, and does not offer undo.

Acceptance criteria:
- The user can understand how their money amount changed over time.
- The user sees separate entries such as `+56$ added` and `-34$ subtracted`.
- The newest `Balance Changes` entry appears first.
- `Balance Changes` does not show only a combined result such as `+22$ net change`.
- `Balance Changes` entries at or after their visible until date are deleted from browser storage and no longer shown.
- Removing old history entries does not change the current money amount.
- The user can scroll down to see more history entries when needed.
- Deleting an entry asks for confirmation first with `Delete this Balance Change?`, shows `Cancel` and `Delete`, removes only the visible history entry, does not change the current money amount, and does not offer undo.
- Saved `Balance Changes` entries cannot be edited.
- Recent money amount change history is visible from the dashboard without a separate `View history` action.

### Milestone 5: Savings Section and Saving Squares

Tasks:
- Build the Savings section or page.
- Treat `Savings` as the separate planning section.
- Treat a `Saving` as one user-named square inside the `Savings` section.
- Let the user open the Savings section by clicking `Savings`.
- Do not add a separate `Open savings` action.
- Show the money amount inside `Savings` at the top with the user-facing label `Savings money amount`.
- Start the money amount inside `Savings` from the current money amount.
- Calculate the money amount inside `Savings` as current money amount minus total planned money amount in `Saving` squares, stopped at `0$`.
- Calculate `Saving` square coverage from top to bottom using the main money amount.
- Treat the visible `Saving` square order as the coverage order.
- Add thin horizontal coverage bars at the very bottom of `Saving` squares.
- Show full green bars for fully covered `Saving` squares, left-to-right partly green bars for partly covered squares, and grey bars for uncovered squares.
- Show a `{money amount} needed` note at the top-left of the bottom coverage bar when a `Saving` square is not fully covered.
- Build the add-a-saving action as a circle with a `+` sign.
- Require the user to enter a `Saving` name and planned money amount before creating the `Saving` square.
- Require each `Saving` square name to be unique inside `Savings`.
- Build create, rename, delete, and reorder square actions.
- Block creating or renaming a `Saving` square with a duplicate name.
- Make delete ask for confirmation with `Delete this Saving?`, show `Cancel` and `Delete`, remove only the selected `Saving` square and its saved details, and not offer undo.
- Build planned-money-amount editing flows that replace the old planned money amount with a new total planned money amount.
- Remove a `Saving` square if its planned money amount becomes `0$`.
- Save `Saving` square changes to browser storage.
- Save the final `Saving` square order to browser storage after the user finishes moving a square and lets go.
- Recalculate the money amount shown inside `Savings` and coverage bars after the final reorder.

Acceptance criteria:
- Money amount shown inside `Savings` starts equal to the current money amount when no `Saving` squares exist.
- The money amount shown at the top of `Savings` uses the label `Savings money amount`.
- The main money amount keeps the label `Current Balance`.
- Creating a `Saving` square with a planned money amount reduces only the money amount shown inside `Savings`.
- A planned money amount in a `Saving` square does not reduce the main money amount.
- `Saving` square planned money amounts do not change automatically when the main money amount changes.
- Changing a `Saving` square planned money amount replaces the old planned money amount instead of adding to or subtracting from it.
- Changing a `Saving` square planned money amount updates the money amount shown inside `Savings` and coverage bars.
- Changing a `Saving` square planned money amount does not change the main money amount or create a `Balance Changes` entry.
- The Savings section helps the user see which plans are fully covered, partly covered, or not covered.
- The user can add a `Saving` square with a circle `+` action.
- A new `Saving` square is created only after the user enters a valid name and a planned money amount greater than `0$`.
- A new `Saving` square is not created when its name duplicates another `Saving` square name.
- Renaming a `Saving` square to a duplicate name is blocked and keeps the old name.
- Duplicate `Saving` square name checks use trimmed names and ignore uppercase or lowercase differences.
- A new `Saving` square is not created with a `0$` planned money amount.
- The planned money amount in a `Saving` square does not mean money has moved into a separate place.
- A `Saving` square disappears from `Savings` if its planned money amount becomes `0$`.
- A `0$` `Saving` square is not saved in browser storage.
- Deleting a `Saving` square removes only that square and its saved details.
- Deleting a `Saving` square does not change the main money amount, `Balance Changes`, or the names, planned money amounts, and relative order of other `Saving` squares.
- After deleting a `Saving` square, the money amount shown inside `Savings` and coverage bars update from the remaining squares.
- The user can reorder `Saving` squares by holding and moving them.
- The visible `Saving` square order is the coverage order.
- A `Saving` square moved to the top is checked first and its coverage bar is calculated before lower squares use what remains.
- Reordering `Saving` squares saves the final order after the user lets go.
- Reordering `Saving` squares updates the money amount shown inside `Savings` and coverage bars from the new visible order.
- Reordering `Saving` squares does not change the main money amount and does not create a `Balance Changes` entry.
- Coverage bars fill green from left to right by the covered percentage of each `Saving` square.
- A `Saving` square that is 80% covered shows the left 80% of the coverage bar as green and the right 20% as grey.
- A not fully covered `Saving` square shows its `{money amount} needed` note at the top-left of the bottom coverage bar.
- If the total planned money amount in `Saving` squares is greater than the main money amount, the money amount shown inside `Savings` is `0$`.
- The user opens the Savings section by clicking `Savings`, not by using a separate `Open savings` action.
- `Saving` square changes do not show as added or subtracted cash.
- A user can create at least one `Saving` square.
- Savings changes do not make the total money amount confusing.

### Milestone 6: Polish

Tasks:
- Show `Balance Changes` without replacing separate add and subtract entries.
- Improve mobile spacing and touch targets.
- Add helpful empty states.
- Review wording for trust and clarity.
- Confirm the website does not explain where saved data is stored and does not warn that saved data may disappear after browser or device changes.
- Add final visual polish.

Acceptance criteria:
- The website feels calm, trustworthy, and practical.
- The interface is usable on phone-sized screens.

### Milestone 7: Verification and Handoff

Tasks:
- Test the main user flow end to end.
- Test data persistence after refresh.
- Test data persistence after closing and reopening the website when saved data is available.
- Test first load with no browser storage data.
- Test unreadable or broken browser storage data.
- Test mobile and desktop layouts.
- Test invalid inputs.
- Test delete confirmation behavior for `Balance Changes` entries, including the exact message `Delete this Balance Change?` and buttons `Cancel` and `Delete`.
- Confirm out-of-scope banking features are not implied.
- Move this plan from `docs/plans/active` to `docs/plans/done` after completion.

Acceptance criteria:
- A user can open the website, see `0$`, add cash, subtract cash, create a `Saving` square, and review `Balance Changes`.
- No real banking language or fake account behavior is present.
- The MVP satisfies the business spec's first-version goals.

## Main User Flow

1. User opens the website.
2. If no saved money amount exists yet, website shows `0$` labeled as `Current Balance`.
3. User clicks the main money amount.
4. Website shows only `Add`.
5. User chooses `Add`.
6. User enters an amount greater than `0$`.
7. Website updates the money amount and saves the add action as `+{money amount} added`.
8. After the money amount is greater than `0$`, user clicks the main money amount.
9. Website shows `Modify`, `Add`, and `Subtract`.
10. User chooses `Add`, `Subtract`, or `Modify`.
11. User enters an amount.
12. Website updates the money amount.
13. If the action is `Add` or `Subtract`, website saves it as its own history entry with the correct display type.
14. If the action is `Modify`, website updates the current money amount without saving history or showing a notification.
15. Website shows money amount change history directly under the main money amount.
16. Website saves the updated data in browser storage.
17. User reviews `Saving` squares.
18. User can close the website and open it again later to see saved data when it is available.

## Validation Rules

- Amounts must be numeric.
- Amounts may include cents, with up to two digits after the decimal point.
- Users should enter only the number, without typing the `$` sign.
- Users may type a decimal point, such as `14.56`.
- The website should save and show money amounts with the `$` sign at the end, such as `14.56$`.
- Saved money amounts should use the decimal money amount with the `$` sign at the end, such as `14.56$`, not integer cents such as `1456`.
- Amounts with more than two digits after the decimal point should be blocked.
- Amounts must be greater than zero for add and subtract actions.
- Modify amounts must be `0$` or more.
- Negative amounts should be blocked for all money actions.
- Subtracting more than the current money amount should set the current money amount to `0$`.
- Subtracting more than the current money amount should save history using the actual amount removed from the money amount.
- Subtracting when the current money amount is `0$` should not create a history entry.
- The current money amount should never be negative.
- `Saving` square name is required.
- `Saving` square name should be unique inside `Savings`.
- Duplicate `Saving` square name checks should use trimmed names and ignore uppercase or lowercase differences.
- `Saving` square planned money amount should be greater than `0$` when creating a `Saving` square.
- `0$` should remove an existing `Saving` square instead of saving it with a `0$` planned money amount.
- Changing a `Saving` square planned money amount should use a new total planned money amount, not an add or subtract amount.
- Total planned money amount in `Saving` squares may be greater than the current money amount. The money amount shown inside `Savings` should stop at `0$`, and coverage bars should show what is still needed.
- Dates are used only internally for clearing `Balance Changes` entries after 30 days.
- The user should not choose dates for `Add` or `Subtract`.
- `Add`, `Subtract`, and `Balance Changes` entries should not have notes.
- Browser storage data should be checked before use so broken saved data does not crash the website.
- Broken saved data should show `Saved data could not be loaded.` and a `Start again` action.
- Clicking `Start again` should delete the broken saved data, start fresh at `0$`, show empty `Balance Changes`, and show no saved `Saving` squares.
- Visible history entries should be shown for 30 days.
- For visible `Balance Changes` history, one month means 30 days, not a calendar month.
- Each `Balance Changes` internal visible until date and time should be calculated from the internal created date and time plus 30 days.
- Expiring old history entries should not change the current money amount.

## Browser Storage Rules

- Save website data in browser storage on the user's device.
- The first version should not use email accounts, login, cloud sync, or a server database.
- Do not create saved browser data only because the website opened and showed the default `0$` money amount.
- Save after every successful money amount change, `Saving` square change, or `Balance Changes` delete.
- Save `Modify` changes only as an updated current money amount, not as a history entry.
- Save `Balance Changes` deletes only as visible history removal, not as a money amount change.
- Load saved browser data before showing the dashboard.
- If saved browser data exists, restore the money amount, 30-day visible `Balance Changes` history, and `Saving` squares.
- Delete visible history entries from browser storage at or after their visible until date without changing the saved current money amount.
- If no saved browser data exists, show the dashboard with the money amount set to `0$`.
- If saved browser data is broken or unreadable, show `Saved data could not be loaded.` and let the user choose `Start again`.
- Use one stable storage key for the website data.
- Include a data version number in the saved data so future versions can upgrade old data safely.
- Do not tell the user where saved information is stored.
- Do not tell the user that saved information belongs only to the same browser or device.
- Do not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

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
- Savings money amount.
- Saved cash.
- Manual cash tracker.

## Testing Checklist

- New user sees the money amount as `0$` when no saved data exists.
- Showing the default `0$` money amount does not create saved browser data by itself.
- The default `0$` starting state does not create a `Balance Changes` entry.
- Clicking the main money amount at `0$` shows only `Add`.
- After a successful add above `0$`, clicking the main money amount shows `Modify`, `Add`, and `Subtract`.
- Add money updates the money amount correctly.
- Add money accepts a decimal amount such as `14.56` and saves it as `14.56$`.
- Subtract money updates the money amount correctly.
- Subtracting more than the current money amount sets the money amount to `0$`.
- Subtracting more than the current money amount saves the actual removed amount in history.
- Subtracting when the current money amount is `0$` does not create a history entry.
- The website never shows a negative money amount.
- Modify updates the current money amount without creating history or notification entries after the money amount is greater than `0$`.
- Add and subtract entries stay separate in history.
- Newest `Balance Changes` entries appear first.
- History does not replace separate entries with only a net result.
- Money amount change history appears directly under the main money amount.
- There is no separate `View history` action for money amount change history.
- Long money amount change history can be reached by scrolling down.
- `Balance Changes` entries at or after their visible until date are deleted from browser storage.
- `Balance Changes` internal visible until dates are calculated from the internal created date and time plus 30 days.
- Expiring old history entries does not change the current money amount.
- `Balance Changes` shows correct differences.
- Deleting a `Balance Changes` entry removes only that visible history entry and does not change the current money amount.
- Deleting a `Balance Changes` entry asks for confirmation first with `Delete this Balance Change?`, shows `Cancel` and `Delete`, and does not offer undo.
- Saved `Balance Changes` entries cannot be edited.
- Browser storage saves the current money amount and history.
- Browser storage restores the current money amount and history after refresh.
- Browser storage restores data after closing and reopening the website in the same browser.
- The first view does not explain where saved data is stored.
- The first view does not warn that saved data may disappear after browser or device changes.
- The default `0$` dashboard appears when there is no saved browser data.
- Broken browser storage data shows a clear recovery message.
- Clicking `Start again` after broken browser storage data deletes the broken saved data and starts fresh at `0$` with empty `Balance Changes` and no saved `Saving` squares.
- The website works whether the user updates money rarely or many times in one day.
- `Saving` squares can be created, renamed, updated, and deleted.
- Creating or renaming a `Saving` square with a duplicate name is blocked.
- `Saving` squares can be reordered by holding and moving them.
- Moving the last `Saving` square to the top makes that square checked first, saves the final order after the user lets go, and recalculates coverage bars from the new visible order.
- Clicking `Savings` opens the Savings section.
- The money amount shown at the top of `Savings` is labeled `Savings money amount`.
- The main money amount is still labeled `Current Balance`.
- Creating a `Rent` `Saving` square with a planned money amount of `40$` changes the money amount shown inside `Savings` from `350$` to `310$` when the main money amount is `350$`.
- Creating a `Saving` square with a planned money amount does not change the main money amount.
- Creating a `Saving` square with a `0$` planned money amount is blocked.
- Changing `Rent` from `40$` to `60$` makes `Rent` show `60$`, not `100$`.
- Changing `Rent` from `40$` to `10$` makes `Rent` show `10$`, not `30$`.
- Changing `Rent` from `40$` to `0$` removes `Rent` from `Savings`.
- Changing a `Saving` square planned money amount to `0$` removes that square from browser storage.
- Changing a `Saving` square planned money amount does not create a `Balance Changes` entry.
- Deleting `Rent` removes only `Rent` and its saved details, does not change the main money amount, and leaves the other `Saving` squares unchanged.
- Deleting a `Saving` square asks for confirmation first with `Delete this Saving?`, shows `Cancel` and `Delete`, and does not offer undo.
- The add-a-saving action appears as a circle with a `+` sign.
- `Saving` squares show thin horizontal coverage bars at the bottom.
- With `85$` main money amount and ordered `Saving` squares of `Rent` `50$`, `Food` `20$`, and `School` `30$`, `Rent` and `Food` show full green bars, `School` shows the left 50% of its bottom bar as green and the right 50% as grey, `School` shows `15$ needed` at the top-left of its bottom coverage bar, and the money amount shown inside `Savings` is `0$`.
- Website reload keeps saved data.
- Layout works on mobile.
- Layout works on desktop.
- Wording does not imply real banking.

## Done Criteria

This plan can move to `docs/plans/done` when:
- The MVP features listed in scope are implemented.
- The main user flow works end to end.
- Add and subtract changes are saved in history as separate entries.
- Modify changes are saved only as the current money amount, not as history.
- `Saving` squares are usable.
- Mobile and desktop layouts have been checked.
- The trust wording has been reviewed.
- Any remaining known limitations are documented.
