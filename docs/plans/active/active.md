# Cash Money Organizer MVP Plan

Status: Active

Source specs:
- `docs/specs/accepted/GLOBAL-SPEC-002-manual-cash-tracker.md`
- `docs/specs/accepted/FEATURE-SPEC-001-adding-money.md`
- `docs/specs/accepted/FEATURE-SPEC-002-subtracting-money.md`
- `docs/specs/accepted/FEATURE-SPEC-003-modifying-the-money-amount.md`
- `docs/specs/accepted/FEATURE-SPEC-004-viewing-balance-changes.md`
- `docs/specs/accepted/FEATURE-SPEC-005-deleting-a-balance-change.md`
- `docs/specs/accepted/FEATURE-SPEC-006-viewing-savings.md`
- `docs/specs/accepted/FEATURE-SPEC-007-creating-a-saving.md`
- `docs/specs/accepted/FEATURE-SPEC-008-opening-saving-actions.md`
- `docs/specs/accepted/FEATURE-SPEC-009-renaming-a-saving.md`
- `docs/specs/accepted/FEATURE-SPEC-010-changing-the-planned-money-amount-of-a-saving.md`
- `docs/specs/accepted/FEATURE-SPEC-011-deleting-a-saving.md`
- `docs/specs/accepted/FEATURE-SPEC-012-reordering-saving-squares.md`
- `docs/specs/accepted/FEATURE-SPEC-013-viewing-savings-coverage.md`
- `docs/specs/accepted/FEATURE-SPEC-014-recovering-unreadable-saved-data.md`
- `docs/specs/accepted/FEATURE-SPEC-015-fixing-a-saving-that-could-not-be-loaded.md`
- `docs/specs/accepted/FEATURE-SPEC-016-handling-changes-that-cannot-be-saved.md`
- `docs/specs/accepted/FEATURE-SPEC-017-discarding-unfinished-actions-after-refresh-or-reopen.md`
- `docs/specs/accepted/FEATURE-SPEC-018-updating-other-open-tabs-and-windows.md`
- `docs/specs/accepted/FEATURE-SPEC-019-keeping-one-action-open-at-a-time.md`
- `docs/specs/accepted/FEATURE-SPEC-020-opening-main-money-actions.md`
- `docs/specs/accepted/FEATURE-SPEC-021-entering-main-money-amounts.md`
- `docs/specs/accepted/FEATURE-SPEC-022-viewing-the-dashboard.md`
- `docs/specs/accepted/TECHNICAL-SPEC-001-main-money-amount-entry-state.md`
- `docs/specs/accepted/TECHNICAL-SPEC-002-balance-changes-records-and-retention.md`
- `docs/specs/accepted/TECHNICAL-SPEC-003-saving-data-and-creation.md`
- `docs/specs/accepted/TECHNICAL-SPEC-004-savings-coverage-calculations.md`
- `docs/specs/accepted/TECHNICAL-SPEC-005-detecting-and-repairing-a-broken-saving.md`
- `docs/specs/accepted/TECHNICAL-SPEC-006-safe-browser-storage-writes-and-rollback.md`
- `docs/specs/accepted/TECHNICAL-SPEC-007-loading-validating-and-recovering-saved-data.md`

Other planned technical behavior is not a source of truth until it is covered by explicitly approved TechnicalSpecs.

## Goal

Build the first usable version of the Cash Money Organizer website: a browser-based personal cash tracking tool that feels calm and bank-like, while clearly communicating that it is not a real bank account.

The MVP should help a user:
- See their money amount with the user-facing label `Current Balance`.
- Start with the money amount shown as `0.00$` when no saved data exists.
- Click the main money amount at `0.00$` to show `Add` and `Modify`, but not `Subtract`.
- Add money manually while the money amount is less than `999,999.99$`, use `Subtract` after the money amount is greater than `0.00$`, and see all three actions when the money amount is greater than `0.00$` and less than `999,999.99$`.
- Use `Subtract` and `Modify`, without `Add`, when the money amount is exactly `999,999.99$`.
- Correct mistakes with `Modify` at any valid money amount from `0.00$` through `999,999.99$`.
- Review 30-day visible money amount change history from the dashboard.
- Keep saved money data after closing and reopening the website when saved data is available.
- Organize the `Savings` section into user-named `Saving` squares.
- See which `Saving` squares are fully covered, partly covered, or not covered.
- See what money amount is left inside `Savings` with the user-facing label `Savings money amount`.
- Review `Balance Changes`.

## MVP Scope

Included in the first version:
- Dashboard with the main money amount as the main focus.
- Default `0.00$` money amount when no saved data exists.
- Visible money amounts use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`, and use comma separators every three digits before the decimal point for values of `1,000.00$` or more, such as `5,895.50$` and `999,999.99$`.
- Money amount values can include cents, but the maximum allowed money amount is `999,999.99$`. The main money amount and each `Saving` square planned money amount should never be saved above `999,999.99$`.
- Clickable main money amount that shows `Add` and `Modify` as horizontal buttons at `0.00$`, shows `Add`, `Subtract`, and `Modify` as horizontal buttons when the money amount is greater than `0.00$` and less than `999,999.99$`, and shows `Subtract` and `Modify` without `Add` when the money amount is exactly `999,999.99$`.
- Browser storage for saved money amount, 30-day visible `Balance Changes` history, and `Saving` squares.
- Same-browser multiple tab or window updates, where a successful saved change in one tab or window automatically updates other open tabs or windows to the latest saved data.
- No normal user-controlled reset, clear-all-data, or start-fresh action in the first version; `Start again` appears only for broken or unreadable saved data.
- Silent discard of any open temporary UI after refresh, tab/window close, or later reopen, with only latest successfully saved data restored.
- No saved website settings in the first version because no settings exist yet.
- Add money flow with amount.
- Subtract money flow with amount.
- Silent modify flow with corrected total amount.
- 30-day visible money amount change history available in the dashboard's `Balance Changes` section, where each add and subtract action stays as its own entry, every current entry remains reachable, and the `Savings` action remains reachable whether history is empty or contains entries.
- Newest `Balance Changes` entries shown first.
- Full-screen `Savings` planning view with a money amount shown inside `Savings` using the label `Savings money amount`, a fixed top `Savings` area while `Saving` squares scroll, normal visual styling when `Savings money amount` is `0.00$`, small top `{money amount} needed` text when total planned money amount in valid `Saving` squares is greater than the main money amount, ordered user-named `Saving` squares in one vertical column on mobile and desktop, coverage bars, outside-click dismissal for `Saving` square action state, blank-area clicks inside an open action-state `Saving` square doing nothing, touch scrolling over `Saving` squares canceling tap and reorder when the finger moves more than `8px` before the `600ms` hold completes, `Saving` square holds for `600ms` without at least `8px` of movement doing nothing, fixed `Saving` reorder auto-scroll within `40px` of the top or bottom edge at `8px` per animation frame, interrupted `Saving` reorder drags canceling back to the original position, duplicate-name checks that include visible broken saved `Saving` squares with readable non-empty saved names, broken-square fix validation order matching new `Saving` square creation, a centered circle `+` empty state only when no normal or broken saved `Saving` squares are visible, a top-left circle `+` action when normal or broken saved `Saving` squares are visible, temporary `Saving` input squares for create, rename, planned-money-amount change, and broken-square fix, raw decimal typing for planned money amount inputs before `Save`, discarded unsaved `Saving` input drafts unless `Save` succeeds, locked broken saved `Saving` squares that cannot be reordered until fixed or deleted, saved-data-only `Savings money amount`, top needed text, and coverage updates while `Saving` input flows are open, a top-left `<` back action, and browser Back behavior that closes an open `Saving` input flow or delete confirmation first before closing full-screen `Savings`.
- `Balance Changes` history for added money and subtracted money, with newest-entry auto-scroll to the top after a successful `Add` or `Subtract`.
- Saved `Balance Changes` entry validation that removes only broken saved history entries while keeping the rest of the saved data.
- Delete support for `Balance Changes` entries that opens from `600ms` press-and-hold or click-and-hold, cancels the pending hold on early release, pointer or finger movement, or scrolling before `600ms`, shows one little `Delete` and `Cancel` square in the middle of the screen after a completed hold, closes that little square on `Cancel`, outside click, or browser Back without changing anything, keeps it open during pointer or finger movement and scrolling, asks for confirmation with `Delete this Balance Change?` after `Delete` is chosen, shows `Cancel` and `Delete` in the confirmation, closes the confirmation on `Cancel`, outside click, or browser Back without changing anything, removes only the visible history entry after the confirmation `Delete` is clicked, and does not offer undo.
- Responsive layout for mobile and desktop.
- Clear trust wording that this is a manual cash tracking tool, not a real bank, using the exact website name and short trust label `Manual Cash Tracker`.
- Browser storage save failure handling that blocks the valid action, keeps the last successfully saved visible state, shows `Changes could not be saved.`, and leaves retryable inputs or confirmations open.
- Global open temporary UI priority that allows only one temporary UI at a time and prevents one click, tap, or hold from opening a second temporary UI while another one is open.

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
- Normal user-controlled reset or clear-all-data action.

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
- The first version should support clickable controls through mouse clicks and finger taps.
- Keyboard use is required only for typing inside money amount inputs and `Saving` square inputs, including the specified `Space` key behavior inside main money amount inputs.
- The first version does not require custom keyboard navigation, custom `Tab` order, `Enter` or `Space` activation for clickable controls, focus-return rules after save/cancel/delete, or keyboard support for reordering `Saving` squares.

## Information Architecture

Primary areas:
- Dashboard
- Balance Changes
- Savings

Dashboard content:
- Exact website name and short trust label `Manual Cash Tracker`.
- Visually prominent main money amount with the user-facing label `Current Balance`; selecting it reveals only the actions available for its current value.
- A `Balance Changes` section that keeps every current entry reachable.
- The exact action `Savings`, which remains reachable whether `Balance Changes` is empty or contains entries.
- Component shapes, positions, sizes, and scrolling implementation remain design decisions.

## Core Data Model

Money amount:
- Money amount.
- Supports cents from `0.00$` through `999,999.99$` as each money amount rule allows.
- Store and calculate money amount values with an exact normalized plain decimal representation so typed values like `5 Space 5` or `5 Cent 5` that display as `5.05`, `58430 Space 88` or `58430 Cent 88` that display as `58,430.88`, and raw digit sequences like `589550` that display as `589,550.00`, keep the parsed money amount exact.
- Save money amount fields as normalized plain decimal strings with exactly two decimal digits, no `$` sign, no comma separators, and no unneeded leading zeros before the decimal point except the single `0` in values below `1.00`, such as `0.00`, `5.00`, `5895.50`, and `999999.99`.
- Render visible money amounts with the `$` sign and required comma separators, such as `5,895.50$` and `999,999.99$`.

Browser storage:
- Storage key: `cash-money-organizer-website-data`.
- Data version: `1` for the first saved data version.
- Load saved browser data only when the data version value is exactly `1`.
- Money amount data.
- 30-day visible `Balance Changes` entries.
- `Saving` squares.

The default `0.00$` starting state is displayed when no saved data exists. It should not write browser storage just because the website opened.

Browser storage should be created after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The first version should not save website settings because no settings exist yet. Add saved settings later only after a real setting is named and specified.

`Balance Changes` entry:
- ID.
- Type: added or subtracted.
- Amount.
- Previous money amount.
- New money amount.
- Difference.
- Created date and created time shown in the visible row using the format `July 21, 2026 at 3:45 PM` for display.
- Internal exact created date and time with seconds and milliseconds for ordering and 30-day clearing.
- Internal visible until date and time.

Broken saved `Balance Changes` entry:
- Removed from loaded visible history and saved history during load cleanup.
- Does not cause the full saved-data error when the rest of the saved browser data can still be read.
- Does not change the current money amount.
- Does not affect `Savings`.
- Does not show a broken history row or user-facing message.

`Saving` square:
- Unique ID created with `crypto.randomUUID()`.
- Name saved after surrounding spaces are removed.
- Planned money amount saved as an exact normalized decimal string greater than `0.00` and not greater than `999999.99`.
- Unique non-negative safe-integer order.
- No created date or updated date fields in the first version.
- No saved coverage, needed money amount, main money amount, or temporary creation input.

Broken saved `Saving` square:
- Exact text: `Saving could not be loaded.`
- Actions: `Fix` and `Delete`.
- Counts as visible content for circle `+` placement only.
- Does not use money amount inside `Savings`.
- Does not show a coverage bar.
- Does not affect coverage calculations until fixed.

## Milestones

### Milestone 1: Project Foundation

Tasks:
- Choose the website stack and folder structure.
- Create the main application shell.
- Add responsive layout foundations.
- Add browser storage persistence.
- Use the stable storage key `cash-money-organizer-website-data` for website data.
- Add data version `1` for first-version saved data.
- Define shared data types and exact money amount helpers for money amount values, `Balance Changes` entries, and `Saving` squares.
- Define broken saved `Balance Changes` entry handling so every broken history entry is removed independently without removing usable entries or breaking the whole saved browser data file.
- Define broken saved `Saving` square handling so one broken square does not break the whole saved browser data file.
- Define global open temporary UI priority for main money actions, main money amount inputs, `Balance Changes` delete UI, `Saving` square action state, `Saving` input flows, `Saving` delete confirmations, and active `Saving` reorder drag.
- Define same-browser multiple tab or window storage updates so other open tabs or windows update after one tab or window successfully saves a change.
- Keep `Start again` only for broken or unreadable saved data, with no normal all-data reset action in the first version.
- Define refresh, tab/window close, and later reopen behavior for temporary UI so open temporary UI is silently discarded and only latest successfully saved data returns.

Acceptance criteria:
- The website opens in a browser.
- The layout works on desktop and mobile widths.
- Data can be saved in browser storage and restored after refresh.
- Data can be restored after closing and reopening the website in the same browser.
- While the website checks browser storage after opening, show neither the dashboard nor recovery and do not briefly show the default `0.00$` dashboard.
- If no saved data exists, the website shows the dashboard with the money amount set to `0.00$`.
- Showing the default `0.00$` money amount does not create saved browser data by itself.
- Saved browser data with a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, shows `Saved data could not be loaded.` and a `Start again` action.
- If saved browser data has one or more broken `Balance Changes` entries but the rest of the data can be read, the website removes every broken entry independently and keeps every usable entry.
- The first view does not explain where saved data is stored.
- Only one temporary UI can be open at a time, and a click, tap, or hold cannot open a second temporary UI while another one is open.
- If the same website is open in multiple tabs or windows in the same browser, a successful saved change in one tab or window updates the other open tabs or windows to the latest saved data.
- Valid saved data does not show `Start again`, `Reset`, `Clear all data`, or any normal all-data reset action.
- Refreshing, closing, or reopening the website while temporary UI is open restores only the latest successfully saved data and does not restore the temporary UI.

### Milestone 2: Cash Dashboard

Tasks:
- Build the dashboard screen.
- Display the main money amount prominently with the label `Current Balance`.
- Make the main money amount clickable.
- Show `Add` and `Modify`, but not `Subtract`, when the user clicks the main money amount at `0.00$`.
- Show `Add`, `Subtract`, and `Modify` when the user clicks the main money amount above `0.00$` and below `999,999.99$`.
- Show `Subtract` and `Modify`, but not `Add`, when the user clicks the main money amount at exactly `999,999.99$`.
- Render visible main money actions as buttons lined together horizontally near the main money amount.
- Order the two visible main money action buttons at `0.00$` from left to right as `Add`, then `Modify`.
- Order the three visible main money action buttons from left to right as `Add`, `Subtract`, and `Modify`.
- Order the two visible main money action buttons at `999,999.99$` from left to right as `Subtract`, then `Modify`.
- Hide the main money action buttons when the user clicks or taps the main money amount again, without changing anything, saving anything, creating a `Balance Changes` entry, or showing a message.
- Hide the main money action buttons when the user clicks or taps outside the main money amount and outside the visible action buttons, without changing anything, saving anything, creating a `Balance Changes` entry, or showing a message.
- Hide the main money action buttons when the user scrolls the dashboard page, without changing anything, saving anything, creating a `Balance Changes` entry, or showing a message.
- Start the selected money amount input flow when the user clicks `Add`, `Subtract`, or `Modify`; these action-button clicks should not be treated as outside clicks.
- Hide the visible main money action buttons when the selected money amount input flow starts, remember the selected action internally only, and do not show a visible `Add`, `Subtract`, or `Modify` reminder inside the open input flow.
- Include a `Balance Changes` section on the dashboard and keep every current entry reachable.
- Keep the exact `Savings` action reachable whether `Balance Changes` is empty or contains entries.
- Add clear manual-tracker wording.
- Keep `Balance Changes` empty with no empty-state content when no history entries exist, and use a centered circle `+` as the empty state when no `Saving` squares exist.

Acceptance criteria:
- The main money amount is the most visible item on the first screen.
- Clicking the main money amount at `0.00$` reveals `Add` and `Modify`, but not `Subtract`.
- Clicking the main money amount above `0.00$` and below `999,999.99$` reveals `Add`, `Subtract`, and `Modify`.
- Clicking the main money amount at exactly `999,999.99$` reveals `Subtract` and `Modify`, but not `Add`.
- The visible money actions are visually connected to the main money amount.
- At `0.00$`, the visible money actions are horizontal buttons, with the two-button order `Add`, then `Modify`.
- The visible money actions are horizontal buttons, with the three-button order `Add`, `Subtract`, and `Modify`.
- At `999,999.99$`, the visible money actions are horizontal buttons, with the two-button order `Subtract`, then `Modify`.
- Clicking or tapping the main money amount again while the visible money actions are open hides the visible money actions without changing anything.
- Clicking or tapping outside the main money amount and outside the visible action buttons hides the visible money actions without changing anything.
- Scrolling the dashboard page while the visible money actions are open hides the visible money actions without changing anything.
- Clicking `Add`, `Subtract`, or `Modify` starts the selected money amount input flow instead of hiding the visible money actions as an outside click.
- When the selected money amount input flow starts, the visible main money action buttons are hidden and the open input flow does not show a visible `Add`, `Subtract`, or `Modify` reminder.
- `Balance Changes` is available from the dashboard and keeps every current entry reachable.
- The `Savings` action remains reachable whether `Balance Changes` is empty or contains entries.
- There is no separate `View history` action for money amount change history.
- The interface does not use misleading bank wording.

### Milestone 3: Manual Money Amount Changes

Tasks:
- Build the default `0.00$` starting state.
- Build add money flow.
- Build subtract money flow.
- Build silent modify/correct money amount flow.
- Validate amount inputs.
- Use the shared money amount entry flow for `Add`, `Subtract`, and `Modify` without requiring a particular shape, position, size, or visual arrangement.
- Keep other dashboard controls inactive while a main money amount input flow is open.
- Treat the input flow as the active interaction context. Clicking or tapping outside it should act like `Cancel`: close the flow, return to the dashboard money amount view, change nothing, save nothing, create no `Balance Changes` entry, and show no message. That interaction should not reopen the main money action buttons.
- Start the money amount input at `0.00`.
- Focus the money amount input immediately when it opens so the user can start typing without another tap or click, and request the mobile keyboard immediately on supported mobile devices.
- Request a mobile keyboard suitable for digit entry for main money action inputs on supported devices.
- Provide an action with the exact visible name `Cent` for all users while a main money action input is open. Choosing it keeps the input active, and the mobile digit-entry keyboard remains available when the browser permits it.
- Let the user type numbers from `0` through `9` into main money action inputs without typing the `$` sign. Let `Cent`, with `Space` as its keyboard alternative, start cents entry. Block typed decimal points and manually typed comma separators with no message. Add the decimal point and comma separators automatically in the displayed input.
- Accept `Cent` or `Space` only after at least one entered digit and only once per input. The initial `0.00` does not count as an entered digit. Ignore `Cent` or `Space` before a digit or after cents entry has already started, with no message. Visibly indicate when cents entry is active without requiring a particular visual treatment.
- Automatically format typed main money action input values with two digits after the decimal point, automatic comma separators for thousands and larger values, and no `$` sign while the user is typing. Before cents entry starts, typed digits are whole money amount digits: typing `5` shows `5.00`, `58` shows `58.00`, `589` shows `589.00`, `5895` shows `5,895.00`, `58955` shows `58,955.00`, and `589550` shows `589,550.00`.
- After `Cent` or `Space` starts cents entry, treat the next one or two digits as cents, left-pad one cents digit with `0`, and block a third cents digit with no message. Keep all earlier digits as the whole money amount and add comma separators to that whole money amount automatically.
- Treat `0` as a normal digit, not a starting zero-position skip. Normalize away unneeded leading zeros in the whole money amount, so typing `0005` shows `5.00`.
- Reformat main money action inputs after deletion with two digits after the decimal point and no `$` sign while the input is still open.
- Return main money action inputs to `0.00` when all typed numbers are deleted, without letting the input become empty.
- Make main money action inputs append-only: focus can open the input, but cursor movement inside the formatted money amount, partial selection, selection replacement, and direct editing of generated comma separators or the generated decimal point are not supported. Accepted typing adds to the end, and delete removes only the last accepted digit or the action that started cents entry.
- Block letters, minus signs, decimal points, `$` signs, manually typed comma separators, `Cent` or `Space` before any entered digit, another `Cent` or `Space` after cents entry starts, a third cents digit, and paste in main money action inputs with no message.
- Block typed characters that would make a main money action input greater than `999,999.99`, with no message and no field change.
- Keep money amounts up to `999,999.99$` and every required control readable and usable in the input flow, dashboard, `Balance Changes`, and `Savings` without rejecting valid values or omitting required comma separators.
- Show the exact text `Save Changes` and provide actions exactly named `Cent`, `Yes`, and `Cancel` without requiring a particular visual arrangement.
- Make `Yes` apply the selected `Add`, `Subtract`, or `Modify` action.
- After a successful `Add`, `Subtract`, or `Modify` that changes the money amount, close the input flow, reset the temporary typed input so the next main money amount input starts at `0.00`, return to the dashboard money amount view, hide the main money action buttons, show the updated money amount, save only the data required by that action, and show no message.
- After a failed save, keep the input flow and entered amount, and show `Changes could not be saved.` while the failed flow remains unchanged.
- Remove the previous save-failure message when the user changes the amount, chooses `Yes` to retry, or closes the flow. Show it again if the retry also fails, and do not carry it into a later new flow.
- Make `Cancel` and a click or tap outside the active entry flow close the main money amount input flow, return to the dashboard money amount view, and change nothing.
- Make the browser Back button, mobile browser back gesture, and system Back action act like `Cancel` while an `Add`, `Subtract`, or `Modify` money amount input flow is open, without changing anything, saving anything, creating a `Balance Changes` entry, or showing a message.
- Discard unsaved typed input from an open `Add`, `Subtract`, or `Modify` money amount input flow after page refresh, browser tab or window close, or later website reopen. Restore only the last successfully saved data, without saving the unsaved typed value, creating a `Balance Changes` entry, or showing a message or browser leave warning.
- Save each `Add` and `Subtract` action to `Balance Changes` as its own separate entry.
- Do not combine separate `Add` and `Subtract` actions into one net history result.
- Do not save `Modify` actions to `Balance Changes`.
- Save each successful money change to browser storage.
- Recalculate the current money amount after each action.

Acceptance criteria:
- The default `0.00$` starting state does not create a `Balance Changes` entry.
- At `0.00$`, the main money amount shows `Add` and `Modify`, but not `Subtract`, when clicked.
- Adding money increases the money amount and creates a positive `Balance Changes` entry.
- After adding money above `0.00$` and below `999,999.99$`, the main money amount shows `Add`, `Subtract`, and `Modify` when clicked.
- If a successful `Add` makes the money amount exactly `999,999.99$`, the main money amount shows `Subtract` and `Modify`, but not `Add`, when clicked.
- Subtracting money decreases the money amount and creates a negative `Balance Changes` entry.
- Subtracting more than the current money amount sets the money amount to `0.00$` instead of creating a negative money amount.
- Subtracting when the current money amount is `0.00$` keeps the money amount at `0.00$` and does not create a history entry.
- Modifying the money amount replaces the current money amount without creating history or notification entries only when the entered money amount is different from the money amount already shown.
- Separate `Add` and `Subtract` actions stay separate in history.
- Money changes are still visible after page refresh.
- `Add`, `Subtract`, and `Modify` use the same money amount entry flow without requiring a particular shape, position, size, or visual arrangement.
- While the flow is open, it is the active interaction context and other dashboard controls are inactive.
- The money amount input starts at `0.00`.
- The money amount input is focused and ready for typing immediately when it opens.
- `Add`, `Subtract`, and `Modify` money amount input flows provide an action with the exact visible name `Cent` on mobile and desktop.
- Choosing `Cent` keeps the input active, and the mobile digit-entry keyboard remains available when the browser permits it.
- Typing `5` in a main money action input shows `5.00`, typing `58` shows `58.00`, typing `589` shows `589.00`, typing `5895` shows `5,895.00`, typing `58955` shows `58,955.00`, and typing raw digits `589550` shows `589,550.00`.
- Typing `5`, then `Space`, then `5` shows `5.05`; typing `5`, then `Space`, then `50` shows `5.50`; typing `58430`, then `Space`, then `88` shows `58,430.88`; and typing `999999`, then `Space`, then `99` shows `999,999.99`.
- Typing `5`, choosing `Cent`, then typing `5` shows `5.05`; typing `5`, choosing `Cent`, then typing `50` shows `5.50`; and typing `58430`, choosing `Cent`, then typing `88` shows `58,430.88`.
- Typing `0005` in a main money action input shows `5.00`.
- Typing `Space`, `Space`, `Space`, then `5` in a main money action input blocks the three `Space` key presses and then shows `5.00` after the `5` is typed.
- Saving a main money action input stores the money amount as a normalized plain decimal string without the `$` sign or comma separators, such as `5.00`, `5.05`, `5895.50`, or `999999.99`, and shows the money amount with the `$` sign at the end and required comma separators for thousands and larger values, such as `5.00$`, `5.05$`, `5,895.50$`, or `999,999.99$`.
- Deleting one typed character from a main money action input reformats the remaining typed value as a money amount.
- Deleting all typed numbers in a main money action input returns the input to `0.00` instead of making it empty.
- Clicking or tapping inside a main money action input does not move the cursor into the middle of the formatted money amount.
- Selecting part of a main money action input and typing does not replace the selected text; accepted typing is added to the end.
- Backspace or Delete in a main money action input removes only the last accepted digit or the action that started cents entry, not generated comma separators or the generated decimal point.
- Main money action inputs keep their previous value when the user types letters, minus signs, decimal points, `$` signs, manually typed comma separators, chooses `Cent` or presses `Space` before entering a digit, chooses `Cent` or presses `Space` again after cents entry starts, enters a third cents digit, or enters another blocked character.
- Choosing `Cent` when cents entry cannot start keeps the previous input value and shows no message.
- Main money action inputs accept valid money amounts up to `999,999.99` and block typed characters that would make the value greater than `999,999.99`.
- Money amounts up to `999,999.99$` and all required controls remain readable and usable with required comma separators while typing or in saved/rendered visible values.
- If the user tries to paste letters, numbers, symbols, or any other content into a main money action input, the pasted content does not appear, the input keeps its previous value, and no message is shown.
- `Add`, `Subtract`, and `Modify` money amount input flows show the exact text `Save Changes` and provide actions exactly named `Cent`, `Yes`, and `Cancel` without requiring a particular visual arrangement.
- `Cancel` in `Add`, `Subtract`, or `Modify` closes the money amount input flow, returns to the dashboard money amount view, changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
- Clicking or tapping outside the active money amount entry flow acts like `Cancel` and does not reopen the main money actions.
- Using browser Back while an `Add`, `Subtract`, or `Modify` money amount input flow is open acts like `Cancel`, changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
- Refreshing, closing, or reopening the website while an `Add`, `Subtract`, or `Modify` money amount input flow has unsaved typed input discards the open input flow and typed value, restores only the last successfully saved data, creates no `Balance Changes` entry, and shows no message or browser leave warning.
- Choosing `Cent` after at least one entered digit starts cents entry, behaves like pressing `Space`, keeps the input active, visibly indicates that cents entry is active, and does not cancel or close the flow.
- `Add`, `Subtract`, and `Modify` do not save invalid money amounts. Negative money amounts and above-limit money amounts are blocked, `Add` and `Subtract` require more than `0.00$` and not greater than `999,999.99$`, and `Modify` allows `0.00$` through `999,999.99$`.
- Clicking `Yes` while the input is `0.00` in `Add` or `Subtract` does nothing: no message, no money amount change, no saved data change, no `Balance Changes` entry, and the same money amount input step stays open.
- Clicking `Yes` in `Modify` with the same money amount that is already shown does nothing: no message, no money amount change, no saved data change, no browser storage creation or update, no `Balance Changes` entry, no `Balance Changes` cleanup, and the same money amount input step stays open until the user enters a different valid money amount or cancels.
- Clicking `Yes` while the input is `0.00` in `Modify` replaces the main money amount with `0.00$` only when the current money amount is greater than `0.00$`.
- Modifying the money amount to `0.00$` makes the next main money amount click show `Add` and `Modify`, but not `Subtract`.
- Modifying the money amount to `999,999.99$` makes the next main money amount click show `Subtract` and `Modify`, but not `Add`.

### Milestone 4: Balance Changes

Tasks:
- Build the `Balance Changes` section on the dashboard.
- Keep every current `Balance Changes` entry reachable when the history is long.
- Keep the `Savings` action reachable whether `Balance Changes` is empty or contains entries.
- Leave the section's shape, size, position, and scrolling implementation as design decisions that work on mobile and desktop.
- Do not set a smaller maximum visible-entry limit for valid 30-day `Balance Changes` entries.
- When no `Balance Changes` entries exist, show no history rows and no empty-state sentence, placeholder, icon, or other empty-state content.
- Show each visible `Balance Changes` row with the signed money amount, action text, and both the created date and created time: `+{money amount} added` or `-{money amount} subtracted`. Use the fixed English date-and-time format shown by `September 18, 2026 at 2:30 PM`; a date-only display is not enough.
- Render each visible `Balance Changes` entry as a compact entry box, not a large panel.
- Place the signed money amount and action text in the top-left corner of each visible `Balance Changes` entry.
- Place the visible created date and created time in the bottom-right corner of the same entry, below the money change text and not too far from it.
- Keep the same `Balance Changes` entry layout on mobile. Text may wrap only as needed to stay readable, but the money change text should remain in the top-left area and the visible date and time should remain in the bottom-right area.
- Record the user's local date and time when the action succeeds, then always show it with the fixed English format.
- Save an internal exact created date and time with seconds and milliseconds for each `Balance Changes` entry, but do not show seconds or milliseconds to the user.
- Do not show the previous money amount, new money amount, internal exact created date and time, or internal visible until date and time in visible `Balance Changes` rows.
- Keep each add and subtract action as its own visible entry.
- Show newest `Balance Changes` entries first by internal exact created date and time, with older changes lower in the list.
- If two `Balance Changes` entries have the exact same internal exact created date and time, use saved list order as the tie-breaker, with entries earlier in the saved list appearing first.
- Move the `Balance Changes` scroll position to the top after a successful `Add` or `Subtract` creates a new `Balance Changes` entry, even if the user had previously scrolled lower in the history list.
- Also move a receiving tab or window's `Balance Changes` scroll position to the top when a new saved `Add` or `Subtract` entry arrives from another open copy.
- Do not ask the user to choose a date for `Add` or `Subtract`.
- Do not replace separate entries with only a combined net result.
- Calculate each `Balance Changes` internal visible-until timestamp as its exact created timestamp plus `2_592_000_000` milliseconds, which is exactly 720 hours.
- Run `Balance Changes` cleanup when the website opens and loads saved data.
- Run `Balance Changes` cleanup after every successful saved user action.
- During cleanup, delete visible history entries from browser storage at or after their visible until date.
- While the website stays open, schedule a view check for the next expiration and recheck after the page becomes visible or its window gains focus.
- Keep the current money amount unchanged when old history entries expire.
- Keep every current entry reachable when the `Balance Changes` history is longer than the available view.
- Add delete support for `Balance Changes` entries.
- For touch users, open the delete action when the user presses and holds a `Balance Changes` entry for `600ms`.
- For mouse users, open the delete action when the user clicks and holds a `Balance Changes` entry for `600ms`.
- Cancel a pending `Balance Changes` delete hold before `600ms` if the user releases the press or click, moves the pointer or finger, or starts scrolling. Do not open the little square, change anything, save anything, delete anything, create a `Balance Changes` entry, or show a message.
- After a completed `600ms` press-and-hold or click-and-hold, show a little square in the middle of the screen with the exact action texts `Delete` and `Cancel`.
- Allow only one `Balance Changes` delete action square to be open at a time.
- Make clicking `Cancel` in the little square close it without changing anything.
- Make clicking or tapping outside the little square close it without changing anything.
- Make browser Back, mobile browser back gesture, or system Back close the little `Delete` and `Cancel` square while keeping the user on the dashboard, changing nothing, saving nothing, deleting nothing, creating no `Balance Changes` entry, and showing no message.
- Keep the little square open during pointer or finger movement and scrolling.
- Make clicking `Delete` in the little square close it, then ask for confirmation with `Delete this Balance Change?` and show `Cancel` and `Delete`.
- Make clicking `Cancel` or clicking/tapping outside the `Delete this Balance Change?` confirmation close the confirmation, keep the `Balance Changes` entry visible, change nothing, save nothing, and show no message.
- Make browser Back, mobile browser back gesture, or system Back close the `Delete this Balance Change?` confirmation while keeping the user on the dashboard, keeping the `Balance Changes` entry visible, changing nothing, saving nothing, deleting nothing, creating no `Balance Changes` entry, and showing no message.
- Make clicking `Delete` in the `Delete this Balance Change?` confirmation remove only the visible history entry, not change the current money amount, and not offer undo.

Acceptance criteria:
- The user can understand how their money amount changed over time.
- `Balance Changes` is available on the dashboard without requiring a particular shape, size, position, or scrolling implementation.
- When there are no `Balance Changes` entries, the history list is empty and shows no sentence, placeholder, icon, or other empty-state content.
- The user sees separate entries such as `+56.00$ added` and `-34.00$ subtracted`.
- Visible `Balance Changes` rows show both the created date and created time using the format `July 21, 2026 at 3:45 PM`.
- Visible `Balance Changes` entries are compact, with the money change text at the top left and the visible date and time at the bottom right.
- Mobile keeps the same `Balance Changes` entry layout, with readable wrapping if needed.
- Visible `Balance Changes` dates and times use the user's browser/device local date and time.
- Visible `Balance Changes` rows do not show previous money amount, new money amount, internal exact created date and time, seconds, milliseconds, or internal visible until date and time.
- The newest `Balance Changes` entry appears first by internal exact created date and time, and older changes go lower.
- If two `Balance Changes` entries have the exact same internal exact created date and time, saved list order decides which one appears first.
- After a successful `Add` or `Subtract` creates a new `Balance Changes` entry, the `Balance Changes` scroll position moves to the top so the newest entry is visible.
- `Balance Changes` does not show only a combined result such as `+22.00$ net change`.
- `Balance Changes` cleanup runs when the website opens and loads saved data.
- `Balance Changes` cleanup runs after every successful saved user action.
- During cleanup, entries at or after their visible until date are deleted from browser storage and no longer shown.
- Removing old history entries does not change the current money amount.
- Every valid 30-day `Balance Changes` entry remains reachable when the history is long, with no smaller maximum visible-entry limit.
- The `Savings` action remains reachable whether `Balance Changes` is empty or contains entries.
- Touch users can open a `Balance Changes` entry delete action by pressing and holding the entry for `600ms`.
- Mouse users can open a `Balance Changes` entry delete action by clicking and holding the entry for `600ms`.
- Releasing before `600ms`, moving the pointer or finger before `600ms`, or starting to scroll before `600ms` cancels the pending `Balance Changes` delete hold and does not open the little square.
- A completed `600ms` press-and-hold or click-and-hold shows a little square in the middle of the screen with the exact action texts `Delete` and `Cancel`.
- Only one `Balance Changes` delete action square can be open at a time.
- Clicking `Cancel` in the little square closes it without changing anything.
- Clicking outside the little square closes it without changing anything.
- Browser Back while the little `Delete` and `Cancel` square is open closes that little square, keeps the user on the dashboard, changes nothing, saves nothing, deletes nothing, creates no `Balance Changes` entry, and shows no message.
- Pointer or finger movement does not close the little square after it is open.
- Scrolling does not close the little square after it is open.
- Deleting an entry asks for confirmation after `Delete` is clicked with `Delete this Balance Change?`, closes the little square, and shows `Cancel` and `Delete`.
- Clicking `Cancel` or clicking/tapping outside the `Delete this Balance Change?` confirmation closes the confirmation, keeps the `Balance Changes` entry visible, changes nothing, saves nothing, and shows no message.
- Browser Back while the `Delete this Balance Change?` confirmation is open closes that confirmation, keeps the user on the dashboard, keeps the `Balance Changes` entry visible, changes nothing, saves nothing, deletes nothing, creates no `Balance Changes` entry, and shows no message.
- Clicking `Delete` in the `Delete this Balance Change?` confirmation removes only the visible history entry, does not change the current money amount, and does not offer undo.
- Saved `Balance Changes` entries cannot be edited.
- Recent money amount change history is visible from the dashboard without a separate `View history` action.

### Milestone 5: Savings Section and Saving Squares

Tasks:
- Build the full-screen `Savings` view.
- Treat `Savings` as the separate planning section.
- Treat a `Saving` as one user-named square inside the `Savings` section.
- Let the user open the Savings section by clicking `Savings`.
- Do not add a separate `Open savings` action.
- Make `Savings` open as a full-screen view.
- Show a small `<` sign in the top-left corner of the full-screen `Savings` view.
- Make clicking `<` return to the dashboard without changing anything, saving anything, or creating a `Balance Changes` entry when no `Saving` square input flow or `Saving` square delete confirmation is open.
- Make browser Back, mobile browser back gesture, or system Back close the full-screen `Savings` view and return to the dashboard without changing anything, saving anything, creating a `Balance Changes` entry, or showing a message when no `Saving` square input flow or `Saving` square delete confirmation is open.
- Make clicking `<` or using browser Back, mobile browser back gesture, or system Back close an open `Saving` square create, rename, planned-money-amount change, or broken-square fix input flow first, act like `Cancel`, keep the user inside `Savings`, discard unsaved typed input, restore the `+`, normal square, or broken square that the input replaced, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- Make clicking `<` or using browser Back, mobile browser back gesture, or system Back close an open `Saving` square delete confirmation first, keep the user inside `Savings`, delete nothing, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- After the open `Saving` square input flow or delete confirmation is closed, make the next `<` click or Back action close full-screen `Savings` when no other `Saving` square input flow or delete confirmation is open.
- In the default `Saving` square state, show the `Saving` square name in the top-left corner, the planned money amount on the right side of the same top row, and the thin coverage bar at the bottom.
- Make clicking a `Saving` square change that same square into its action state.
- In the `Saving` square action state, keep the `Saving` name at the top left and planned money amount on the right side of the same top row, show `Delete` at the bottom center, and hide the thin coverage bar.
- In the action state, make clicking the `Saving` square name open rename.
- In the action state, make clicking the planned money amount open planned-money-amount change.
- In the action state, make clicking `Delete` start delete confirmation.
- Do not use a separate action menu or larger action square for rename, planned-money-amount change, and delete.
- Render the `Saving` square delete confirmation as a small confirmation square in the middle of the screen with exact message `Delete this Saving?` and buttons `Cancel` and `Delete`.
- Do not render the `Saving` square delete confirmation inside the `Saving` square, as a temporary square state, as a bottom sheet, or as a separate page.
- Allow only one `Saving` square delete confirmation to be open at a time.
- Make `Cancel` or outside click close the `Saving` square delete confirmation without changing anything, saving anything, creating a `Balance Changes` entry, or showing a message.
- Make clicking `Delete` in the small confirmation square close the confirmation and delete only the selected `Saving` square.
- Make rename replace that same `Saving` square with a temporary `Saving` input square in the same visible position, asking only for the new `Saving` name.
- Make planned-money-amount change replace that same `Saving` square with a temporary `Saving` input square in the same visible position, asking only for the new total planned money amount.
- Do not render rename or planned-money-amount change as a modal, bottom sheet, separate page, or separate floating input.
- Close an open `Saving` square action state when the user clicks or taps outside that same square, without saving, creating a `Balance Changes` entry, or showing a message.
- Do not treat clicks or taps inside the open action-state square as outside clicks.
- Make clicking or tapping a blank part of the same open action-state square do nothing: keep the square in action state, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- For touch users, treat finger movement greater than `8px` before finger release and before the `600ms` reorder hold completes as scrolling, not as a tap.
- When touch scrolling over a `Saving` square is detected before `600ms`, cancel the pending tap and pending reorder hold without opening the action state, starting reorder, showing a drag placeholder, changing anything, saving anything, creating a `Balance Changes` entry, or showing a message.
- Let a touch release before `600ms` with movement of `8px` or less count as a tap that opens the action state.
- If the user clicks another normal default `Saving` square while one square is in action state, close the old action state and do not open the clicked square in its own action state from that same click.
- Keep an open `Saving` square action state open during scrolling by itself.
- Clear any open `Saving` square action state when the user clicks the small `<` sign to return to the dashboard.
- Let touch users reorder by holding a normal default `Saving` square for `600ms`, then moving the whole square.
- Let mouse users reorder by clicking and holding a normal default `Saving` square for `600ms`, then dragging the whole square.
- Keep broken saved `Saving` squares locked in their current displayed positions until fixed or deleted.
- Do not let holding, clicking and holding, moving, or dragging a broken saved `Saving` square start reorder, show a drag placeholder, change anything, save anything, create a `Balance Changes` entry, or show a message.
- Let normal default `Saving` squares still be reordered while broken saved `Saving` squares are visible, as long as no `Saving` square is in action state, input state, or delete confirmation state.
- Start the reorder drag only after the completed `600ms` hold and at least `8px` of pointer or finger movement.
- If a touch or mouse user holds a normal default `Saving` square for `600ms` but releases before moving at least `8px`, do nothing: no action state, no reorder, no drag placeholder, no saved change, no `Balance Changes` entry, and no message.
- Make holding or dragging a `Saving` square avoid opening rename, planned-money-amount change, or delete.
- Make the dragged `Saving` square follow the user's finger or mouse pointer.
- Show a placeholder the same size as the dragged square in the old position.
- Start `Saving` square reorder auto-scroll when the user's finger or mouse pointer is within `40px` of the top or bottom edge of the scrollable `Saving` squares area.
- Use a fixed auto-scroll speed of `8px` per animation frame.
- Use the same trigger distance and fixed speed for top and bottom auto-scroll.
- Do not change auto-scroll speed based on how close the pointer or finger is to the edge.
- Cancel a `Saving` square reorder if the user's finger or mouse pointer leaves the screen, the browser window loses focus, or the drag is interrupted before the square is dropped.
- After an interrupted `Saving` square reorder, return the square to its original position, remove the drag placeholder, stop any reorder auto-scroll, save nothing, create no `Balance Changes` entry, and show no message.
- Disable reordering while any `Saving` square is in action state, input state, or delete confirmation state.
- Show the money amount inside `Savings` at the top with the user-facing label `Savings money amount`.
- Keep the top `Savings` area with the small `<` sign, `Savings money amount`, and optional top needed text fixed while the user scrolls through many `Saving` squares.
- Make only the `Saving` squares area scroll below the fixed top `Savings` area, without hiding square content.
- Keep `Savings money amount` styled the same at `0.00$` as it is for nonzero values, with no warning or error style only because it is `0.00$`.
- Show small top `{money amount} needed` text directly under `Savings money amount` only when total planned money amount in valid `Saving` squares is greater than the main money amount.
- Calculate the top needed text as total planned money amount in valid `Saving` squares minus the main money amount.
- Do not show top needed text when total planned money amount in valid `Saving` squares is equal to or less than the main money amount.
- Start the money amount inside `Savings` from the current money amount.
- Calculate the money amount inside `Savings` as current money amount minus total planned money amount in `Saving` squares, stopped at `0.00$`.
- Keep the `Savings money amount`, top needed text, and coverage bars based on saved `Saving` squares while `Saving` square create, rename, planned-money-amount change, or broken-square fix input flows are open.
- Recalculate the `Savings money amount`, top needed text, and coverage bars only after a successful `Save`; `Cancel` leaves them unchanged.
- Render visible `Saving` squares in one vertical column on mobile and desktop.
- Do not render `Saving` squares in multiple columns or a grid.
- Calculate `Saving` square coverage from top to bottom using the main money amount.
- Treat the visible order of normal `Saving` squares as the coverage order, skipping broken saved `Saving` squares.
- Derive coverage, each needed amount, `Savings money amount`, and the overall amount needed with exact integer `bigint` cents in normal-square order without floating-point arithmetic or rounding.
- Keep derived `bigint` coverage values only in memory and rebuild them from validated saved decimal strings whenever coverage is recalculated.
- Add thin horizontal coverage bars at the very bottom of `Saving` squares in their default state.
- Show full green bars for fully covered `Saving` squares, left-to-right partly green bars for partly covered squares, and grey bars for uncovered squares.
- Show a `{money amount} needed` note at the top-left of the bottom coverage bar when a `Saving` square is not fully covered.
- Build the add-a-saving action as a circle with a `+` sign.
- When no normal `Saving` squares and no broken saved `Saving` squares are visible, show the circle `+` action in the middle of the `Saving` squares area as the empty state.
- Do not show a separate empty-state text sentence for no `Saving` squares.
- After at least one normal `Saving` square exists, place the circle `+` action at the top-left of the `Saving` squares area, above or before the visible squares.
- If only broken saved `Saving` squares are visible, still place the circle `+` action at the top-left of the `Saving` squares area, above or before the broken saved `Saving` squares.
- Make the circle `+` action open the same new `Saving` square create flow from both positions.
- Make the create flow replace the clicked circle `+` with a temporary `Saving` input square inside the `Saving` squares area.
- If the clicked `+` was centered, render the temporary create input square centered in the `Saving` squares area.
- If the clicked `+` was at the top-left, render the temporary create input square at the top-left before the existing squares.
- Do not render the create flow as a modal, bottom sheet, or separate page.
- Do not render a second create `+` action while the temporary create input square is open.
- Require the user to enter a `Saving` name and planned money amount before creating the `Saving` square.
- Show accepted `Saving` square planned money amount input as raw decimal number text while typing, with no `$` sign, comma separators, or automatic two-decimal formatting before `Save`.
- Do not show a `Cent` button in `Saving` square planned money amount inputs, do not apply the main money amount `Space` key cents behavior there, and block `Space` with no message.
- Treat `Saving` square planned money amount input without a decimal point as a whole money amount, and input with one decimal point as cents after the decimal point.
- After `Save`, normalize and render `Saving` square planned money amounts with two decimal digits, comma separators when needed, and the `$` sign.
- After a successful creation, append the new `Saving` after every existing `Saving`, keep every existing relative position unchanged, and give the new `Saving` the last coverage priority until it is reordered.
- Give the first saved `Saving` order `0`; otherwise, give a new `Saving` one more than the highest usable existing non-negative safe-integer order. If no next safe order is available, save nothing and handle the attempt as a save failure.
- Require each `Saving` square name to be unique inside `Savings`.
- Allow `Saving` square names with no maximum length, including one-letter names, number-only names, names with numbers before or after words, multiple words, full sentences, symbols, punctuation, emoji characters, and very long names.
- Trim only the spaces at the beginning and end of `Saving` square names before saving, while preserving spaces inside the trimmed name.
- Make long `Saving` square names wrap or stay contained so they do not create horizontal page overflow, overlap square content, or break the square layout.
- Build create, rename, delete, and reorder square actions.
- Keep create or rename open with no message and no saved data changes when the user tries to save a missing `Saving` name.
- Keep create or planned-money-amount change open with no message and no saved data changes when the user tries to save a missing planned money amount.
- Keep the `Saving` square create input step open with no message and no saved data changes when the user tries to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$`.
- Add bottom text actions `Save` and `Cancel` to `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows.
- Make `Cancel` close the `Saving` square input flow, return to the `Saving` squares view, and change nothing.
- Do not remember unsaved typed `Saving` input as a draft.
- Discard the typed `Saving` name and planned money amount if the user refreshes the page, closes the browser tab or window, reopens the website later, cancels the input flow, closes the input flow with `<` or Back, or leaves `Savings` after the input flow has been closed before a successful `Save`.
- Restore only the last successfully saved data after discarded unsaved `Saving` input, without saving a draft to browser storage, creating a `Balance Changes` entry, showing a browser leave warning, or showing a message.
- After a failed `Saving` creation save, keep `Changes could not be saved.` visible while the flow is unchanged. Remove the previous message after an accepted name or planned-money edit, while retrying, or when the flow closes or is discarded; show it again if the retry also fails.
- After a failed broken-square fix save, keep `Changes could not be saved.` visible while the flow is unchanged. Remove the previous message after an accepted name or planned-money edit, while retrying, or when the flow closes or is discarded; show it again if the retry also fails.
- For new `Saving` square creation, validate in this order: planned money amount missing, `0.00$`, or greater than `999,999.99$`; missing `Saving` name; then duplicate `Saving` name.
- Count normal `Saving` squares and visible broken saved `Saving` squares with readable non-empty saved names as reserved names during duplicate-name checks.
- During a broken-square fix, do not count the broken `Saving` square being fixed as a duplicate against itself, but still count normal `Saving` squares and other broken `Saving` squares with the same trimmed name.
- For broken-square fix, validate in this order: planned money amount missing, `0.00$`, or greater than `999,999.99$`; missing `Saving` name; then duplicate `Saving` name.
- If a broken-square fix attempt has a duplicate name and a missing, `0.00$`, or above-limit planned money amount, make the planned-money-amount rule happen first so the same broken-square fix input step stays open with no message and no saved data change.
- Return to the `Saving` squares view with no changes when creating, renaming, or fixing a `Saving` square with a duplicate name.
- Make delete ask for confirmation in a small centered confirmation square with `Delete this Saving?`, show `Cancel` and `Delete`, remove only the selected `Saving` square and its saved details after confirmation, and not offer undo.
- Build planned-money-amount editing flows that replace the old planned money amount with a new total planned money amount.
- Remove a `Saving` square if its planned money amount becomes `0.00$`.
- Save `Saving` square changes to browser storage.
- Render a broken saved `Saving` square as an error square with exact text `Saving could not be loaded.` and actions `Fix` and `Delete`.
- Keep valid saved data loaded when only one saved `Saving` square is broken.
- Exclude broken saved `Saving` squares from `Savings money amount`, top needed text, and coverage calculations until fixed.
- Keep broken saved `Saving` squares locked during reorder until fixed or deleted, while still allowing normal default `Saving` squares to be reordered around those locked broken squares.
- Make `Fix` replace the broken square with a temporary `Saving` input square in the same visible position.
- Start a broken-square fix with no entered name text or planned-money digits, copy no input value from the broken record, and do not insert `0` or `0.00`.
- Make the broken-square fix input square ask for a valid `Saving` name and planned money amount greater than `0.00$` and not greater than `999,999.99$`.
- Do not render the broken-square fix input square as a modal, bottom sheet, or separate page.
- Give each loaded broken record a unique in-memory symbol that is recreated on every complete load and is never written to browser storage.
- If the selected broken record is no longer available, close and discard the fix flow without saving or showing a message, and keep the latest successfully saved information visible.
- Before repairing, reserve every usable order number from remaining broken records and assign normal records increasing unused safe-integer orders in their visible order; fail without writing if no safe order remains.
- Make a successful broken-square fix save browser storage, turn the broken square into a normal `Saving` square, keep it in the same visible position when possible, give it a valid unique ID and valid order if needed, recalculate the `Savings money amount`, top needed text, and coverage bars, create no `Balance Changes` entry, and show no message.
- After a successful broken-square fix, revalidate the complete saved `Saving` list so any other unchanged record that is now valid becomes normal and every still-invalid record remains broken.
- Make broken-square `Delete` use the same small centered `Saving` delete confirmation with `Delete this Saving?`, show `Cancel` and `Delete`, remove only that broken square after confirmation, save browser storage, create no `Balance Changes` entry, and offer no undo.
- Save the final `Saving` square order to browser storage after the user finishes moving a square and lets go.
- Recalculate the money amount shown inside `Savings`, top needed text, and coverage bars from the visible order of normal `Saving` squares after the final reorder, skipping broken saved `Saving` squares.

Acceptance criteria:
- Money amount shown inside `Savings` starts equal to the current money amount when no `Saving` squares exist.
- The money amount shown at the top of `Savings` uses the label `Savings money amount`.
- The `Savings money amount` stays visible at the top of the full-screen `Savings` view while the user scrolls through many `Saving` squares.
- `Saving` squares do not get hidden behind the fixed top `Savings` area while scrolling.
- `Savings money amount` at `0.00$` uses the same visual style as nonzero `Savings money amount` values.
- `Savings money amount` at `0.00$` does not show a warning style, error style, icon, or extra message only because it is `0.00$`.
- When total planned money amount in valid `Saving` squares is greater than the main money amount, the top `Savings` area shows `{money amount} needed` directly under `Savings money amount`.
- The top needed text uses the difference between total planned money amount in valid `Saving` squares and the main money amount.
- The top needed text is hidden when total planned money amount in valid `Saving` squares is equal to or less than the main money amount.
- The main money amount keeps the label `Current Balance`.
- Visible `Saving` squares appear in one vertical column on mobile and desktop.
- `Saving` squares do not appear in multiple columns or a grid.
- Creating a `Saving` square with a planned money amount reduces only the money amount shown inside `Savings`.
- Typing unsaved values in `Saving` square input flows does not preview-change the `Savings money amount`, top needed text, or coverage bars.
- Successful `Save` in a `Saving` square input flow updates the `Savings money amount`, top needed text, and coverage bars when the saved data changes.
- A planned money amount in a `Saving` square does not reduce the main money amount.
- `Saving` square planned money amounts do not change automatically when the main money amount changes.
- Changing a `Saving` square planned money amount replaces the old planned money amount instead of adding to or subtracting from it.
- Changing a `Saving` square planned money amount updates the money amount shown inside `Savings`, top needed text, and coverage bars.
- Changing a `Saving` square planned money amount does not change the main money amount or create a `Balance Changes` entry.
- The Savings section helps the user see which plans are fully covered, partly covered, or not covered.
- A default `Saving` square shows its name at the top left, its planned money amount on the right side of the top row, and the thin coverage bar at the bottom.
- Clicking a `Saving` square opens that same square's action state.
- The `Saving` square action state shows the name at the top left, the planned money amount on the right side of the top row, and `Delete` at the bottom center.
- The `Saving` square action state hides the thin coverage bar.
- Clicking a `Saving` square name in the action state opens rename.
- Clicking a `Saving` square planned money amount in the action state opens planned-money-amount change.
- Clicking `Delete` in the action state starts delete confirmation.
- `Saving` square delete confirmation appears as a small confirmation square in the middle of the screen.
- `Saving` square delete confirmation shows `Delete this Saving?`, `Cancel`, and `Delete`.
- Only one `Saving` square delete confirmation can be open at a time.
- Clicking `Cancel` or outside the `Saving` square delete confirmation closes it without changing saved data.
- Clicking `Delete` in the small confirmation square closes it and deletes only the selected `Saving` square.
- Clicking or tapping outside an open `Saving` square action state closes that action state, changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
- Clicking or tapping inside the open action-state square does not close it as an outside click.
- Clicking or tapping a blank part inside the open action-state square does nothing and keeps that square in action state.
- Clicking another normal default `Saving` square while one square is in action state closes the old action state and does not open the clicked square from that same click.
- Scrolling by itself does not close an open `Saving` square action state.
- Clicking `<` while a `Saving` square is in action state returns to the dashboard and clears the open action state.
- Rename, planned-money-amount change, and delete do not open from a separate action menu or larger action square.
- The user can add a `Saving` square with a circle `+` action.
- When no visible `Saving` squares exist, the circle `+` action appears in the middle of the `Saving` squares area.
- The no-`Saving`-squares empty state uses the centered circle `+` action only when no normal or broken saved `Saving` squares are visible, and does not show a separate empty-state text sentence.
- After at least one normal `Saving` square exists, the circle `+` action appears at the top-left of the `Saving` squares area.
- If only broken saved `Saving` squares are visible, the circle `+` action appears at the top-left above or before those broken squares.
- Clicking the circle `+` action opens the same create flow from the centered empty state and from the top-left non-empty state.
- Clicking the circle `+` action replaces that `+` with a temporary `Saving` input square inside the `Saving` squares area.
- The temporary create input square appears centered when opened from the centered empty-state `+`, and appears at the top-left before existing squares when opened from the top-left `+`.
- `Saving` square create does not open as a modal, bottom sheet, or separate page.
- A new `Saving` square is created only after the user enters a valid name and a planned money amount greater than `0.00$` and not greater than `999,999.99$`.
- A successfully created `Saving` square appears after all existing `Saving` squares and has the last coverage priority until the user reorders it.
- `Saving` square planned money amount inputs show raw decimal number text while typing and do not add a `$` sign, comma separators, or automatic two-decimal formatting before `Save`.
- `Saving` square planned money amount inputs do not show a `Cent` button, do not use the main money amount `Space` key cents behavior, and block `Space` with no message.
- In `Saving` square planned money amount inputs, typing `14` saves and displays as `14.00$`, typing `14.5` saves and displays as `14.50$`, typing `5898` saves and displays as `5,898.00$`, and typing `589.80` saves and displays as `589.80$`.
- If the user tries to save a new `Saving` square without a `Saving` name, nothing happens: no message appears, no square is created, saved data stays unchanged, and the create flow stays open until the user enters a name or cancels.
- If the user tries to save a new `Saving` square without a planned money amount, nothing happens: no message appears, no square is created, saved data stays unchanged, and the create flow stays open until the user enters a planned money amount or cancels.
- If the user tries to rename a `Saving` square with an empty name, nothing happens: no message appears, the old name stays saved, saved data stays unchanged, and the rename flow stays open until the user enters a name or cancels.
- If the user tries to change a `Saving` square planned money amount with an empty planned money amount or a planned money amount greater than `999,999.99$`, nothing happens: no message appears, the old planned money amount stays saved, saved data stays unchanged, and the change flow stays open until the user enters a valid planned money amount or cancels.
- `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows show bottom text actions `Save` and `Cancel`.
- `Cancel` in `Saving` square create, rename, planned-money-amount change, or broken-square fix closes the input flow, returns to the `Saving` squares view, changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
- If the user enters a duplicate name while creating a `Saving` square, the website returns to the `Saving` squares view, shows no duplicate-name error message, creates no new `Saving` square, and keeps saved data unchanged.
- If the user enters a duplicate name while renaming a `Saving` square, the website returns to the `Saving` squares view, shows no duplicate-name error message, keeps the old name, and keeps saved data unchanged.
- If the user enters a duplicate name while fixing a broken `Saving` square, the website returns to the `Saving` squares view, shows no duplicate-name error message, keeps the broken square broken, and keeps saved data unchanged.
- Duplicate `Saving` square name checks use trimmed names and ignore uppercase or lowercase differences.
- Duplicate `Saving` square name checks count visible broken saved `Saving` squares with readable non-empty saved names as reserved names until those broken squares are fixed or deleted.
- A broken `Saving` square being fixed does not count as a duplicate against itself, but normal `Saving` squares and other broken `Saving` squares with the same trimmed name still block the fix.
- Broken-square fix validation uses the same order as new `Saving` square creation: planned money amount first, missing `Saving` name second, and duplicate `Saving` name third.
- A duplicate `Saving` name with a missing, `0.00$`, or above-limit planned money amount during broken-square fix keeps the same broken-square fix input step open with no message and no saved data change.
- `Saving` square names can be one letter, only numbers, include numbers before or after words, include multiple words, be full sentences, include symbols, include punctuation, or include emoji characters.
- A valid `Saving` square name is not rejected only because it is long.
- Long `Saving` square names stay contained in the square layout without horizontal page overflow or overlap.
- A new `Saving` square is not created with a `0.00$` planned money amount or a planned money amount greater than `999,999.99$`.
- If the user tries to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$`, nothing happens: no message appears, no square is created, saved data stays unchanged, `Balance Changes` does not get a new entry, and the same `Saving` square create input step stays open until the user enters a planned money amount greater than `0.00$` and not greater than `999,999.99$` or cancels.
- If a new `Saving` square create attempt has a duplicate name and a missing, `0.00$`, or above-limit planned money amount, the planned-money-amount rule happens first, so the same `Saving` square create input step stays open with no message and no saved data change.
- The planned money amount in a `Saving` square does not mean money has moved into a separate place.
- A `Saving` square disappears from `Savings` if its planned money amount becomes `0.00$`.
- A `0.00$` `Saving` square is not saved in browser storage.
- Deleting a `Saving` square removes only that square and its saved details.
- Deleting a `Saving` square does not change the main money amount, `Balance Changes`, or the names, planned money amounts, and relative order of other `Saving` squares.
- After deleting a `Saving` square, the money amount shown inside `Savings`, top needed text, and coverage bars update from the remaining squares.
- Touch users can reorder `Saving` squares by holding a normal default square for `600ms`, then moving the whole square.
- Mouse users can reorder `Saving` squares by clicking and holding a normal default square for `600ms`, then dragging the whole square.
- Broken saved `Saving` squares cannot be dragged or reordered until fixed or deleted.
- Holding, clicking and holding, moving, or dragging a broken saved `Saving` square does not start reorder, does not show a drag placeholder, changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
- Normal default `Saving` squares can still be reordered while broken saved `Saving` squares are visible, and broken saved `Saving` squares stay in their displayed positions.
- Touch movement greater than `8px` before the `600ms` reorder hold completes is treated as scrolling and cancels the pending tap and pending reorder hold.
- `Saving` square reorder starts only after a completed `600ms` hold and at least `8px` of pointer or finger movement.
- Holding a normal default `Saving` square for `600ms` and releasing before moving at least `8px` does nothing: no action state, no reorder, no drag placeholder, no saved change, no `Balance Changes` entry, and no message.
- The dragged `Saving` square follows the user's finger or mouse pointer and leaves a same-size placeholder in the old position.
- Dragging within `40px` of the top or bottom edge of the scrollable `Saving` squares area auto-scrolls that area at a fixed speed of `8px` per animation frame.
- Top and bottom reorder auto-scroll use the same trigger distance and speed, and speed does not change based on edge distance.
- If a `Saving` square reorder drag is interrupted before the square is dropped, the square returns to its original position, the drag placeholder is removed, auto-scroll stops, nothing saves, no `Balance Changes` entry is created, and no message appears.
- Reordering is disabled while any `Saving` square is in action state, input state, or delete confirmation state.
- Holding or dragging a `Saving` square does not open rename, planned-money-amount change, or delete.
- The visible order of normal `Saving` squares is the coverage order, skipping broken saved `Saving` squares.
- A normal `Saving` square moved to the top is checked first and its coverage bar is calculated before lower normal squares use what remains.
- Reordering `Saving` squares saves the final order after the user lets go.
- Reordering normal `Saving` squares updates the money amount shown inside `Savings`, top needed text, and coverage bars from the new visible order of normal `Saving` squares, skipping broken saved `Saving` squares.
- Reordering `Saving` squares does not change the main money amount and does not create a `Balance Changes` entry.
- Coverage bars fill green from left to right by the covered percentage of each `Saving` square.
- A `Saving` square that is 80% covered shows the left 80% of the coverage bar as green and the right 20% as grey.
- A not fully covered `Saving` square shows its `{money amount} needed` note at the top-left of the bottom coverage bar.
- The thin bottom coverage bar is hidden while a `Saving` square is in its action state.
- A broken saved `Saving` square shows `Saving could not be loaded.` with `Fix` and `Delete`.
- A broken saved `Saving` square does not lower the `Savings money amount`, does not affect top needed text, does not show a coverage bar, and does not affect valid square coverage calculations.
- A broken saved `Saving` square stays locked in its current displayed position until fixed or deleted.
- Fixing a broken saved `Saving` square starts by replacing that broken square with a temporary `Saving` input square in the same visible position.
- Broken-square fix does not open as a modal, bottom sheet, or separate page.
- Saving a fixed broken `Saving` square turns it into a normal `Saving` square, keeps it in the same visible position when possible, and creates no `Balance Changes` entry.
- Deleting a broken saved `Saving` square removes only that broken square and creates no `Balance Changes` entry.
- If the total planned money amount in `Saving` squares is greater than the main money amount, the money amount shown inside `Savings` is `0.00$`.
- If the total planned money amount in valid `Saving` squares is greater than the main money amount, the top `Savings` area shows the difference as `{money amount} needed`.
- When the money amount shown inside `Savings` is `0.00$`, it looks like a normal `Savings money amount` value, not a warning or error.
- The user opens the Savings section by clicking `Savings`, not by using a separate `Open savings` action.
- `Savings` opens as a full-screen view with a small `<` sign in the top-left corner.
- Clicking `<` returns to the dashboard without data changes, saved changes, or `Balance Changes` entries.
- The full-screen `Savings` view shows the `Savings money amount` and the `Saving` squares together.
- The full-screen `Savings` view keeps the `Savings money amount` fixed at the top while `Saving` squares scroll.
- `Saving` square changes do not show as added or subtracted cash.
- A user can create at least one `Saving` square.
- Savings changes do not make the total money amount confusing.

### Milestone 6: Polish

Tasks:
- Show `Balance Changes` without replacing separate add and subtract entries.
- Improve mobile spacing and touch targets.
- Add only the specified empty-state behavior: keep empty `Balance Changes` blank, and use the centered circle `+` action as the empty state for no `Saving` squares.
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
- Test long `Balance Changes` history by confirming that every current entry remains reachable, the `Savings` action remains reachable whether history is empty or long, and the `Balance Changes` scroll position moves to the top after a successful `Add` or `Subtract` creates a new entry on mobile and desktop.
- Test `Saving` square planned money amount input display with raw decimal typing before `Save`, no `Cent` button, no main money amount `Space` key cents behavior, and formatted display only after `Save`.
- Test exact Savings coverage with no `Saving` records, zero money, exact and excess money, the `100.00$` with `80.00$` and `50.00$` example, changed order, broken records, failed actions, and combined planned totals above JavaScript's safe-integer range.
- Test that a new `Saving` is appended after all existing `Saving` squares, receives order `0` when it is first or one more than the highest usable order otherwise, and does not change existing order or coverage priority.
- Test a failed `Saving` creation save and confirm its message clears after an accepted edit, retry, or closure, remains after rejected amount input, and appears again after another failed retry.
- Test unsaved `Saving` input recovery by typing a `Saving` name and planned money amount, then canceling, using `<` or Back, leaving `Savings`, refreshing, and reopening later before `Save`; confirm only last successfully saved data returns, no draft is saved, no `Balance Changes` entry is created, no browser leave warning appears, and no message appears.
- Test duplicate-name checks against broken `Saving` squares by loading a broken square with a readable non-empty saved name; confirm create, rename, and fixing another broken square with that same trimmed name return to the `Saving` squares view with no duplicate-name error, no saved data change, no `Balance Changes` entry, and the broken square still reserving the name until fixed or deleted.
- Test fixing a broken `Saving` square using its own readable saved name when no other square uses that same trimmed name; confirm the square does not block itself and can be fixed if all other fix values are valid.
- Test broken-square fix validation order by entering a duplicate name with a missing, `0.00$`, or above-limit planned money amount; confirm the planned-money-amount rule happens first, the same broken-square fix input step stays open, no message appears, no data saves, no square is fixed, and no `Balance Changes` entry is created.
- Test that a new broken-square fix starts with empty raw name and planned-money values, `confirming` and failure visibility set to `false`, and no information copied from the broken record.
- Test a failed broken-square fix and confirm its message clears after an accepted edit, retry, or closure, remains after rejected amount input, and appears again after another failed retry.
- Test that an unavailable broken-square fix target closes the stale flow without calling the storage writer or showing a message and leaves the latest successfully saved information visible.
- Test a repair with usable order numbers in remaining broken records; confirm those numbers are reserved, normal records receive increasing unused safe orders without changing their relative order, and no normal record becomes broken from a new duplicate order.
- Test complete-list revalidation after a successful fix; confirm another unchanged record becomes normal only if it now passes every validation rule and otherwise remains broken.
- Test touch scrolling over `Saving` squares by moving the finger more than `8px` before `600ms`; confirm the list scrolls, action state does not open, reorder does not start, no drag placeholder appears, nothing saves, no `Balance Changes` entry is created, and no message appears.
- Test holding a normal default `Saving` square for `600ms` and releasing before moving at least `8px`; confirm no action state opens, no reorder starts, no drag placeholder appears, nothing saves, no `Balance Changes` entry is created, and no message appears.
- Test `Saving` square reorder auto-scroll by dragging within `40px` of the top and bottom edges of the scrollable `Saving` squares area; confirm the area auto-scrolls at a fixed `8px` per animation frame, top and bottom use the same behavior, and speed does not change based on edge distance.
- Test interrupted `Saving` square reorder by moving the finger or mouse pointer off the screen, making the browser window lose focus, or otherwise interrupting the drag before drop; confirm the square returns to its original position, the placeholder is removed, auto-scroll stops, nothing saves, no `Balance Changes` entry is created, and no message appears.
- Test `Savings` with only broken saved `Saving` squares and confirm the circle `+` action appears at the top-left above or before the broken squares, broken squares cannot be reordered, and normal default squares can still be reordered while broken squares stay locked in their displayed positions.
- Test `Balance Changes` delete opening behavior for `600ms` touch press-and-hold and `600ms` mouse click-and-hold, including pending-hold cancellation on early release, pointer or finger movement, or scrolling before `600ms`; the centered little `Delete` and `Cancel` square; single-open behavior; `Cancel`, outside-click, and browser Back close behavior; movement-keeps-open behavior; scroll-keeps-open behavior; and the exact confirmation message `Delete this Balance Change?` with buttons `Cancel` and `Delete` where `Cancel`, outside click, or browser Back closes the confirmation without deleting the entry.
- Confirm out-of-scope banking features are not implied.
- Move this plan from `docs/plans/active` to `docs/plans/done` after completion.

Acceptance criteria:
- A user can open the website, see `0.00$`, add cash, subtract cash, create a `Saving` square, and review `Balance Changes`.
- No real banking language or fake account behavior is present.
- The MVP satisfies the accepted `GlobalSpec` direction and `FeatureSpec` requirements for the first version.

## Main User Flow

1. User opens the website.
2. If no saved money amount exists yet, website shows `0.00$` labeled as `Current Balance`.
3. User clicks the main money amount.
4. Website shows `Add` and `Modify`, but not `Subtract`.
5. User chooses `Add`.
6. User enters an amount greater than `0.00$`.
7. Website updates the money amount and saves the add action as `+{money amount} added`.
8. After the money amount is greater than `0.00$`, user clicks the main money amount.
9. If the money amount is greater than `0.00$` and less than `999,999.99$`, website shows `Add`, `Subtract`, and `Modify`.
10. If the money amount is exactly `999,999.99$`, website shows `Subtract` and `Modify`, but not `Add`.
11. User chooses one visible action.
12. User enters an amount.
13. Website updates the money amount.
14. If the action is `Add` or `Subtract`, website saves it as its own history entry with the correct display type.
15. If the action is `Modify` and the entered money amount is different from the money amount already shown, website updates the current money amount without saving history or showing a notification.
16. Website shows money amount change history in `Balance Changes` on the dashboard.
17. Website saves the updated data in browser storage.
17. User reviews `Saving` squares.
18. User can close the website and open it again later to see saved data when it is available.

## Validation Rules

- Main money action input amounts may include cents and must use valid money amount text.
- Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`, and should use comma separators every three digits before the decimal point for values of `1,000.00$` or more, such as `5,895.50$` and `999,999.99$`.
- Main money action inputs should start at `0.00`.
- Main money action inputs should focus immediately when they open so the user can start typing without another tap or click.
- Main money action inputs should request a mobile keyboard suitable for digit entry on supported devices.
- Main money action inputs should provide an action with the exact visible name `Cent` on mobile and desktop without requiring a particular shape, position, size, or visual arrangement. Choosing `Cent` should keep the input active, and the mobile digit-entry keyboard should remain available when the browser permits it.
- Main money action inputs should accept only numbers from `0` through `9`, with `Cent` or its keyboard alternative `Space` starting cents entry, without letting the user type the `$` sign. Typed decimal points and manually typed comma separators should be blocked with no message. The input should add the decimal point and comma separators automatically while the user is typing.
- Main money action inputs should accept `Cent` or `Space` only after at least one entered digit and only once per input. The initial `0.00` should not count as an entered digit. `Cent` or `Space` before a digit or after cents entry has already started should be ignored without a message. The flow should visibly indicate when cents entry is active without requiring a particular visual treatment.
- Main money action inputs should automatically format typed values with two digits after the decimal point, automatic comma separators for thousands and larger values, and no `$` sign while the user is typing. Before cents entry starts, typed digits are whole money amount digits: typing `5` should show `5.00`, `58` should show `58.00`, `589` should show `589.00`, `5895` should show `5,895.00`, `58955` should show `58,955.00`, and `589550` should show `589,550.00`.
- After `Cent` or `Space` starts cents entry, the next one or two digits should be cents, with one digit left-padded by `0`. A third cents digit should be blocked without a message. All digits entered before cents entry should remain the whole money amount, with comma separators added automatically.
- Main money action inputs should treat `0` as a normal digit, not a starting zero-position skip. Unneeded leading zeros in the whole money amount should be normalized away, so typing `0005` shows `5.00`.
- Deleting one typed character from a main money action input should reformat the remaining typed value with two digits after the decimal point and no `$` sign while the input is still open.
- Deleting all typed numbers from a main money action input should return it to `0.00` instead of making it empty.
- Main money action inputs should be append-only: users should not move the cursor into the formatted value, select part of it, replace selected text, or directly edit generated comma separators or the generated decimal point.
- Main money action input typing should add only to the end, and Backspace or Delete should remove only the last accepted digit or the action that started cents entry.
- Main money action inputs should block letters, minus signs, decimal points, `$` signs, manually typed comma separators, `Cent` or `Space` before any entered digit, another `Cent` or `Space` after cents entry starts, a third cents digit, and other invalid typed characters.
- Main money action inputs should block typed characters that would make the input greater than `999,999.99`, with no message and no field change.
- The money amount entry flow, dashboard, `Balance Changes`, and `Savings` displays should keep money amounts up to `999,999.99$` and required controls readable and usable without rejecting valid values or omitting required comma separators.
- Main money action inputs should block paste. Pasted content should not appear, the input should keep its previous value, and no message should appear.
- Main money action input flows should show the exact text `Save Changes` and provide actions exactly named `Cent`, `Yes`, and `Cancel` without requiring a particular visual arrangement.
- In main money action input flows, `Yes` should try to save the entered value using the selected `Add`, `Subtract`, or `Modify` rule.
- In main money action input flows, `Cancel` should close the input flow, return to the dashboard, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- In main money action input flows, clicking or tapping outside the active entry flow should act like `Cancel` and should not reopen the main money actions.
- In main money action input flows, choosing `Cent` should behave like pressing `Space`, start cents entry after at least one entered digit, keep the input active, and not act like `Cancel`.
- `Saving` square planned money amount inputs may include cents, with up to two digits after the decimal point, and should not save values greater than `999,999.99$`.
- `Saving` square planned money amount inputs should show the accepted typed value as raw decimal number text while typing, with no `$` sign, no comma separators, and no automatic two-decimal formatting before `Save`.
- `Saving` square planned money amount inputs should accept digits from `0` through `9` and one decimal point, block letters, `$` signs, comma separators, a second decimal point, more than two digits after the decimal point, and `Space` with no message, should not show a `Cent` button, and should not use the main money amount `Space` key cents behavior.
- `Saving` square planned money amount inputs should request a decimal numeric keyboard on devices that support it, so the user gets number keys `0` through `9` and a decimal point.
- `Saving` square planned money amount inputs should enforce the `999,999.99$` maximum and should not save values above that limit.
- `Saving` square planned money amount inputs should not require users to type comma separators; saved planned money amounts should use normalized plain decimal strings without the `$` sign or comma separators, such as `5895.50`, while rendered planned money amounts of `1,000.00$` or more should show required comma separators, such as `5,895.50$`.
- `Saving` square planned money amount inputs should block paste. Pasted content should not appear, the input should keep its previous value, and no message should appear.
- `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows should show bottom text actions `Save` and `Cancel`.
- `Saving` square create and broken-square fix input flows should use temporary `Saving` input squares inside the `Saving` squares area, not modals, bottom sheets, or separate pages.
- The create input square should replace the clicked circle `+` while open.
- The broken-square fix input square should replace the broken saved `Saving` square while open.
- In `Saving` square input flows, `Save` should try to save the entered values using the rules for that action.
- In `Saving` square input flows, `Cancel` should close the input flow, return to the view the user was already using, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- In `Saving` square input flows, unsaved typed `Saving` names and planned money amounts should not be remembered as drafts after refresh, browser tab or window close, later website reopen, cancel, `<`, Back, or leaving `Savings` after the input flow has been closed before a successful `Save`.
- Discarded unsaved `Saving` input should not save browser storage, create a `Balance Changes` entry, show a browser leave warning, or show a message.
- `Add` should be available only when the money amount is less than `999,999.99$`, including when the money amount is `0.00$`.
- `Modify` should be available when the money amount is from `0.00$` through `999,999.99$`.
- `Subtract` should be available only when the money amount is greater than `0.00$`.
- At exactly `999,999.99$`, the visible main money actions should be `Subtract` and `Modify`, with no `Add`.
- Add and subtract amounts must be greater than `0.00$` and not greater than `999,999.99$`.
- Clicking `Yes` while the input is `0.00` in `Add` or `Subtract` should do nothing: no message, no money amount change, no saved data change, no `Balance Changes` entry, and the same money amount input step stays open.
- Modify amounts must be from `0.00$` through `999,999.99$`.
- Negative amounts should be blocked for all money actions.
- Subtracting more than the current money amount should set the current money amount to `0.00$`.
- Subtracting more than the current money amount should save history using the actual amount removed from the money amount.
- Subtracting when the current money amount is `0.00$` should not create a history entry.
- The current money amount should never be negative and should never be greater than `999,999.99$`.
- `Saving` square name is required.
- Visible `Saving` squares should render in one vertical column on mobile and desktop, not in multiple columns or a grid.
- Trying to save a missing `Saving` name should do nothing until the user enters a `Saving` name or cancels.
- `Saving` square names should not have a maximum length.
- `Saving` square names may be one letter, only numbers, include numbers before or after words, include multiple words, be full sentences, include symbols, include punctuation, or include emoji characters.
- Spaces at the beginning and end of a `Saving` square name should be trimmed before saving, while spaces inside the trimmed name should stay.
- `Saving` square name should be unique inside `Savings`.
- Duplicate `Saving` square name checks should use trimmed names and ignore uppercase or lowercase differences.
- Duplicate-name checks should include normal `Saving` squares and visible broken saved `Saving` squares with readable non-empty saved names.
- During broken-square fix, the broken square being fixed should not block itself, but matching names from normal `Saving` squares or other broken `Saving` squares should still block the fix.
- `Saving` square planned money amount should be greater than `0.00$` and not greater than `999,999.99$` when creating a `Saving` square.
- Trying to save a missing planned money amount should do nothing until the user enters a planned money amount or cancels.
- Trying to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$` should do nothing: no message, no new `Saving` square, no saved data change, no `Balance Changes` entry, and the same `Saving` square create input step stays open.
- New `Saving` square create validation should check planned money amount first, missing `Saving` name second, and duplicate `Saving` name third.
- A duplicate `Saving` name with a missing, `0.00$`, or above-limit planned money amount should keep the same `Saving` square create input step open with no message and no saved data change.
- `0.00$` should remove an existing `Saving` square instead of saving it with a `0.00$` planned money amount.
- Changing a `Saving` square planned money amount should use a new total planned money amount, not an add or subtract amount.
- Total planned money amount in valid `Saving` squares may be greater than the current money amount. The money amount shown inside `Savings` should stop at `0.00$`, coverage bars should show what is still needed for each square, and the fixed top `Savings` area should show the total difference as `{money amount} needed`.
- `Balance Changes` created dates and created times use the local date and time recorded when the action succeeds and the fixed English format shown by `September 18, 2026 at 2:30 PM`. Each entry also saves an exact created timestamp with milliseconds for ordering and expiration, but does not show seconds or milliseconds to the user.
- The user should not choose dates for `Add` or `Subtract`.
- `Add`, `Subtract`, and `Balance Changes` entries should not have notes.
- Browser storage data should be checked before use so broken saved data does not crash the website.
- Broken saved data should show `Saved data could not be loaded.` and a `Start again` action.
- Saved browser data should load only when the data version value is exactly `1`.
- Saved browser data with a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, should be treated as broken saved data.
- Clicking `Start again` should replace the broken saved data in one successful complete save with the saved money amount set to `0.00`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`, without deleting the storage key first.
- If saved browser data can be read and has data version `1`, one broken saved `Saving` square should appear as a broken square in `Savings` instead of triggering the full saved-data error.
- Visible history entries should be shown for exactly 720 hours.
- For visible `Balance Changes` history, 30 days means exactly 720 hours, not 30 calendar dates or one calendar month.
- Each `Balance Changes` internal visible-until timestamp should equal its exact created timestamp plus `2_592_000_000` milliseconds.
- Expiring old history entries should not change the current money amount.

## Browser Storage Rules

- Save website data in browser storage on the user's device.
- The first version should not use email accounts, login, cloud sync, or a server database.
- Do not create saved browser data only because the website opened and showed the default `0.00$` money amount.
- Save after every successful money amount change, `Saving` square change, or `Balance Changes` delete.
- If the same website is open in multiple tabs or windows in the same browser, a successful saved change in one tab or window should automatically update the other open tabs or windows to the latest saved data.
- Same-browser tab or window updates should refresh the visible money amount, `Balance Changes`, `Savings`, `Savings money amount`, top needed text, and coverage bars from the latest saved data.
- When that update contains a new saved `Add` or `Subtract` entry, the receiving tab or window should move `Balance Changes` to the top so the newest entry is visible.
- Same-browser tab or window updates should happen silently, create no `Balance Changes` entry, and should not write browser storage again only because another tab or window saved a change.
- If a receiving tab or window has a temporary UI open when a same-browser storage update arrives, close that temporary UI like a cancel, discard unsaved typed input, save nothing from that tab or window, delete nothing from that tab or window, create no `Balance Changes` entry from that tab or window, and show the latest saved data.
- Do not implement a normal user-controlled reset, clear-all-data, or start-fresh action for valid saved data in the first version.
- Show `Start again` only for broken or unreadable saved data.
- Do not save temporary UI state, unsaved typed input, pending delete state, or pending reorder state in browser storage.
- If the user refreshes the page, closes the browser tab or window, or reopens the website later while temporary UI is open, silently discard that temporary UI and show the latest successfully saved data with no temporary UI open.
- Do not show a browser leave warning only because temporary UI is open.
- Treat a user action as successful only after the required browser storage write succeeds.
- If browser storage is unavailable, full, blocked, or fails while saving a valid user action, block that action and show `Changes could not be saved.`.
- On browser storage save failure, keep the visible money amount, `Balance Changes`, `Savings`, and saved data on the last successfully saved state.
- On browser storage save failure, do not create, update, delete, or reorder saved data, do not create a `Balance Changes` entry, and do not run `Balance Changes` cleanup as a successful saved user action.
- Keep main money amount input flows open with the same typed value after a browser storage save failure.
- Keep `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows open with the same typed values after a browser storage save failure.
- Keep `Saving` square delete and `Balance Changes` delete confirmations open with the selected item still visible after a browser storage save failure.
- Return a failed `Saving` square reorder to the last successfully saved order after a browser storage save failure.
- Do not save browser storage for `Add`, `Subtract`, or `Modify` attempts with an empty or above-limit money amount because nothing changed.
- Do not save browser storage for `Add` or `Subtract` attempts with `0.00$` because nothing changed.
- Do not save browser storage, create browser storage, run `Balance Changes` cleanup, close the input flow, or show a message when `Modify` is confirmed with the same money amount that is already shown.
- Do not save browser storage for new `Saving` square attempts with a planned money amount of `0.00$` or greater than `999,999.99$` because nothing changed.
- Save `Modify` changes only as an updated current money amount, not as a history entry.
- Save `Balance Changes` deletes only as visible history removal, not as a money amount change.
- Keep a neutral loading state while checking saved browser data; show neither the dashboard nor recovery, and do not briefly show the default `0.00$` dashboard before the check finishes.
- If saved browser data exists, restore the money amount, 30-day visible `Balance Changes` history, and `Saving` squares.
- Restore saved browser data only when the data version value is exactly `1`.
- Treat saved browser data with a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, as broken saved data.
- When the user chooses `Start again` after broken saved data, replace it in one successful complete save with the saved money amount set to `0.00`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`, without deleting the storage key first.
- After a failed `Start again`, keep `Changes could not be saved.` visible while recovery is unchanged, remove it while retrying or when recovery closes, show it again after another failure, and do not restore it in a later new recovery state.
- If saved browser data can be read and has data version `1`, do not show the full saved-data error only because one saved `Saving` square is broken.
- A saved `Saving` square should be treated as broken when it has a missing ID, duplicate ID, missing name, empty trimmed name, duplicate trimmed name ignoring uppercase or lowercase differences, missing planned money amount, invalid planned money amount, planned money amount of `0.00$` or less, planned money amount greater than `999,999.99$`, missing order, invalid order, or duplicate order.
- For duplicate saved IDs, duplicate saved names, or duplicate saved orders, keep the first matching saved `Saving` square in saved list order as normal if it is otherwise valid, and treat later matching saved `Saving` squares as broken.
- Show broken saved `Saving` squares in `Savings` with `Saving could not be loaded.`, `Fix`, and `Delete`.
- Exclude broken saved `Saving` squares from `Savings money amount`, top needed text, coverage calculations, and coverage bars until fixed.
- Keep broken saved `Saving` squares locked in their displayed positions until fixed or deleted, and do not include them in reorder calculation.
- Save browser storage after a successful broken-square fix or confirmed broken-square delete.
- Delete visible history entries from browser storage at or after their visible until date during cleanup without changing the saved current money amount.
- Run `Balance Changes` cleanup when the website opens and loads saved data, and after every successful saved user action.
- Do not add a background timer for old `Balance Changes` cleanup while the website stays open with no user action.
- If no saved browser data exists, show the dashboard with the money amount set to `0.00$`.
- If saved browser data is broken or unreadable, show `Saved data could not be loaded.` and let the user choose `Start again`.
- Use one stable storage key for website data: `cash-money-organizer-website-data`.
- Include data version `1` in first-version saved data so future versions can upgrade old data safely.
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
- Manual Cash Tracker.

## Testing Checklist

- New user sees the money amount as `0.00$` when no saved data exists.
- Opening a temporary UI while another temporary UI is already open does not open the second temporary UI; the user must close, cancel, save, delete from, finish, or drop/cancel the current temporary UI first.
- An outside click or tap that closes or cancels an open temporary UI does not also activate the control behind it or open another temporary UI from the same click or tap.
- If the same website is open in two tabs or windows in the same browser, a successful saved change in one tab or window automatically updates the other tab or window to the latest saved money amount, `Balance Changes`, and `Savings`.
- If another tab or window has an unsaved temporary UI open when a same-browser storage update arrives, that temporary UI closes like a cancel, unsaved typed input is discarded, nothing saves or deletes from that tab or window, and the latest saved data appears.
- Normal valid saved data does not show `Start again`, `Reset`, `Clear all data`, or a similar all-data delete action.
- Refreshing, closing, or reopening while main money action buttons, an input flow, a delete UI, a `Saving` square action state, or a reorder drag is open silently discards that temporary UI and restores only the latest successfully saved data.
- Refreshing, closing, or reopening while temporary UI is open does not save unsaved typed input, confirm a pending delete, save a pending reorder, create a `Balance Changes` entry, show a message, or show a browser leave warning.
- Showing the default `0.00$` money amount does not create saved browser data by itself.
- The default `0.00$` starting state does not create a `Balance Changes` entry.
- Clicking the main money amount at `0.00$` shows `Add` and `Modify`, but not `Subtract`.
- After a successful add above `0.00$` and below `999,999.99$`, clicking the main money amount shows `Add`, `Subtract`, and `Modify`.
- After a successful `Add` makes the money amount exactly `999,999.99$`, clicking the main money amount shows `Subtract` and `Modify`, but not `Add`.
- After `Modify` sets the money amount to `0.00$`, clicking the main money amount shows `Add` and `Modify`, but not `Subtract`.
- After `Modify` sets the money amount to `999,999.99$`, clicking the main money amount shows `Subtract` and `Modify`, but not `Add`.
- At `0.00$`, main money actions appear as horizontal buttons ordered `Add`, then `Modify`.
- Main money actions appear as horizontal buttons near the main money amount, ordered `Add`, `Subtract`, and `Modify` when all three are visible.
- At `999,999.99$`, main money actions appear as horizontal buttons ordered `Subtract`, then `Modify`.
- Clicking or tapping the main money amount again while the visible main money action buttons are open hides those buttons without changing anything.
- Clicking or tapping outside the main money amount and outside the visible action buttons hides the visible main money action buttons without changing anything.
- Scrolling the dashboard page while the visible main money action buttons are open hides those buttons without changing anything.
- Clicking `Add`, `Subtract`, or `Modify` starts the selected money amount input flow and does not count as an outside click.
- Add money updates the money amount correctly.
- `Add`, `Subtract`, and `Modify` use the same money amount entry flow without requiring a particular shape, position, size, or visual arrangement.
- The money amount entry flow is the active interaction context while open, and other dashboard controls are inactive.
- The money amount input starts at `0.00`.
- The money amount input is focused and ready for typing immediately when it opens, without requiring another tap or click.
- Main money action inputs request a mobile keyboard suitable for digit entry on supported devices.
- Main money action inputs provide an action with the exact visible name `Cent` on mobile and desktop.
- Choosing `Cent` keeps the input active, and the mobile digit-entry keyboard remains available when the browser permits it.
- Typing `5` in a main money action input shows `5.00`.
- Typing `58` in a main money action input shows `58.00`.
- Typing `589` in a main money action input shows `589.00`.
- Typing `5895` in a main money action input shows `5,895.00`.
- Typing `58955` in a main money action input shows `58,955.00`.
- Typing raw digits `589550` in a main money action input shows `589,550.00`.
- Typing `5`, then `Space`, in a main money action input keeps the display at `5.00` and visibly indicates that cents entry is active.
- Typing `5`, then `Space`, then `5` in a main money action input shows `5.05` and saves and shows as `5.05$`.
- Typing `5`, then `Space`, then `50` in a main money action input shows `5.50` and saves and shows as `5.50$`.
- Typing `58430`, then `Space`, then `88` in a main money action input shows `58,430.88` and saves and shows as `58,430.88$`.
- Pressing another `Space` or choosing `Cent` after cents entry has started does not change the input and shows no message.
- Typing `0`, then `Space`, then `5` in a main money action input shows `0.05` and saves and shows as `0.05$`.
- Typing `999999`, then `Space`, then `99` in a main money action input shows `999,999.99` and saves and shows as `999,999.99$`.
- Typing `5`, choosing `Cent`, then typing `5` in a main money action input shows `5.05` and saves and shows as `5.05$`.
- Typing `5`, choosing `Cent`, then typing `50` in a main money action input shows `5.50` and saves and shows as `5.50$`.
- Typing `58430`, choosing `Cent`, then typing `88` in a main money action input shows `58,430.88` and saves and shows as `58,430.88$`.
- Typing `0005` in a main money action input shows `5.00` and saves and shows as `5.00$`.
- Typing `Space`, `Space`, `Space`, then `5` in a main money action input blocks the three `Space` key presses and then shows `5.00` after the `5` is typed.
- Tapping `Cent`, `Cent`, `Cent`, then typing `5` in a main money action input blocks the three `Cent` taps and then shows `5.00` after the `5` is typed.
- Choosing `Cent` or pressing `Space` after cents entry has started does not change the input and shows no message.
- Trying to type a third cents digit after two cents digits does not change the input and shows no message.
- Saving raw digits `589550` in a main money action input stores `589550.00` and shows `589,550.00$`.
- Saving a main money action input stores the money amount without the `$` sign or comma separators and shows the saved money amount with the `$` sign at the end.
- Deleting from `5.50` entered as `5`, `Cent`, `5`, `0` first shows `5.05`, then `5.00`, then removes the accepted `Cent` action and its visible cents-entry indication while leaving the display at `5.00`.
- Deleting all typed numbers in a main money action input returns it to `0.00` instead of making it empty.
- Clicking or tapping inside `58,430.88` keeps the main money action input append-only, so the next accepted typed character is handled at the end.
- Selecting `430` in `58,430.88` and typing `9` does not replace `430`; accepted typing uses append-only behavior.
- Backspace or Delete with the cursor or selection inside the formatted value removes only the last accepted digit or the action that started cents entry.
- Typing letters, minus signs, decimal points, `$` signs, manually typed comma separators, choosing `Cent` or pressing `Space` before any entered digit, choosing `Cent` or pressing `Space` again after cents entry starts, entering a third cents digit, or entering another blocked character does not change the field, and no message appears.
- Choosing `Cent` when cents entry cannot start does not change the field, and no message appears.
- Choosing `Cent` keeps the input active, visibly indicates that cents entry is active, and does not cancel or close the main money amount input flow.
- Trying to type a main money action input value greater than `999,999.99` does not change the field and shows no message.
- Money amounts up to `999,999.99$` and all required controls remain readable and usable in the entry flow, dashboard, `Balance Changes`, and `Savings`, with required comma separators while typing or in visible saved/rendered values.
- Trying to paste letters, numbers, symbols, or any other content into a main money action input does not change the field and shows no message.
- Trying to paste letters, numbers, symbols, or any other content into a `Saving` square planned money amount input does not change the field and shows no message.
- `Add`, `Subtract`, and `Modify` money amount input flows show the exact text `Save Changes` and provide actions exactly named `Cent`, `Yes`, and `Cancel` without requiring a particular visual arrangement.
- Clicking `Yes` applies the selected `Add`, `Subtract`, or `Modify` action when the typed amount is valid for that action and, for `Modify`, different from the money amount already shown.
- After a successful `Add`, `Subtract`, or `Modify` that changes the money amount, the input flow closes, the temporary typed input resets for the next input, the dashboard money amount view returns, the main money action buttons are hidden, the updated money amount is shown, and no message appears.
- After a failed `Add`, `Subtract`, or `Modify` save, `Changes could not be saved.` remains while the flow is unchanged, disappears when the amount changes or the user retries or closes the flow, and appears again if the retry fails.
- Clicking `Cancel` or clicking/tapping outside a main money amount input flow changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
- Clicking or tapping outside the active money amount entry flow acts like `Cancel` and does not reopen the main money action buttons.
- `Saving` square create, `Saving` square rename, `Saving` square planned-money-amount change, and broken `Saving` square fix input flows show `Save` and `Cancel` text actions at the bottom.
- `Saving` square create and broken `Saving` square fix input flows appear as temporary `Saving` input squares inside the `Saving` squares area.
- Trying to confirm `0.00` in `Add` does not change the money amount, does not save data, does not create a `Balance Changes` entry, and keeps the same money amount input step open.
- Trying to confirm `0.00` in `Subtract` does not change the money amount, does not save data, does not create a `Balance Changes` entry, and keeps the same money amount input step open.
- Trying to confirm the same money amount in `Modify` does not change the money amount, does not save data, does not create browser storage, does not run `Balance Changes` cleanup, does not create a `Balance Changes` entry, and keeps the same money amount input step open.
- Subtract money updates the money amount correctly.
- Subtracting more than the current money amount sets the money amount to `0.00$`.
- Subtracting more than the current money amount saves the actual removed amount in history.
- Subtracting when the current money amount is `0.00$` does not create a history entry.
- The website never shows a negative money amount.
- Modify updates the current money amount without creating history or notification entries when the entered money amount is valid and different from the money amount already shown.
- Modify can set the money amount to any different value from `0.00$` through `999,999.99$`.
- Modify to `0.00$` from `0.00$` is a no-op and does not create browser storage.
- Add and subtract entries stay separate in history.
- Newest `Balance Changes` entries appear first.
- After a successful `Add` or `Subtract` creates a new `Balance Changes` entry, the `Balance Changes` scroll position moves to the top so the newest entry is visible.
- A new saved `Add` or `Subtract` entry received from another open tab or window also moves the receiving `Balance Changes` view to the top.
- Empty `Balance Changes` shows no rows, sentence, placeholder, icon, or other empty-state content.
- History does not replace separate entries with only a net result.
- Money amount change history is available in `Balance Changes` on the dashboard without requiring a particular shape, size, position, or scrolling implementation.
- There is no separate `View history` action for money amount change history.
- All valid 30-day `Balance Changes` entries remain reachable when the history is long, with no smaller maximum visible-entry limit.
- The `Savings` action remains reachable whether `Balance Changes` is empty or contains entries.
- During cleanup, `Balance Changes` entries at or after their visible until date are deleted from browser storage.
- `Balance Changes` cleanup runs when the website opens and loads saved data.
- `Balance Changes` cleanup runs after every successful saved user action.
- `Balance Changes` internal visible-until timestamps equal their exact created timestamps plus `2_592_000_000` milliseconds, or exactly 720 hours.
- Expiring old history entries does not change the current money amount.
- `Balance Changes` shows correct differences.
- Visible `Balance Changes` rows show the signed money amount, action text, and both the created date and created time, such as `+56.00$ added` or `-34.00$ subtracted` plus a visible date and time like `July 21, 2026 at 3:45 PM`.
- Visible `Balance Changes` entries are compact, with the signed money amount and action text in the top-left corner and the visible date and time in the bottom-right corner.
- Mobile keeps the same compact `Balance Changes` entry layout and allows wrapping only as needed to keep text readable.
- Visible `Balance Changes` dates and times use the local values recorded when each action succeeds and the fixed English format shown by `September 18, 2026 at 2:30 PM`.
- Visible `Balance Changes` rows do not show previous money amount, new money amount, internal exact created date and time, seconds, milliseconds, or internal visible until date and time.
- Deleting a `Balance Changes` entry removes only that visible history entry and does not change the current money amount.
- Touch users open `Balance Changes` delete by pressing and holding the entry for `600ms`.
- Mouse users open `Balance Changes` delete by clicking and holding the entry for `600ms`.
- Releasing before `600ms`, moving the pointer or finger before `600ms`, or starting to scroll before `600ms` cancels the pending `Balance Changes` delete hold and does not open the little square.
- Opening `Balance Changes` delete after a completed `600ms` hold shows a centered little square with the exact action texts `Delete` and `Cancel`.
- Only one `Balance Changes` delete action square can be open at a time.
- Clicking `Cancel` closes the little square without changing anything.
- Clicking outside the little square closes it without changing anything.
- Pointer or finger movement does not close the little square after it is open.
- Scrolling does not close the little square after it is open.
- Deleting a `Balance Changes` entry asks for confirmation after `Delete` is clicked with `Delete this Balance Change?`, closes the little square, shows `Cancel` and `Delete`, closes the little square or confirmation on browser Back without deleting the entry, closes the confirmation on `Cancel` or outside click without deleting the entry, and does not offer undo.
- Saved `Balance Changes` entries cannot be edited.
- Browser storage saves the current money amount and history.
- Browser storage restores the current money amount and history after refresh.
- Browser storage restores data after closing and reopening the website in the same browser.
- While browser storage is being checked after opening, neither the dashboard nor recovery is shown and the default `0.00$` dashboard does not briefly appear.
- Browser storage changes saved from one open tab or window update other open tabs or windows in the same browser without creating extra history entries, extra saves, or user-facing messages.
- `Start again` appears only when saved browser data is broken or unreadable, not when saved data is valid.
- Open temporary UI is not restored after refresh, close, or later reopen; only latest successfully saved data is restored.
- The first view does not explain where saved data is stored.
- The first view does not warn that saved data may disappear after browser or device changes.
- The default `0.00$` dashboard appears when there is no saved browser data.
- Broken browser storage data shows a clear recovery message.
- Saved browser storage data with a current money amount greater than `999999.99` shows the broken saved-data recovery message.
- Saved browser storage data with a current money amount of `5000.00` loads and shows the money amount as `5,000.00$`.
- Saved browser storage data with a current money amount of `5`, `5.0`, `005.00`, `5,000.00`, or `5.000` shows the broken saved-data recovery message.
- Clicking `Start again` after broken browser storage data replaces it in one successful complete save with the saved money amount set to `0.00`, empty `Balance Changes`, no saved `Saving` squares, and data version `1`, without deleting the storage key first.
- A failed `Start again` keeps `Changes could not be saved.` until retry or recovery closes, shows it again if retry fails, and does not restore it in a later new recovery state.
- If saved browser data has one or more broken saved `Balance Changes` entries but the rest of the data can be read, the website removes every broken history entry independently, keeps every usable entry, does not change the current money amount, and shows no error message.
- If saved browser data has one broken saved `Saving` square but the rest of the data can be read, the website loads the rest of the data and shows that square as `Saving could not be loaded.` with `Fix` and `Delete`.
- A broken saved `Saving` square with missing name, duplicate name, invalid order, duplicate order, invalid planned money amount, planned money amount of `0.00$` or less, or planned money amount greater than `999,999.99$` does not affect `Savings money amount`, top needed text, or valid square coverage calculations.
- A broken saved `Saving` square cannot be reordered, stays locked in its displayed position until fixed or deleted, and does not prevent normal default `Saving` squares from being reordered.
- Fixing a broken saved `Saving` square replaces the broken square with a temporary `Saving` input square in the same visible position, asks for a valid `Saving` name and planned money amount greater than `0.00$` and not greater than `999,999.99$`, saves browser storage, turns the broken square into a normal square in the same visible position when possible, recalculates `Savings`, and creates no `Balance Changes` entry.
- Deleting a broken saved `Saving` square uses the small centered `Saving` delete confirmation, removes only that broken square after confirmation, saves browser storage, recalculates `Savings`, and creates no `Balance Changes` entry.
- The website works whether the user updates money rarely or many times in one day.
- `Saving` squares can be created, renamed, updated, and deleted.
- `Saving` square rename starts by opening the square's action state and clicking the `Saving` name in the square.
- `Saving` square planned-money-amount change starts by opening the square's action state and clicking the planned money amount in the square.
- `Saving` square rename replaces that same square with a temporary `Saving` input square in the same visible position.
- `Saving` square planned-money-amount change replaces that same square with a temporary `Saving` input square in the same visible position.
- `Saving` square delete starts by opening the square's action state and clicking `Delete` at the bottom center of the square.
- `Saving` square actions do not open from a separate action menu or larger action square.
- Trying to save a new `Saving` square without a name does nothing, keeps the create flow open, and does not change saved data.
- A `Saving` square can be created with a one-letter name.
- A `Saving` square can be created with a number-only name such as `2026`.
- A `Saving` square can be created with numbers before or after words, such as `2 Trip` or `Trip 2`.
- A `Saving` square can be created with multiple words or a full sentence as its name.
- A `Saving` square can be created with symbols, punctuation, or emoji characters in its name.
- A very long `Saving` square name is not rejected only because of its length.
- A very long `Saving` square name stays contained in the square layout without horizontal page overflow or overlapping other square content.
- Trying to save a new `Saving` square without a planned money amount does nothing, keeps the create flow open, and does not change saved data.
- Trying to rename a `Saving` square with an empty name does nothing, keeps the rename flow open, and does not change saved data.
- Trying to change a `Saving` square planned money amount with an empty planned money amount does nothing, keeps the change flow open, and does not change saved data.
- `Saving` square planned money amount inputs show raw decimal number text while typing, do not show a `Cent` button, block `Space` with no message, and format with two decimal digits, comma separators when needed, and the `$` sign only after `Save`.
- Creating, renaming, or fixing a `Saving` square with a duplicate name returns to the `Saving` squares view with no duplicate-name error message and no saved data changes.
- Visible broken saved `Saving` squares with readable non-empty saved names reserve those names until fixed or deleted, while a broken square being fixed does not block itself.
- Broken-square fix checks planned money amount first, missing `Saving` name second, and duplicate `Saving` name third.
- Creating a `Saving` square with a duplicate name and a missing or `0.00$` planned money amount keeps the same `Saving` square create input step open with no message and no saved data change.
- Default `Saving` squares show their thin coverage bars.
- `Saving` squares appear in one vertical column on mobile and desktop.
- Clicking a `Saving` square opens its action state.
- The `Saving` square action state hides the thin coverage bar and shows `Delete` at the bottom center.
- Clicking outside the open `Saving` square action state closes it and returns the square to its default state without changing saved data.
- Clicking inside the open `Saving` square action state does not close it as an outside click.
- Clicking a blank part inside the open `Saving` square action state does nothing and keeps that square in action state.
- Touch scrolling over a `Saving` square by moving the finger more than `8px` before `600ms` does not open action state or start reorder.
- Clicking another normal default `Saving` square while one `Saving` square is already in action state closes the old action state and does not open the clicked square from that same click.
- Scrolling does not close the open `Saving` square action state.
- Clicking `<` clears the open `Saving` square action state while returning to the dashboard.
- Touch users can reorder `Saving` squares by holding a normal default square for `600ms`, then moving the whole square after at least `8px` of finger movement.
- Mouse users can reorder `Saving` squares by clicking and holding a normal default square for `600ms`, then dragging the whole square after at least `8px` of pointer movement.
- Holding a normal default `Saving` square for `600ms` and releasing before moving at least `8px` does nothing.
- Dragging within `40px` of the top or bottom edge of the scrollable `Saving` squares area auto-scrolls at a fixed `8px` per animation frame with the same behavior at both edges.
- Interrupting a `Saving` square reorder before drop cancels the reorder, returns the square to its original position, removes the placeholder, stops auto-scroll, saves nothing, creates no `Balance Changes` entry, and shows no message.
- Broken saved `Saving` squares cannot be dragged or reordered and stay locked in their displayed positions while normal default squares are reordered.
- Holding or dragging a `Saving` square does not open rename, planned-money-amount change, or delete.
- Moving the last normal `Saving` square to the top makes that square checked first, saves the final normal square order, and recalculates coverage bars from the new visible order of normal `Saving` squares, skipping broken saved `Saving` squares.
- Clicking `Savings` opens the Savings section.
- Using browser Back while full-screen `Savings` is open closes an open `Saving` square input flow or delete confirmation first without changing saved data; using Back again closes `Savings` and returns to the dashboard when no `Saving` square input flow or delete confirmation is open.
- The money amount shown at the top of `Savings` is labeled `Savings money amount`.
- With enough `Saving` squares to scroll, the `Savings money amount` stays visible at the top while the squares scroll.
- `Savings money amount` uses the same visual style at `0.00$` as it uses for other values.
- With `100.00$` main money amount and `120.00$` total planned money amount in valid `Saving` squares, the top `Savings` area shows `20.00$ needed`.
- With `100.00$` main money amount and `100.00$` total planned money amount in valid `Saving` squares, the top `Savings` area does not show a top needed text.
- The main money amount is still labeled `Current Balance`.
- Creating a `Rent` `Saving` square with a planned money amount of `40.00$` changes the money amount shown inside `Savings` from `350.00$` to `310.00$` when the main money amount is `350.00$`.
- Creating a `Saving` square with a planned money amount does not change the main money amount.
- Trying to save a new `Saving` square with a `0.00$` planned money amount does not create a square, does not save data, does not create a `Balance Changes` entry, shows no message, and keeps the same `Saving` square create input step open.
- Changing `Rent` from `40.00$` to `60.00$` makes `Rent` show `60.00$`, not `100.00$`.
- Changing `Rent` from `40.00$` to `10.00$` makes `Rent` show `10.00$`, not `30.00$`.
- Changing `Rent` from `40.00$` to `0.00$` removes `Rent` from `Savings`.
- Changing a `Saving` square planned money amount to `0.00$` removes that square from browser storage.
- Changing a `Saving` square planned money amount does not create a `Balance Changes` entry.
- Deleting `Rent` removes only `Rent` and its saved details, does not change the main money amount, and leaves the other `Saving` squares unchanged.
- Deleting a `Saving` square asks for confirmation first in a small centered confirmation square with `Delete this Saving?`, shows `Cancel` and `Delete`, closes on `Cancel` or outside click with no saved data changes, and does not offer undo.
- The add-a-saving action appears as a circle with a `+` sign.
- When `Savings` has no visible `Saving` squares, the circle `+` action appears in the middle of the `Saving` squares area.
- The no-`Saving`-squares empty state uses the centered circle `+` action only when no normal or broken saved `Saving` squares are visible, and does not show a separate empty-state text sentence.
- After at least one normal `Saving` square exists, the circle `+` action appears at the top-left of the `Saving` squares area.
- If only broken saved `Saving` squares are visible, the circle `+` action appears at the top-left above or before those broken squares.
- Clicking the circle `+` action opens the same create flow from the centered empty state and from the top-left non-empty state.
- Clicking the circle `+` action replaces that `+` with a temporary `Saving` input square instead of opening a modal, bottom sheet, or separate page.
- Default `Saving` squares show thin horizontal coverage bars at the bottom.
- With `85.00$` main money amount and ordered `Saving` squares of `Rent` `50.00$`, `Food` `20.00$`, and `School` `30.00$`, `Rent` and `Food` show full green bars, `School` shows the left 50% of its bottom bar as green and the right 50% as grey, `School` shows `15.00$ needed` at the top-left of its bottom coverage bar, and the money amount shown inside `Savings` is `0.00$`.
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
