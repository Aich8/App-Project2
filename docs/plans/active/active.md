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
- Start with the money amount shown as `0.00$` when no saved data exists.
- Click the main money amount at `0.00$` to show `Add` and `Modify`, but not `Subtract`.
- Add money manually while the money amount is less than `999,999.99$`, use `Subtract` after the money amount is greater than `0.00$`, and see all three actions when the money amount is greater than `0.00$` and less than `999,999.99$`.
- Use `Subtract` and `Modify`, without `Add`, when the money amount is exactly `999,999.99$`.
- Correct mistakes with `Modify` at any valid money amount from `0.00$` through `999,999.99$`.
- See 30-day visible money amount change history directly under the main money amount.
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
- No saved website settings in the first version because no settings exist yet.
- Add money flow with amount.
- Subtract money flow with amount.
- Silent modify flow with corrected total amount.
- 30-day visible money amount change history shown directly under the main money amount, where each add and subtract action stays as its own entry.
- Newest `Balance Changes` entries shown first.
- Full-screen `Savings` planning view with a money amount shown inside `Savings` using the label `Savings money amount`, a fixed top `Savings` area while `Saving` squares scroll, normal visual styling when `Savings money amount` is `0.00$`, small top `{money amount} needed` text when total planned money amount in valid `Saving` squares is greater than the main money amount, ordered user-named `Saving` squares in one vertical column on mobile and desktop, coverage bars, a centered circle `+` empty state when no `Saving` squares exist, a top-left circle `+` action when `Saving` squares exist, temporary `Saving` input squares for create and broken-square fix, saved-data-only `Savings money amount`, top needed text, and coverage updates while `Saving` input flows are open, and a top-left `<` back action to return to the dashboard.
- `Balance Changes` history for added money and subtracted money.
- Saved `Balance Changes` entry validation that removes only broken saved history entries while keeping the rest of the saved data.
- Delete support for `Balance Changes` entries that opens from press-and-hold or click-and-hold, shows a little square with `Delete` and `Cancel`, asks for confirmation with `Delete this Balance Change?` after `Delete` is chosen, shows `Cancel` and `Delete` in the confirmation, removes only the visible history entry, and does not offer undo.
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
- Main money amount area: user-facing label `Current Balance`; clicking the money amount reveals horizontal action buttons near the money amount, with `Add` and `Modify` in that order at `0.00$`, `Add`, `Subtract`, and `Modify` in that order when the money amount is greater than `0.00$` and less than `999,999.99$`, or `Subtract` and `Modify` when the money amount is exactly `999,999.99$`.
- Money amount change history directly under the main money amount.
- Secondary action: click `Savings`.
- Overview area: `Balance Changes` and savings summary.

## Core Data Model

Money amount:
- Money amount.
- Supports cents from `0.00$` through `999,999.99$` as each money amount rule allows.
- Store and calculate money amount values with an exact normalized plain decimal representation so typed values like `5 Space 5` or mobile `5 Cent 5` that display as `5.05`, `58 Space 430 Space 88` or mobile `58 Cent 430 Cent 88` that display as `58,430.88`, and raw digit sequences like `589550` that display as `589,550.00`, keep the parsed money amount exact.
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
- ID.
- Name.
- Planned money amount greater than `0.00$` and not greater than `999,999.99$`.
- Order.
- Created date.
- Updated date.

Broken saved `Saving` square:
- Exact text: `Saving could not be loaded.`
- Actions: `Fix` and `Delete`.
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
- Define broken saved `Balance Changes` entry handling so one broken history entry is removed without breaking the whole saved browser data file.
- Define broken saved `Saving` square handling so one broken square does not break the whole saved browser data file.

Acceptance criteria:
- The website opens in a browser.
- The layout works on desktop and mobile widths.
- Data can be saved in browser storage and restored after refresh.
- Data can be restored after closing and reopening the website in the same browser.
- If no saved data exists, the website shows the dashboard with the money amount set to `0.00$`.
- Showing the default `0.00$` money amount does not create saved browser data by itself.
- Saved browser data with a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, shows `Saved data could not be loaded.` and a `Start again` action.
- If saved browser data has one broken saved `Balance Changes` entry but the rest of the data can be read, the website loads the rest of the saved data and removes only that broken history entry.
- The first view does not explain where saved data is stored.

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
- Start the selected money amount input flow when the user clicks `Add`, `Subtract`, or `Modify`; these action-button clicks should not be treated as outside clicks.
- Hide the visible main money action buttons when the selected money amount input flow starts, remember the selected action internally only, and do not show a visible `Add`, `Subtract`, or `Modify` reminder inside the open input flow.
- Show money amount change history directly under the main money amount.
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
- Clicking `Add`, `Subtract`, or `Modify` starts the selected money amount input flow instead of hiding the visible money actions as an outside click.
- When the selected money amount input flow starts, the visible main money action buttons are hidden and the open input flow does not show a visible `Add`, `Subtract`, or `Modify` reminder.
- Money amount change history appears directly under the main money amount.
- There is no separate `View history` action for money amount change history.
- The interface does not use misleading bank wording.

### Milestone 3: Manual Money Amount Changes

Tasks:
- Build the default `0.00$` starting state.
- Build add money flow.
- Build subtract money flow.
- Build silent modify/correct money amount flow.
- Validate amount inputs.
- Render `Add`, `Subtract`, and `Modify` money amount inputs as a horizontal square in the middle of the screen.
- Keep the main money amount circle visible in the background while the horizontal input square is open.
- Start the horizontal input square at `0.00`.
- Focus the horizontal input square immediately when it opens so the user can start typing without another tap or click, and request the mobile keyboard immediately on supported mobile devices.
- Request a mobile keyboard suitable for digit entry for main money action inputs on supported devices.
- Show a rectangular `Cent` button for all users while a main money action input is open. The `Cent` button should be part of the main money amount input controls. On mobile, it should appear directly above the mobile keyboard when possible.
- Let the user type numbers from `0` through `9` and the `Space` key into main money action inputs, without typing the `$` sign. Let all users choose the `Cent` button for cents. Block typed decimal points and manually typed comma separators with no message. Add the decimal point and comma separators automatically in the displayed input.
- Use `Space` and `Cent` as non-visible separators between digit groups. Accept separator input only after at least one digit and only when the previous accepted input is a digit. Block starting separators and consecutive separators with no message.
- Automatically format typed main money action input values with two digits after the decimal point, automatic comma separators for thousands and larger values, and no `$` sign while the user is typing. Without separator input, typed digits are whole money amount digits: typing `5` shows `5.00`, `58` shows `58.00`, `589` shows `589.00`, `5895` shows `5,895.00`, `58955` shows `58,955.00`, and `589550` shows `589,550.00`.
- For accepted input with separator input from `Space` or `Cent`, split the accepted input into digit groups at accepted separators. If the input ends with a separator, treat all completed groups as the whole money amount and show cents as `00`. If the final group after a separator has one or two digits, treat that final group as cents and left-pad one cents digit with `0`; join all earlier groups as the whole money amount. If there is only one accepted separator and the final group grows to three or more digits, treat that final group as another whole money amount group and show cents as `00`. Once the input has two accepted separators, the final group is the cents group and should accept at most two digits. Block additional separator input after two accepted separators with no message.
- Treat `0` as a normal digit, not a starting zero-position skip. Normalize away unneeded leading zeros in the whole money amount, so typing `0005` shows `5.00`.
- Reformat main money action inputs after deletion with two digits after the decimal point and no `$` sign while the input is still open.
- Return main money action inputs to `0.00` when all typed numbers are deleted, without letting the input become empty.
- Make main money action inputs append-only: focus can open the input, but cursor movement inside the formatted money amount, partial selection, selection replacement, and direct editing of generated comma separators or the generated decimal point are not supported. Accepted typing adds to the end, and delete removes only the last accepted digit or accepted separator.
- Block letters, minus signs, decimal points, `$` signs, manually typed comma separators, starting separators, consecutive separators, additional separator input after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, and paste in main money action inputs with no message.
- Block typed characters that would make a main money action input greater than `999,999.99`, with no message and no field change.
- Keep money amounts up to `999,999.99$` contained in the horizontal input square, dashboard, `Balance Changes`, and `Savings` displays without horizontal page overflow, text overlap, hidden actions, rejected valid values, or missing required comma separators while typing or in visible saved/rendered values.
- Show `Save Changes` under the horizontal input square, with buttons exactly named `Yes` and `Cancel`.
- Make `Yes` apply the selected `Add`, `Subtract`, or `Modify` action.
- After a successful `Add`, `Subtract`, or `Modify` that changes the money amount, close the horizontal input square, reset the temporary typed input so the next main money amount input starts at `0.00`, return to the dashboard money amount view, hide the main money action buttons, show the updated money amount, save only the data required by that action, and show no message.
- Make `Cancel` and outside click or tap close the main money amount input flow, return to the dashboard money amount view, and change nothing.
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
- `Add`, `Subtract`, and `Modify` money amount input flows show a horizontal input square in the middle of the screen while the main money amount circle stays visible in the background.
- The horizontal input square starts at `0.00`.
- The horizontal input square is focused and ready for typing immediately when it opens.
- `Add`, `Subtract`, and `Modify` money amount input flows show a rectangular `Cent` button for all users.
- On mobile, the `Cent` button appears directly above the mobile keyboard when possible.
- Typing `5` in a main money action input shows `5.00`, typing `58` shows `58.00`, typing `589` shows `589.00`, typing `5895` shows `5,895.00`, typing `58955` shows `58,955.00`, and typing raw digits `589550` shows `589,550.00`.
- Typing `5`, then `Space`, keeps the display at `5.00`; typing `5`, then `Space`, then `5` shows `5.05`; typing `5`, then `Space`, then `50` shows `5.50`; typing `58`, then `Space`, then `430` shows `58,430.00`; typing `58`, then `Space`, then `430`, then `Space`, then `88` shows `58,430.88`; and typing `999999`, then `Space`, then `99` shows `999,999.99`.
- Typing `5`, choosing `Cent`, then typing `5` shows `5.05`; typing `5`, choosing `Cent`, then typing `50` shows `5.50`; and typing `58`, choosing `Cent`, typing `430`, choosing `Cent`, then typing `88` shows `58,430.88`.
- Typing `0005` in a main money action input shows `5.00`.
- Typing `Space`, `Space`, `Space`, then `5` in a main money action input blocks the three `Space` key presses and then shows `5.00` after the `5` is typed.
- Saving a main money action input stores the money amount as a normalized plain decimal string without the `$` sign or comma separators, such as `5.00`, `5.05`, `5895.50`, or `999999.99`, and shows the money amount with the `$` sign at the end and required comma separators for thousands and larger values, such as `5.00$`, `5.05$`, `5,895.50$`, or `999,999.99$`.
- Deleting one typed character from a main money action input reformats the remaining typed value as a money amount.
- Deleting all typed numbers in a main money action input returns the horizontal input square to `0.00` instead of making it empty.
- Clicking or tapping inside a main money action input does not move the cursor into the middle of the formatted money amount.
- Selecting part of a main money action input and typing does not replace the selected text; accepted typing is added to the end.
- Backspace or Delete in a main money action input removes only the last accepted digit or accepted separator, not generated comma separators or the generated decimal point.
- Main money action inputs keep their previous value when the user types letters, minus signs, decimal points, `$` signs, manually typed comma separators, starting separators, consecutive separators, additional separator input after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, or other blocked characters.
- Tapping `Cent` when a separator would be blocked keeps the previous input value and shows no message.
- Main money action inputs accept valid money amounts up to `999,999.99` and block typed characters that would make the value greater than `999,999.99`.
- Money amounts up to `999,999.99$` remain visible or editable without horizontal page overflow, overlapping content, hidden controls, or missing required comma separators while typing or in saved/rendered visible values.
- If the user tries to paste letters, numbers, symbols, or any other content into a main money action input, the pasted content does not appear, the input keeps its previous value, and no message is shown.
- `Add`, `Subtract`, and `Modify` money amount input flows show `Save Changes`, `Yes`, and `Cancel`.
- `Cancel` in `Add`, `Subtract`, or `Modify` closes the money amount input flow, returns to the dashboard money amount view, changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
- Clicking or tapping outside the horizontal input square, `Save Changes`, `Yes`, `Cancel`, and the `Cent` button acts like `Cancel`.
- Tapping or clicking the `Cent` button behaves like pressing `Space` and does not cancel or close the main money amount input flow.
- `Add`, `Subtract`, and `Modify` do not save invalid money amounts. Negative money amounts and above-limit money amounts are blocked, `Add` and `Subtract` require more than `0.00$` and not greater than `999,999.99$`, and `Modify` allows `0.00$` through `999,999.99$`.
- Clicking `Yes` while the input is `0.00` in `Add` or `Subtract` does nothing: no message, no money amount change, no saved data change, no `Balance Changes` entry, and the same money amount input step stays open.
- Clicking `Yes` in `Modify` with the same money amount that is already shown does nothing: no message, no money amount change, no saved data change, no browser storage creation or update, no `Balance Changes` entry, no `Balance Changes` cleanup, and the same money amount input step stays open until the user enters a different valid money amount or cancels.
- Clicking `Yes` while the input is `0.00` in `Modify` replaces the main money amount with `0.00$` only when the current money amount is greater than `0.00$`.
- Modifying the money amount to `0.00$` makes the next main money amount click show `Add` and `Modify`, but not `Subtract`.
- Modifying the money amount to `999,999.99$` makes the next main money amount click show `Subtract` and `Modify`, but not `Add`.

### Milestone 4: Balance Changes

Tasks:
- Build the `Balance Changes` list directly under the main money amount.
- When no `Balance Changes` entries exist, show no history rows and no empty-state sentence, placeholder, icon, or other empty-state content.
- Show each visible `Balance Changes` row with the signed money amount, action text, and both the created date and created time: `+{money amount} added` or `-{money amount} subtracted`, plus the visible created date and created time. A date-only display is not enough. The visible date and time format should be like `July 21, 2026 at 3:45 PM`.
- Render each visible `Balance Changes` entry as a compact entry box, not a large panel.
- Place the signed money amount and action text in the top-left corner of each visible `Balance Changes` entry.
- Place the visible created date and created time in the bottom-right corner of the same entry, below the money change text and not too far from it.
- Keep the same `Balance Changes` entry layout on mobile. Text may wrap only as needed to stay readable, but the money change text should remain in the top-left area and the visible date and time should remain in the bottom-right area.
- Use the user's browser/device local date and time for visible `Balance Changes` created dates and times.
- Save an internal exact created date and time with seconds and milliseconds for each `Balance Changes` entry, but do not show seconds or milliseconds to the user.
- Do not show the previous money amount, new money amount, internal exact created date and time, or internal visible until date and time in visible `Balance Changes` rows.
- Keep each add and subtract action as its own visible entry.
- Show newest `Balance Changes` entries first by internal exact created date and time, with older changes lower in the list.
- If two `Balance Changes` entries have the exact same internal exact created date and time, use saved list order as the tie-breaker, with entries earlier in the saved list appearing first.
- Do not ask the user to choose a date for `Add` or `Subtract`.
- Do not replace separate entries with only a combined net result.
- Calculate each `Balance Changes` internal visible until date and time as the internal exact created date and time plus 30 days using the user's browser/device local date and time.
- Run `Balance Changes` cleanup when the website opens and loads saved data.
- Run `Balance Changes` cleanup after every successful saved user action.
- During cleanup, delete visible history entries from browser storage at or after their visible until date.
- Do not add a background timer for checking old `Balance Changes` entries while the website stays open with no user action.
- Keep the current money amount unchanged when old history entries expire.
- Let the user scroll down when the history list is longer than the screen.
- Add delete support for `Balance Changes` entries.
- For touch users, open the delete action when the user presses and holds a `Balance Changes` entry.
- For mouse users, open the delete action when the user clicks and holds a `Balance Changes` entry.
- After press-and-hold or click-and-hold, show a little square on the screen with the exact action texts `Delete` and `Cancel`.
- Make clicking `Cancel` in the little square close it without changing anything.
- Make clicking `Delete` in the little square ask for confirmation with `Delete this Balance Change?`, show `Cancel` and `Delete`, remove only the visible history entry after confirmation, not change the current money amount, and not offer undo.

Acceptance criteria:
- The user can understand how their money amount changed over time.
- When there are no `Balance Changes` entries, the history list is empty and shows no sentence, placeholder, icon, or other empty-state content.
- The user sees separate entries such as `+56.00$ added` and `-34.00$ subtracted`.
- Visible `Balance Changes` rows show both the created date and created time using the format `July 21, 2026 at 3:45 PM`.
- Visible `Balance Changes` entries are compact, with the money change text at the top left and the visible date and time at the bottom right.
- Mobile keeps the same `Balance Changes` entry layout, with readable wrapping if needed.
- Visible `Balance Changes` dates and times use the user's browser/device local date and time.
- Visible `Balance Changes` rows do not show previous money amount, new money amount, internal exact created date and time, seconds, milliseconds, or internal visible until date and time.
- The newest `Balance Changes` entry appears first by internal exact created date and time, and older changes go lower.
- If two `Balance Changes` entries have the exact same internal exact created date and time, saved list order decides which one appears first.
- `Balance Changes` does not show only a combined result such as `+22.00$ net change`.
- `Balance Changes` cleanup runs when the website opens and loads saved data.
- `Balance Changes` cleanup runs after every successful saved user action.
- During cleanup, entries at or after their visible until date are deleted from browser storage and no longer shown.
- Removing old history entries does not change the current money amount.
- The user can scroll down to see more history entries when needed.
- Touch users can open a `Balance Changes` entry delete action by pressing and holding the entry.
- Mouse users can open a `Balance Changes` entry delete action by clicking and holding the entry.
- Press-and-hold or click-and-hold shows a little square with the exact action texts `Delete` and `Cancel`.
- Clicking `Cancel` in the little square closes it without changing anything.
- Deleting an entry asks for confirmation after `Delete` is clicked with `Delete this Balance Change?`, shows `Cancel` and `Delete`, removes only the visible history entry, does not change the current money amount, and does not offer undo.
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
- Make clicking `<` return to the dashboard without changing anything, saving anything, creating a `Balance Changes` entry, or updating dates.
- In the default `Saving` square state, show the `Saving` square name in the top-left corner, the planned money amount on the right side of the same top row, and the thin coverage bar at the bottom.
- Make clicking a `Saving` square change that same square into its action state.
- In the `Saving` square action state, keep the `Saving` name at the top left and planned money amount on the right side of the same top row, show `Delete` at the bottom center, and hide the thin coverage bar.
- In the action state, make clicking the `Saving` square name open rename.
- In the action state, make clicking the planned money amount open planned-money-amount change.
- In the action state, make clicking `Delete` start delete confirmation.
- Do not use a separate action menu or larger action square for rename, planned-money-amount change, and delete.
- Let touch users reorder by holding and moving the whole `Saving` square.
- Let mouse users reorder by clicking, holding, and dragging the whole `Saving` square.
- Make holding or dragging a `Saving` square avoid opening rename, planned-money-amount change, or delete.
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
- Treat the visible `Saving` square order as the coverage order.
- Add thin horizontal coverage bars at the very bottom of `Saving` squares in their default state.
- Show full green bars for fully covered `Saving` squares, left-to-right partly green bars for partly covered squares, and grey bars for uncovered squares.
- Show a `{money amount} needed` note at the top-left of the bottom coverage bar when a `Saving` square is not fully covered.
- Build the add-a-saving action as a circle with a `+` sign.
- When no visible `Saving` squares exist, show the circle `+` action in the middle of the `Saving` squares area as the empty state.
- Do not show a separate empty-state text sentence for no `Saving` squares.
- After at least one `Saving` square exists, place the circle `+` action at the top-left of the `Saving` squares area, above or before the visible squares.
- Make the circle `+` action open the same new `Saving` square create flow from both positions.
- Make the create flow replace the clicked circle `+` with a temporary `Saving` input square inside the `Saving` squares area.
- If the clicked `+` was centered, render the temporary create input square centered in the `Saving` squares area.
- If the clicked `+` was at the top-left, render the temporary create input square at the top-left before the existing squares.
- Do not render the create flow as a modal, bottom sheet, or separate page.
- Do not render a second create `+` action while the temporary create input square is open.
- Require the user to enter a `Saving` name and planned money amount before creating the `Saving` square.
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
- For new `Saving` square creation, validate in this order: planned money amount missing, `0.00$`, or greater than `999,999.99$`; missing `Saving` name; then duplicate `Saving` name.
- Return to the `Saving` squares view with no changes when creating or renaming a `Saving` square with a duplicate name.
- Make delete ask for confirmation with `Delete this Saving?`, show `Cancel` and `Delete`, remove only the selected `Saving` square and its saved details, and not offer undo.
- Build planned-money-amount editing flows that replace the old planned money amount with a new total planned money amount.
- Remove a `Saving` square if its planned money amount becomes `0.00$`.
- Save `Saving` square changes to browser storage.
- Render a broken saved `Saving` square as an error square with exact text `Saving could not be loaded.` and actions `Fix` and `Delete`.
- Keep valid saved data loaded when only one saved `Saving` square is broken.
- Exclude broken saved `Saving` squares from `Savings money amount`, top needed text, and coverage calculations until fixed.
- Make `Fix` replace the broken square with a temporary `Saving` input square in the same visible position.
- Make the broken-square fix input square ask for a valid `Saving` name and planned money amount greater than `0.00$` and not greater than `999,999.99$`.
- Do not render the broken-square fix input square as a modal, bottom sheet, or separate page.
- Make a successful broken-square fix save browser storage, turn the broken square into a normal `Saving` square, keep it in the same visible position when possible, give it a valid unique ID and valid order if needed, recalculate the `Savings money amount`, top needed text, and coverage bars, create no `Balance Changes` entry, and show no message.
- Make broken-square `Delete` ask for confirmation with `Delete this Saving?`, show `Cancel` and `Delete`, remove only that broken square, save browser storage, create no `Balance Changes` entry, and offer no undo.
- Save the final `Saving` square order to browser storage after the user finishes moving a square and lets go.
- Recalculate the money amount shown inside `Savings`, top needed text, and coverage bars after the final reorder.

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
- Rename, planned-money-amount change, and delete do not open from a separate action menu or larger action square.
- The user can add a `Saving` square with a circle `+` action.
- When no visible `Saving` squares exist, the circle `+` action appears in the middle of the `Saving` squares area.
- The no-`Saving`-squares empty state uses the centered circle `+` action and does not show a separate empty-state text sentence.
- After at least one `Saving` square exists, the circle `+` action appears at the top-left of the `Saving` squares area.
- Clicking the circle `+` action opens the same create flow from the centered empty state and from the top-left non-empty state.
- Clicking the circle `+` action replaces that `+` with a temporary `Saving` input square inside the `Saving` squares area.
- The temporary create input square appears centered when opened from the centered empty-state `+`, and appears at the top-left before existing squares when opened from the top-left `+`.
- `Saving` square create does not open as a modal, bottom sheet, or separate page.
- A new `Saving` square is created only after the user enters a valid name and a planned money amount greater than `0.00$` and not greater than `999,999.99$`.
- If the user tries to save a new `Saving` square without a `Saving` name, nothing happens: no message appears, no square is created, saved data stays unchanged, and the create flow stays open until the user enters a name or cancels.
- If the user tries to save a new `Saving` square without a planned money amount, nothing happens: no message appears, no square is created, saved data stays unchanged, and the create flow stays open until the user enters a planned money amount or cancels.
- If the user tries to rename a `Saving` square with an empty name, nothing happens: no message appears, the old name stays saved, saved data stays unchanged, and the rename flow stays open until the user enters a name or cancels.
- If the user tries to change a `Saving` square planned money amount with an empty planned money amount or a planned money amount greater than `999,999.99$`, nothing happens: no message appears, the old planned money amount stays saved, saved data stays unchanged, and the change flow stays open until the user enters a valid planned money amount or cancels.
- `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows show bottom text actions `Save` and `Cancel`.
- `Cancel` in `Saving` square create, rename, planned-money-amount change, or broken-square fix closes the input flow, returns to the `Saving` squares view, changes nothing, saves nothing, creates no `Balance Changes` entry, updates no dates, and shows no message.
- If the user enters a duplicate name while creating a `Saving` square, the website returns to the `Saving` squares view, shows no duplicate-name error message, creates no new `Saving` square, and keeps saved data unchanged.
- If the user enters a duplicate name while renaming a `Saving` square, the website returns to the `Saving` squares view, shows no duplicate-name error message, keeps the old name, and keeps saved data unchanged.
- Duplicate `Saving` square name checks use trimmed names and ignore uppercase or lowercase differences.
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
- Touch users can reorder `Saving` squares by holding and moving the whole square.
- Mouse users can reorder `Saving` squares by clicking, holding, and dragging the whole square.
- Holding or dragging a `Saving` square does not open rename, planned-money-amount change, or delete.
- The visible `Saving` square order is the coverage order.
- A `Saving` square moved to the top is checked first and its coverage bar is calculated before lower squares use what remains.
- Reordering `Saving` squares saves the final order after the user lets go.
- Reordering `Saving` squares updates the money amount shown inside `Savings`, top needed text, and coverage bars from the new visible order.
- Reordering `Saving` squares does not change the main money amount and does not create a `Balance Changes` entry.
- Coverage bars fill green from left to right by the covered percentage of each `Saving` square.
- A `Saving` square that is 80% covered shows the left 80% of the coverage bar as green and the right 20% as grey.
- A not fully covered `Saving` square shows its `{money amount} needed` note at the top-left of the bottom coverage bar.
- The thin bottom coverage bar is hidden while a `Saving` square is in its action state.
- A broken saved `Saving` square shows `Saving could not be loaded.` with `Fix` and `Delete`.
- A broken saved `Saving` square does not lower the `Savings money amount`, does not affect top needed text, does not show a coverage bar, and does not affect valid square coverage calculations.
- Fixing a broken saved `Saving` square starts by replacing that broken square with a temporary `Saving` input square in the same visible position.
- Broken-square fix does not open as a modal, bottom sheet, or separate page.
- Saving a fixed broken `Saving` square turns it into a normal `Saving` square, keeps it in the same visible position when possible, and creates no `Balance Changes` entry.
- Deleting a broken saved `Saving` square removes only that broken square and creates no `Balance Changes` entry.
- If the total planned money amount in `Saving` squares is greater than the main money amount, the money amount shown inside `Savings` is `0.00$`.
- If the total planned money amount in valid `Saving` squares is greater than the main money amount, the top `Savings` area shows the difference as `{money amount} needed`.
- When the money amount shown inside `Savings` is `0.00$`, it looks like a normal `Savings money amount` value, not a warning or error.
- The user opens the Savings section by clicking `Savings`, not by using a separate `Open savings` action.
- `Savings` opens as a full-screen view with a small `<` sign in the top-left corner.
- Clicking `<` returns to the dashboard without data changes, saved changes, `Balance Changes` entries, or date updates.
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
- Test `Balance Changes` delete opening behavior for touch press-and-hold and mouse click-and-hold, including the little `Delete` and `Cancel` square, `Cancel` close behavior, and the exact confirmation message `Delete this Balance Change?` with buttons `Cancel` and `Delete`.
- Confirm out-of-scope banking features are not implied.
- Move this plan from `docs/plans/active` to `docs/plans/done` after completion.

Acceptance criteria:
- A user can open the website, see `0.00$`, add cash, subtract cash, create a `Saving` square, and review `Balance Changes`.
- No real banking language or fake account behavior is present.
- The MVP satisfies the business spec's first-version goals.

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
16. Website shows money amount change history directly under the main money amount.
17. Website saves the updated data in browser storage.
17. User reviews `Saving` squares.
18. User can close the website and open it again later to see saved data when it is available.

## Validation Rules

- Main money action input amounts may include cents and must use valid money amount text.
- Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`, and should use comma separators every three digits before the decimal point for values of `1,000.00$` or more, such as `5,895.50$` and `999,999.99$`.
- Main money action inputs should start at `0.00`.
- Main money action inputs should focus immediately when they open so the user can start typing without another tap or click.
- Main money action inputs should request a mobile keyboard suitable for digit entry on supported devices.
- Main money action inputs should show a rectangular `Cent` button for all users while a main money action input is open. On mobile, the `Cent` button should appear directly above the mobile keyboard when possible.
- Main money action inputs should accept only numbers from `0` through `9` and separator input from the `Space` key or `Cent` button, without letting the user type the `$` sign. Typed decimal points and manually typed comma separators should be blocked with no message. The input should add the decimal point and comma separators automatically while the user is typing.
- Main money action inputs should accept separator input only after at least one digit and only when the previous accepted input is a digit. Starting separators and consecutive separators should be blocked with no message.
- Main money action inputs should automatically format typed values with two digits after the decimal point, automatic comma separators for thousands and larger values, and no `$` sign while the user is typing. Without separator input, typed digits are whole money amount digits: typing `5` should show `5.00`, `58` should show `58.00`, `589` should show `589.00`, `5895` should show `5,895.00`, `58955` should show `58,955.00`, and `589550` should show `589,550.00`.
- Main money action inputs should use accepted separators from `Space` or `Cent` to split digit groups. If the input ends with a separator, all completed groups should be whole money amount digits and cents should show as `00`. If the final group after a separator has one or two digits, it should be cents and should be left-padded with `0` when it has one digit. Earlier groups should join into the whole money amount. If there is only one accepted separator and the final group grows to three or more digits, that final group should become another whole money amount group and cents should show as `00`. Once the input has two accepted separators, the final group is the cents group and should accept at most two digits.
- Main money action inputs should treat `0` as a normal digit, not a starting zero-position skip. Unneeded leading zeros in the whole money amount should be normalized away, so typing `0005` shows `5.00`.
- Deleting one typed character from a main money action input should reformat the remaining typed value with two digits after the decimal point and no `$` sign while the input is still open.
- Deleting all typed numbers from a main money action input should return it to `0.00` instead of making it empty.
- Main money action inputs should be append-only: users should not move the cursor into the formatted value, select part of it, replace selected text, or directly edit generated comma separators or the generated decimal point.
- Main money action input typing should add only to the end, and Backspace or Delete should remove only the last accepted digit or accepted separator.
- Main money action inputs should block letters, minus signs, decimal points, `$` signs, manually typed comma separators, starting separators, consecutive separators, additional separator input after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, and other invalid typed characters.
- Main money action inputs should block typed characters that would make the input greater than `999,999.99`, with no message and no field change.
- The dashboard, horizontal input square, `Balance Changes`, and `Savings` displays should contain money amounts up to `999,999.99$` without horizontal page overflow, overlapping content, hidden actions, rejected valid values, or missing required comma separators while typing or in visible saved/rendered values.
- Main money action inputs should block paste. Pasted content should not appear, the input should keep its previous value, and no message should appear.
- Main money action input flows should show `Save Changes`, `Yes`, and `Cancel`.
- In main money action input flows, `Yes` should try to save the entered value using the selected `Add`, `Subtract`, or `Modify` rule.
- In main money action input flows, `Cancel` should close the input flow, return to the dashboard, change nothing, save nothing, create no `Balance Changes` entry, and show no message.
- In main money action input flows, clicking or tapping outside the horizontal input square, `Save Changes`, `Yes`, `Cancel`, and the `Cent` button should act like `Cancel`.
- In main money action input flows, tapping or clicking the `Cent` button should behave like pressing `Space` and should not act like `Cancel`.
- `Saving` square planned money amount inputs may include cents, with up to two digits after the decimal point, and should not save values greater than `999,999.99$`.
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
- `Saving` square planned money amount should be greater than `0.00$` and not greater than `999,999.99$` when creating a `Saving` square.
- Trying to save a missing planned money amount should do nothing until the user enters a planned money amount or cancels.
- Trying to save a new `Saving` square with a planned money amount of `0.00$` or greater than `999,999.99$` should do nothing: no message, no new `Saving` square, no saved data change, no `Balance Changes` entry, and the same `Saving` square create input step stays open.
- New `Saving` square create validation should check planned money amount first, missing `Saving` name second, and duplicate `Saving` name third.
- A duplicate `Saving` name with a missing, `0.00$`, or above-limit planned money amount should keep the same `Saving` square create input step open with no message and no saved data change.
- `0.00$` should remove an existing `Saving` square instead of saving it with a `0.00$` planned money amount.
- Changing a `Saving` square planned money amount should use a new total planned money amount, not an add or subtract amount.
- Total planned money amount in valid `Saving` squares may be greater than the current money amount. The money amount shown inside `Savings` should stop at `0.00$`, coverage bars should show what is still needed for each square, and the fixed top `Savings` area should show the total difference as `{money amount} needed`.
- `Balance Changes` created dates and created times are visible to the user in the format `July 21, 2026 at 3:45 PM` and use the user's browser/device local date and time. Each entry should also save an internal exact created date and time with seconds and milliseconds for ordering and clearing entries after 30 days, but should not show seconds or milliseconds to the user.
- The user should not choose dates for `Add` or `Subtract`.
- `Add`, `Subtract`, and `Balance Changes` entries should not have notes.
- Browser storage data should be checked before use so broken saved data does not crash the website.
- Broken saved data should show `Saved data could not be loaded.` and a `Start again` action.
- Saved browser data should load only when the data version value is exactly `1`.
- Saved browser data with a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, should be treated as broken saved data.
- Clicking `Start again` should delete the broken saved data and immediately create fresh browser storage data with the saved money amount set to `0.00`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`.
- If saved browser data can be read and has data version `1`, one broken saved `Saving` square should appear as a broken square in `Savings` instead of triggering the full saved-data error.
- Visible history entries should be shown for 30 days.
- For visible `Balance Changes` history, one month means 30 days, not a calendar month.
- Each `Balance Changes` internal visible until date and time should be calculated from the internal exact created date and time plus 30 days using the user's browser/device local date and time.
- Expiring old history entries should not change the current money amount.

## Browser Storage Rules

- Save website data in browser storage on the user's device.
- The first version should not use email accounts, login, cloud sync, or a server database.
- Do not create saved browser data only because the website opened and showed the default `0.00$` money amount.
- Save after every successful money amount change, `Saving` square change, or `Balance Changes` delete.
- Do not save browser storage for `Add`, `Subtract`, or `Modify` attempts with an empty or above-limit money amount because nothing changed.
- Do not save browser storage for `Add` or `Subtract` attempts with `0.00$` because nothing changed.
- Do not save browser storage, create browser storage, run `Balance Changes` cleanup, close the input flow, or show a message when `Modify` is confirmed with the same money amount that is already shown.
- Do not save browser storage for new `Saving` square attempts with a planned money amount of `0.00$` or greater than `999,999.99$` because nothing changed.
- Save `Modify` changes only as an updated current money amount, not as a history entry.
- Save `Balance Changes` deletes only as visible history removal, not as a money amount change.
- Load saved browser data before showing the dashboard.
- If saved browser data exists, restore the money amount, 30-day visible `Balance Changes` history, and `Saving` squares.
- Restore saved browser data only when the data version value is exactly `1`.
- Treat saved browser data with a missing, wrong, future, unreadable, or unrecognized data version, or a saved current money amount outside `0.00` through `999999.99` or not in normalized plain decimal format, as broken saved data.
- When the user chooses `Start again` after broken saved data, immediately create fresh browser storage data with the saved money amount set to `0.00`, an empty `Balance Changes` list, no saved `Saving` squares, and data version `1`.
- If saved browser data can be read and has data version `1`, do not show the full saved-data error only because one saved `Saving` square is broken.
- A saved `Saving` square should be treated as broken when it has a missing ID, duplicate ID, missing name, empty trimmed name, duplicate trimmed name ignoring uppercase or lowercase differences, missing planned money amount, invalid planned money amount, planned money amount of `0.00$` or less, planned money amount greater than `999,999.99$`, missing order, invalid order, or duplicate order.
- For duplicate saved IDs, duplicate saved names, or duplicate saved orders, keep the first matching saved `Saving` square in saved list order as normal if it is otherwise valid, and treat later matching saved `Saving` squares as broken.
- Show broken saved `Saving` squares in `Savings` with `Saving could not be loaded.`, `Fix`, and `Delete`.
- Exclude broken saved `Saving` squares from `Savings money amount`, top needed text, coverage calculations, and coverage bars until fixed.
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
- Manual cash tracker.

## Testing Checklist

- New user sees the money amount as `0.00$` when no saved data exists.
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
- Clicking `Add`, `Subtract`, or `Modify` starts the selected money amount input flow and does not count as an outside click.
- Add money updates the money amount correctly.
- `Add`, `Subtract`, and `Modify` money amount inputs open as a horizontal square in the middle of the screen.
- The main money amount circle stays visible in the background while the horizontal input square is open.
- The horizontal input square starts at `0.00`.
- The horizontal input square is focused and ready for typing immediately when it opens, without requiring another tap or click.
- Main money action inputs request a mobile keyboard suitable for digit entry on supported devices.
- Main money action inputs show a rectangular `Cent` button for all users while the input is open.
- On mobile, the `Cent` button appears directly above the mobile keyboard when possible.
- Typing `5` in a main money action input shows `5.00`.
- Typing `58` in a main money action input shows `58.00`.
- Typing `589` in a main money action input shows `589.00`.
- Typing `5895` in a main money action input shows `5,895.00`.
- Typing `58955` in a main money action input shows `58,955.00`.
- Typing raw digits `589550` in a main money action input shows `589,550.00`.
- Typing `5`, then `Space`, in a main money action input keeps the display at `5.00`.
- Typing `5`, then `Space`, then `5` in a main money action input shows `5.05` and saves and shows as `5.05$`.
- Typing `5`, then `Space`, then `50` in a main money action input shows `5.50` and saves and shows as `5.50$`.
- Typing `58`, then `Space`, then `430` in a main money action input shows `58,430.00` and saves and shows as `58,430.00$`.
- Typing `58`, then `Space`, then `430`, then `Space`, then `88` in a main money action input shows `58,430.88` and saves and shows as `58,430.88$`.
- Typing `0`, then `Space`, then `5` in a main money action input shows `0.05` and saves and shows as `0.05$`.
- Typing `999999`, then `Space`, then `99` in a main money action input shows `999,999.99` and saves and shows as `999,999.99$`.
- Typing `5`, choosing `Cent`, then typing `5` in a main money action input shows `5.05` and saves and shows as `5.05$`.
- Typing `5`, choosing `Cent`, then typing `50` in a main money action input shows `5.50` and saves and shows as `5.50$`.
- Typing `58`, choosing `Cent`, typing `430`, choosing `Cent`, then typing `88` in a main money action input shows `58,430.88` and saves and shows as `58,430.88$`.
- Typing `0005` in a main money action input shows `5.00` and saves and shows as `5.00$`.
- Typing `Space`, `Space`, `Space`, then `5` in a main money action input blocks the three `Space` key presses and then shows `5.00` after the `5` is typed.
- Tapping `Cent`, `Cent`, `Cent`, then typing `5` in a main money action input blocks the three `Cent` taps and then shows `5.00` after the `5` is typed.
- Trying to type another `Space` or tap `Cent` after `58`, `Space`, `430`, `Space`, `88` does not change the input and shows no message.
- Trying to type a third cents digit after the cents group is fixed by two accepted separators does not change the input and shows no message.
- Saving raw digits `589550` in a main money action input stores `589550.00` and shows `589,550.00$`.
- Saving a main money action input stores the money amount without the `$` sign or comma separators and shows the saved money amount with the `$` sign at the end.
- Deleting one typed character from a main money action input reformats the remaining typed value as a money amount.
- Deleting all typed numbers in a main money action input returns it to `0.00` instead of making it empty.
- Clicking or tapping inside `58,430.88` keeps the main money action input append-only, so the next accepted typed character is handled at the end.
- Selecting `430` in `58,430.88` and typing `9` does not replace `430`; accepted typing uses append-only behavior.
- Backspace or Delete with the cursor or selection inside the formatted value removes only the last accepted digit or accepted separator.
- Typing letters, minus signs, decimal points, `$` signs, manually typed comma separators, starting separators, consecutive separators, additional separator input after two accepted separators, a third cents digit after the cents group is fixed by two accepted separators, or other blocked characters into a main money action input does not change the field, and no message appears for that typed character.
- Tapping `Cent` when separator input is blocked does not change the field, and no message appears.
- Tapping `Cent` does not cancel or close the main money amount input flow.
- Trying to type a main money action input value greater than `999,999.99` does not change the field and shows no message.
- Money amounts up to `999,999.99$` stay contained in the horizontal input square, dashboard, `Balance Changes`, and `Savings` displays without horizontal page overflow, overlap, hidden actions, or missing required comma separators while typing or in visible saved/rendered values.
- Trying to paste letters, numbers, symbols, or any other content into a main money action input does not change the field and shows no message.
- Trying to paste letters, numbers, symbols, or any other content into a `Saving` square planned money amount input does not change the field and shows no message.
- `Add`, `Subtract`, and `Modify` money amount input flows show `Save Changes`, `Yes`, and `Cancel`.
- Clicking `Yes` applies the selected `Add`, `Subtract`, or `Modify` action when the typed amount is valid for that action and, for `Modify`, different from the money amount already shown.
- After a successful `Add`, `Subtract`, or `Modify` that changes the money amount, the horizontal input square closes, the temporary typed input resets for the next input, the dashboard money amount view returns, the main money action buttons are hidden, the updated money amount is shown, and no message appears.
- Clicking `Cancel` or clicking/tapping outside a main money amount input flow changes nothing, saves nothing, creates no `Balance Changes` entry, and shows no message.
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
- Empty `Balance Changes` shows no rows, sentence, placeholder, icon, or other empty-state content.
- History does not replace separate entries with only a net result.
- Money amount change history appears directly under the main money amount.
- There is no separate `View history` action for money amount change history.
- Long money amount change history can be reached by scrolling down.
- During cleanup, `Balance Changes` entries at or after their visible until date are deleted from browser storage.
- `Balance Changes` cleanup runs when the website opens and loads saved data.
- `Balance Changes` cleanup runs after every successful saved user action.
- `Balance Changes` internal visible until dates are calculated from the internal exact created date and time plus 30 days using the user's browser/device local date and time.
- Expiring old history entries does not change the current money amount.
- `Balance Changes` shows correct differences.
- Visible `Balance Changes` rows show the signed money amount, action text, and both the created date and created time, such as `+56.00$ added` or `-34.00$ subtracted` plus a visible date and time like `July 21, 2026 at 3:45 PM`.
- Visible `Balance Changes` entries are compact, with the signed money amount and action text in the top-left corner and the visible date and time in the bottom-right corner.
- Mobile keeps the same compact `Balance Changes` entry layout and allows wrapping only as needed to keep text readable.
- Visible `Balance Changes` dates and times use the user's browser/device local date and time.
- Visible `Balance Changes` rows do not show previous money amount, new money amount, internal exact created date and time, seconds, milliseconds, or internal visible until date and time.
- Deleting a `Balance Changes` entry removes only that visible history entry and does not change the current money amount.
- Touch users open `Balance Changes` delete by pressing and holding the entry.
- Mouse users open `Balance Changes` delete by clicking and holding the entry.
- Opening `Balance Changes` delete shows a little square with the exact action texts `Delete` and `Cancel`.
- Clicking `Cancel` closes the little square without changing anything.
- Deleting a `Balance Changes` entry asks for confirmation after `Delete` is clicked with `Delete this Balance Change?`, shows `Cancel` and `Delete`, and does not offer undo.
- Saved `Balance Changes` entries cannot be edited.
- Browser storage saves the current money amount and history.
- Browser storage restores the current money amount and history after refresh.
- Browser storage restores data after closing and reopening the website in the same browser.
- The first view does not explain where saved data is stored.
- The first view does not warn that saved data may disappear after browser or device changes.
- The default `0.00$` dashboard appears when there is no saved browser data.
- Broken browser storage data shows a clear recovery message.
- Saved browser storage data with a current money amount greater than `999999.99` shows the broken saved-data recovery message.
- Saved browser storage data with a current money amount of `5000.00` loads and shows the money amount as `5,000.00$`.
- Saved browser storage data with a current money amount of `5`, `5.0`, `005.00`, `5,000.00`, or `5.000` shows the broken saved-data recovery message.
- Clicking `Start again` after broken browser storage data deletes the broken saved data and immediately creates fresh browser storage data with the saved money amount set to `0.00`, empty `Balance Changes`, no saved `Saving` squares, and data version `1`.
- If saved browser data has one broken saved `Balance Changes` entry but the rest of the data can be read, the website loads the rest of the data, removes only that broken history entry, does not change the current money amount, and shows no error message.
- If saved browser data has one broken saved `Saving` square but the rest of the data can be read, the website loads the rest of the data and shows that square as `Saving could not be loaded.` with `Fix` and `Delete`.
- A broken saved `Saving` square with missing name, duplicate name, invalid order, duplicate order, invalid planned money amount, planned money amount of `0.00$` or less, or planned money amount greater than `999,999.99$` does not affect `Savings money amount`, top needed text, or valid square coverage calculations.
- Fixing a broken saved `Saving` square replaces the broken square with a temporary `Saving` input square in the same visible position, asks for a valid `Saving` name and planned money amount greater than `0.00$` and not greater than `999,999.99$`, saves browser storage, turns the broken square into a normal square in the same visible position when possible, recalculates `Savings`, and creates no `Balance Changes` entry.
- Deleting a broken saved `Saving` square asks for `Delete this Saving?`, removes only that broken square, saves browser storage, recalculates `Savings`, and creates no `Balance Changes` entry.
- The website works whether the user updates money rarely or many times in one day.
- `Saving` squares can be created, renamed, updated, and deleted.
- `Saving` square rename starts by opening the square's action state and clicking the `Saving` name in the square.
- `Saving` square planned-money-amount change starts by opening the square's action state and clicking the planned money amount in the square.
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
- Creating or renaming a `Saving` square with a duplicate name returns to the `Saving` squares view with no duplicate-name error message and no saved data changes.
- Creating a `Saving` square with a duplicate name and a missing or `0.00$` planned money amount keeps the same `Saving` square create input step open with no message and no saved data change.
- Default `Saving` squares show their thin coverage bars.
- `Saving` squares appear in one vertical column on mobile and desktop.
- Clicking a `Saving` square opens its action state.
- The `Saving` square action state hides the thin coverage bar and shows `Delete` at the bottom center.
- Touch users can reorder `Saving` squares by holding and moving the whole square.
- Mouse users can reorder `Saving` squares by clicking, holding, and dragging the whole square.
- Holding or dragging a `Saving` square does not open rename, planned-money-amount change, or delete.
- Moving the last `Saving` square to the top makes that square checked first, saves the final order, and recalculates coverage bars from the new visible order.
- Clicking `Savings` opens the Savings section.
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
- Deleting a `Saving` square asks for confirmation first with `Delete this Saving?`, shows `Cancel` and `Delete`, and does not offer undo.
- The add-a-saving action appears as a circle with a `+` sign.
- When `Savings` has no visible `Saving` squares, the circle `+` action appears in the middle of the `Saving` squares area.
- The no-`Saving`-squares empty state uses the centered circle `+` action and does not show a separate empty-state text sentence.
- After at least one `Saving` square exists, the circle `+` action appears at the top-left of the `Saving` squares area.
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
