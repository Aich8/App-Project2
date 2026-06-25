# Technical Spec: Cash Money Organizer Website

## Money Amount Storage

The current money amount should be saved separately from the visible history list.

The money amount is calculated and updated from:

- First money amount setup.
- Recorded `Add` money actions.
- Recorded `Subtract` money actions.
- Silent `Modify` corrections.

Removing or hiding old history entries should not change the current money amount, because the current money amount is saved separately.

Deleting a `Balance Changes` entry should only remove that visible history entry. It should not change, recalculate, or reverse the current money amount.

The first money amount history entry should be stored as a normal visible `Balance Changes` entry. It should stay visible for 30 days and may be deleted from visible history without changing the current money amount.

## Savings Coverage And Storage

Each `Saving` square should be saved with:

- ID.
- Name.
- Planned money amount greater than `$0`.
- Order.
- Created date.
- Updated date.

The website should not save `Saving` squares with a planned money amount of `$0`.

`Saving` square order should be saved in browser storage so the same order appears after refresh.

Coverage bars and `{money amount} needed` notes should be calculated from the current money amount and the ordered `Saving` square planned money amounts. They should not be stored as the source of truth.

Each `Saving` square coverage bar should be a thin horizontal bar at the bottom of the square.

The coverage bar fill should use width, not height. A 0% covered square should have a fully grey bar, a partly covered square should fill green from left to right by the covered percentage, and a 100% covered square should have a fully green bar.

When a `Saving` square is not fully covered, the `{money amount} needed` note should be rendered at the top-left of the bottom coverage bar. The note position should not depend on the width of the grey part of the bar, so it stays readable when the grey part is small.

The coverage calculation should:

- Start with the current money amount.
- Check `Saving` squares from top to bottom.
- Mark each square as fully covered, partly covered, or not covered.
- Reduce the remaining money amount used for display after each square.
- Stop the money amount shown inside `Savings` at `$0`.

Changing the current money amount should update coverage bars and the money amount shown inside `Savings`, but it should not automatically change the saved planned money amounts inside `Saving` squares.

Changing a `Saving` square planned money amount should replace the saved planned money amount for that square with the new total planned money amount.

Changing a `Saving` square planned money amount should update that square's updated date.

Changing a `Saving` square planned money amount should recalculate coverage bars and the money amount shown inside `Savings`.

Changing a `Saving` square planned money amount should not create a `Balance Changes` entry and should not change the current money amount.

If a `Saving` square planned money amount is changed to `$0`, the website should remove that `Saving` square from saved browser storage instead of saving it with `$0`.

After removing a `Saving` square because its planned money amount became `$0`, the remaining `Saving` squares should keep their relative order. The website should recalculate the money amount shown inside `Savings` and coverage bars from the remaining `Saving` squares.

Deleting a `Saving` square should remove only that square and its saved details from browser storage.

Deleting a `Saving` square should not change the current money amount, should not create a `Balance Changes` entry, and should not change the saved name or planned money amount of any other `Saving` square.

After deleting a `Saving` square, the remaining `Saving` squares should keep their relative order. The website should recalculate the money amount shown inside `Savings` and coverage bars from the remaining `Saving` squares.

## Browser Storage

The first version should save data in the user's browser storage.

The first version should not require:

- Email account.
- Login.
- Online sync.
- Server database.

The saved browser data should include:

- Current money amount.
- First money amount.
- Visible `Balance Changes` history for the last 30 days.
- `Saving` squares.
- Basic website settings if needed.

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

The website should explain this in simple words so the user understands that browser storage is useful for convenience but is not the same as an online account or cloud backup.

## Local Privacy Scope

The MVP does not need a PIN lock, website passcode, login, or local privacy feature because the website is not a real bank account and should not collect personal information.

## History Retention

Visible money change history should be kept for 30 days.

For visible `Balance Changes` history, one month means 30 days, not a calendar month.

When a `Balance Changes` entry is created, its visible until date should be calculated as the entry date and time plus 30 days.

When the current date and time is at or after the visible until date and time, the entry may be removed or hidden from the visible history list.

The current money amount should not change when old visible history entries are removed or hidden.

## Data Safety

If saved browser data is broken or cannot be read, the website should not silently delete it.

The website should show a clear message and give the user a safe way to start again.

The website should use one stable storage key for the website data.

The website should include a data version number so future versions can upgrade old saved data safely.
