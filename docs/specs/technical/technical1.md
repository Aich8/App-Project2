# Technical Spec: Cash Money Organizer Website

## Money Amount Storage

The current money amount should be saved separately from the visible history list.

The money amount is calculated and updated from:

- Default `0$` starting state when no saved data exists.
- Recorded `Add` money actions.
- Recorded `Subtract` money actions.
- Silent `Modify` corrections.

Deleting old `Balance Changes` entries from browser storage should not change the current money amount, because the current money amount is saved separately.

Deleting a `Balance Changes` entry should only remove that visible history entry. It should not change, recalculate, or reverse the current money amount.

Deleting a `Balance Changes` entry should require user confirmation before deletion.

The confirmation message should be exactly `Delete this Balance Change?`, with buttons exactly named `Cancel` and `Delete`.

After a `Balance Changes` entry is deleted, the website should not offer undo.

The default `0$` starting state should not create a visible `Balance Changes` entry.

Money action availability should be based on the current money amount:

- If the current money amount is `0$`, only `Add` should be available.
- If the current money amount is greater than `0$`, `Add`, `Modify`, and `Subtract` should be available.

Money action validation should block negative values. `Add` and `Subtract` should require a money amount greater than `0$`. `Modify` should allow `0$` or more.

Money input parsing should allow cents, with up to two digits after the decimal point. The user should enter only the number and may type a decimal point, such as `14.56`.

Saved money amounts should use the decimal money amount with the `$` sign at the end, such as `14.56$`. They should not be converted to integer cents, such as `1456`.

Inputs with more than two digits after the decimal point should be blocked.

## Savings Coverage And Storage

Each `Saving` square should be saved with:

- ID.
- Name.
- Planned money amount greater than `0$`.
- Order.
- Created date.
- Updated date.

The website should not save `Saving` squares with a planned money amount of `0$`.

`Saving` square planned money amounts should allow cents, with up to two digits after the decimal point, and should be saved as decimal money amounts with the `$` sign at the end, such as `14.56$`.

Saved `Saving` square names should be unique inside `Savings`.

Before creating or renaming a `Saving` square, the website should trim spaces at the beginning and end of the entered name.

The website should check duplicate `Saving` square names by comparing trimmed names without uppercase or lowercase differences.

If a create or rename action would duplicate another saved `Saving` square name, the website should not save that duplicate name.

`Saving` square order should be saved in browser storage so the same order appears after refresh.

When the user finishes moving a `Saving` square and lets go, the website should save the final visible order in browser storage. Intermediate drag positions should not be saved as the source of truth.

After a reorder action finishes, coverage bars and the money amount shown inside `Savings` should be recalculated from the new visible order.

Reordering `Saving` squares should not change the current money amount and should not create a `Balance Changes` entry.

Coverage bars and `{money amount} needed` notes should be calculated from the current money amount and the ordered `Saving` square planned money amounts. They should not be stored as the source of truth.

Each `Saving` square coverage bar should be a thin horizontal bar at the bottom of the square.

The coverage bar fill should use width, not height. A 0% covered square should have a fully grey bar, a partly covered square should fill green from left to right by the covered percentage, and a 100% covered square should have a fully green bar.

When a `Saving` square is not fully covered, the `{money amount} needed` note should be rendered at the top-left of the bottom coverage bar. The note position should not depend on the width of the grey part of the bar, so it stays readable when the grey part is small.

The coverage calculation should:

- Start with the current money amount.
- Check `Saving` squares from top to bottom.
- Use the final visible order after any completed reorder action.
- Mark each square as fully covered, partly covered, or not covered.
- Reduce the remaining money amount used for display after each square.
- Stop the money amount shown inside `Savings` at `0$`.

The calculated money amount shown at the top of `Savings` should render with the exact user-facing label `Savings money amount`.

`Savings money amount` is display text only. It should not be stored as the source of truth for the calculated value.

Changing the current money amount should update coverage bars and the money amount shown inside `Savings`, but it should not automatically change the saved planned money amounts inside `Saving` squares.

Changing a `Saving` square planned money amount should replace the saved planned money amount for that square with the new total planned money amount.

Changing a `Saving` square planned money amount should update that square's updated date.

Changing a `Saving` square planned money amount should recalculate coverage bars and the money amount shown inside `Savings`.

Changing a `Saving` square planned money amount should not create a `Balance Changes` entry and should not change the current money amount.

If a `Saving` square planned money amount is changed to `0$`, the website should remove that `Saving` square from saved browser storage instead of saving it with `0$`.

After removing a `Saving` square because its planned money amount became `0$`, the remaining `Saving` squares should keep their relative order. The website should recalculate the money amount shown inside `Savings` and coverage bars from the remaining `Saving` squares.

Deleting a `Saving` square should remove only that square and its saved details from browser storage.

Deleting a `Saving` square should require user confirmation before deletion.

The confirmation message should be exactly `Delete this Saving?`, with buttons exactly named `Cancel` and `Delete`.

Deleting a `Saving` square should not change the current money amount, should not create a `Balance Changes` entry, and should not change the saved name or planned money amount of any other `Saving` square.

After deleting a `Saving` square, the remaining `Saving` squares should keep their relative order. The website should recalculate the money amount shown inside `Savings` and coverage bars from the remaining `Saving` squares.

After a `Saving` square is deleted, the website should not offer undo.

## Browser Storage

The first version should save data in the user's browser storage.

The first version should not require:

- Email account.
- Login.
- Online sync.
- Server database.

The saved browser data should include:

- Money amount.
- Visible `Balance Changes` history for the last 30 days.
- `Saving` squares.

The default `0$` starting state is displayed when no saved data exists. It should not write browser storage just because the website opened.

Browser storage should be created after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The first version has no website settings to save. Do not save a general settings item or an empty settings object.

The website should save data after every successful:

- `Add`.
- `Subtract`.
- `Modify`.
- `Saving` square change.
- `Balance Changes` delete that removes only the visible history entry.

When the user closes the website, refreshes the page, or opens the website again later in the same browser, the saved money amount and 30-day visible history should still be there.

The saved data only belongs to that browser on that device.

If the user opens the website on another phone, another computer, another browser, or private browsing mode, the saved data may not appear.

If the user clears browser data, clears site data, or uninstalls the browser, the saved website data may be deleted.

Browser storage is an internal implementation detail. The website should not tell the user where saved information is stored.

The website should not tell the user that saved information belongs to the same browser or device.

The website should not warn the user that saved information may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

## Local Privacy Scope

The MVP does not need a PIN lock, website passcode, login, or local privacy feature because the website is not a real bank account and should not collect personal information.

## History Retention

Visible money change history should be kept for 30 days.

Visible `Balance Changes` entries should be displayed newest first by internal created date and time.

For visible `Balance Changes` history, one month means 30 days, not a calendar month.

The user should not choose dates for `Add` or `Subtract` entries.

The website should store a `Balance Changes` created date and time internally only for 30-day clearing.

When a `Balance Changes` entry is created, its visible until date should be calculated as the internal created date and time plus 30 days.

When the current date and time is at or after the visible until date and time, the entry should be deleted from browser storage and removed from the visible history list.

The current money amount should not change when old visible history entries are deleted.

## Data Safety

If saved browser data is broken or cannot be read, the website should not silently delete it.

The website should show this exact message:

`Saved data could not be loaded.`

The website should show a `Start again` action with that message.

When the user clicks `Start again`, the website should:

- Delete the broken saved data from browser storage.
- Create fresh browser storage data with the money amount set to `0$`.
- Use an empty `Balance Changes` list.
- Use an empty `Saving` squares list.

The website should use one stable storage key for the website data.

The website should include a data version number so future versions can upgrade old saved data safely.
