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

The first money amount history entry should be stored as a normal visible `Balance Changes` entry. It should stay visible for one month and may be deleted from visible history without changing the current money amount.

## Savings Coverage And Storage

Each `Saving` square should be saved with:

- ID.
- Name.
- Amount set aside.
- Order.
- Created date.
- Updated date.

`Saving` square order should be saved in browser storage so the same order appears after refresh.

Coverage bars and `{money amount} needed` notes should be calculated from the current money amount and the ordered `Saving` squares. They should not be stored as the source of truth.

The coverage calculation should:

- Start with the current money amount.
- Check `Saving` squares from top to bottom.
- Mark each square as fully covered, partly covered, or not covered.
- Reduce the remaining money amount used for display after each square.
- Stop the money amount shown inside `Savings` at `$0`.

Changing the current money amount should update coverage bars and the money amount shown inside `Savings`, but it should not automatically change the saved amounts inside `Saving` squares.

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
- Visible `Balance Changes` history for the last month.
- `Saving` squares.
- Basic website settings if needed.

The website should save data after every successful:

- `Add`.
- `Subtract`.
- `Modify`.
- `Saving` square change.
- `Balance Changes` delete that removes only the visible history entry.

When the user closes the website, refreshes the page, or opens the website again later in the same browser, the saved money amount and one-month visible history should still be there.

The saved data only belongs to that browser on that device.

If the user opens the website on another phone, another computer, another browser, or private browsing mode, the saved data may not appear.

If the user clears browser data, clears site data, or uninstalls the browser, the saved website data may be deleted.

The website should explain this in simple words so the user understands that browser storage is useful for convenience but is not the same as an online account or cloud backup.

## Local Privacy Scope

The MVP does not need a PIN lock, website passcode, login, or local privacy feature because the website is not a real bank account and should not collect personal information.

## History Retention

Visible money change history should be kept for one month.

When a history entry becomes older than one month, it may be removed or hidden from the visible history list.

The current money amount should not change when old visible history entries are removed or hidden.

## Data Safety

If saved browser data is broken or cannot be read, the website should not silently delete it.

The website should show a clear message and give the user a safe way to start again.

The website should use one stable storage key for the website data.

The website should include a data version number so future versions can upgrade old saved data safely.
