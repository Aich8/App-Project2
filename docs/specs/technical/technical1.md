# Technical Spec: Cash Money Organizer Website

## Money Amount Storage

The current money amount should be saved separately from the visible history list.

The money amount is calculated and updated from:

- Default `0.00$` starting state when no saved data exists.
- Recorded `Add` money actions.
- Recorded `Subtract` money actions.
- Silent `Modify` corrections.

Deleting old `Balance Changes` entries from browser storage should not change the current money amount, because the current money amount is saved separately.

Deleting a `Balance Changes` entry should only remove that visible history entry. It should not change, recalculate, or reverse the current money amount.

Deleting a `Balance Changes` entry should require user confirmation before deletion.

The delete action for a `Balance Changes` entry should open from the entry itself. Touch users should press and hold the entry. Mouse users should click and hold the entry. The implementation should then render a small square on the screen with two actions exactly named `Delete` and `Cancel`.

Opening, canceling, or closing the small `Delete` and `Cancel` square should not delete the `Balance Changes` entry and should not write browser storage. Only confirming the later delete confirmation should remove the visible history entry.

The confirmation message should be exactly `Delete this Balance Change?`, with buttons exactly named `Cancel` and `Delete`.

After a `Balance Changes` entry is deleted, the website should not offer undo.

The default `0.00$` starting state should not create a visible `Balance Changes` entry.

When the visible `Balance Changes` list is empty after loading, cleanup, or a user action, the implementation should render no `Balance Changes` entry rows and no empty-state text, placeholder, icon, or other empty-state content.

Visible `Balance Changes` rows should render the signed money amount, action text, and both the created date and created time for that entry: `+{money amount} added` or `-{money amount} subtracted`, plus the visible created date and created time. A date-only display is not enough. The visible date and time format should be like `July 21, 2026 at 3:45 PM`.

Visible `Balance Changes` entries should render as compact entry boxes, not large panels. The signed money amount and action text should align to the top-left corner of the entry. The visible created date and created time should align to the bottom-right corner of the same entry, below the money change text and close enough that the two details feel connected. Mobile should keep the same layout. Text may wrap only as needed to stay readable, but the implementation should preserve the top-left money change text area and bottom-right visible date and time area on small screens.

Visible `Balance Changes` dates and times should use the user's browser/device local date and time. The first version should not use a server time, cloud time, or external time service for `Balance Changes`.

Visible `Balance Changes` rows should not render the previous money amount, new money amount, or internal visible until date and time. Saved previous money amount, new money amount, and internal visible until date and time are internal data fields only. The created date and created time should be saved and rendered in the visible `Balance Changes` row.

Money action availability should be based on the current money amount:

- If the current money amount is `0.00$`, only `Add` should be available.
- If the current money amount is greater than `0.00$`, `Add`, `Subtract`, and `Modify` should be available.

When main money actions are visible, they should render as buttons lined together horizontally near the main money amount. When all three actions are visible, the left-to-right order should be `Add`, `Subtract`, and `Modify`. The main money action buttons should not render as a modal, bottom sheet, separate page, or large expanded area.

When main money action buttons are visible, a click or tap outside the main money amount and outside the action buttons should close the buttons without changing saved data, writing browser storage, creating a `Balance Changes` entry, or showing a message.

Money action validation should block negative values. `Add` and `Subtract` should require a money amount greater than `0.00$`. `Modify` should allow `0.00$` or more. Clicking `Yes` while the main money action input is `0.00$` in `Add` or `Subtract` should be treated as no action: no message, no money amount change, no browser storage write, no `Balance Changes` entry, and the same money amount input step stays open.

`Add`, `Subtract`, and `Modify` money amount input flows should render a horizontal money amount input square in the middle of the screen. The main money amount circle should remain visible in the background while the horizontal input square is open. The square should start by showing `0.00$`.

Main money action input fields should accept only digits from `0` through `9`. The implementation should automatically format typed digits as a whole money amount with `.00$` at the end, such as `5.00$` or `50.00$`. The user should not type the decimal point or the `$` sign.

When a user deletes one typed digit from a main money action input, the implementation should remove that digit from the typed-digit value and format the remaining typed digits again with `.00$`. For example, deleting the `0` from `50.00$` should show `5.00$`.

When a user deletes all typed digits from a main money action input, the implementation should set the input display back to `0.00$`. The main money action input should not render as an empty field.

Main money action input fields should keep the previous input value when the user types a character that is not allowed. Letters, minus signs, decimal points, `$` signs, and other blocked typed characters should not appear in the field, and no message should appear for those blocked typed characters.

Main money action input fields should block paste. If the user tries to paste letters, numbers, symbols, or any other content into a main money action input, the pasted content should not appear, the field should keep its previous value, and no message should appear.

`Add`, `Subtract`, and `Modify` money amount input flows should render the exact text `Save Changes` under the horizontal input square, with buttons exactly named `Yes` and `Cancel` under that text. `Yes` should run the validation and save rules for the selected action. `Cancel` should close the money amount input flow, return to the dashboard money amount view, leave all saved data unchanged, create no `Balance Changes` entry, and show no message. Clicking or tapping outside the horizontal input square and its `Save Changes`, `Yes`, and `Cancel` controls should act like `Cancel`.

Main money action input values should use the decimal money amount with the `$` sign at the end, such as `5.00$`. They should not be converted to integer cents, such as `500`.

## Savings Coverage And Storage

Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`.

Each `Saving` square should be saved with:

- ID.
- Name.
- Planned money amount greater than `0.00$`.
- Order.
- Created date.
- Updated date.

The website should not save `Saving` squares with a planned money amount of `0.00$`.

Trying to save a new `Saving` square with a planned money amount of `0.00$` should be treated as no action: no message, no new `Saving` square, no browser storage write, no `Balance Changes` entry, and the same `Saving` square create input step stays open.

`Saving` square planned money amounts should allow cents, with up to two digits after the decimal point, and should be saved as decimal money amounts with the `$` sign at the end, such as `14.56$`.

`Saving` square planned money amounts should use their own saved and shown money amount normalization: `0.00$` for zero, two decimal digits for nonzero whole money amounts, and two decimal digits for nonzero money amounts with cents.

If the planned money amount is empty when creating a `Saving` square, the create flow should stay open without showing an error message. The failed save should not write browser storage, should not create a `Saving` square, should not update any dates, and should not create a `Balance Changes` entry.

If the planned money amount is empty when changing an existing `Saving` square, the planned-money-amount change flow should stay open without showing an error message. The failed save should not write browser storage, should not change the saved planned money amount, should not update any dates, and should not create a `Balance Changes` entry.

`Saving` square planned money amount inputs should request a decimal numeric keyboard on devices that support it. The implementation should still validate the entered value because desktop keyboards and browser differences can bypass the mobile keyboard layout.

`Saving` square planned money amount inputs should allow numbers and one decimal point, while still blocking more than two digits after the decimal point.

`Saving` square planned money amount inputs should block paste. If the user tries to paste letters, numbers, symbols, or any other content into a planned money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear.

`Saving` square create, rename, planned-money-amount change, and broken-square fix input flows should render two text actions at the bottom: `Save` and `Cancel`. `Save` should run the validation and save rules for that action. `Cancel` should close the input flow, return to the `Saving` squares view, leave all saved data unchanged, create no `Balance Changes` entry, update no dates, and show no message.

Saved `Saving` square names should be unique inside `Savings`.

The implementation should not set a character-count maximum for `Saving` square names and should not reject names only because they are long. Names should be treated as text values, so one-letter names, number-only names, names with numbers before or after words, multiple-word names, full sentences, symbols, punctuation, emoji characters, and very long names are valid when they satisfy the required-name and unique-name rules.

Before creating or renaming a `Saving` square, the website should trim spaces at the beginning and end of the entered name.

Spaces inside the trimmed `Saving` square name should be preserved in saved browser data.

If the trimmed name is empty when creating a `Saving` square, the create flow should stay open without showing an error message. The failed save should not write browser storage, should not create a `Saving` square, should not update any dates, and should not create a `Balance Changes` entry.

If the trimmed name is empty when renaming a `Saving` square, the rename flow should stay open without showing an error message. The failed save should not write browser storage, should not rename the `Saving` square, should not update any dates, and should not create a `Balance Changes` entry.

The website should check duplicate `Saving` square names by comparing trimmed names without uppercase or lowercase differences.

If a create or rename action would duplicate another saved `Saving` square name, the website should not save that duplicate name.

If a create or rename action would duplicate another saved `Saving` square name, the website should close the create or rename flow and return to the `Saving` squares view without showing a duplicate-name error message. The failed duplicate-name action should not write browser storage, should not create a `Saving` square, should not rename an existing `Saving` square, should not update any dates, and should not create a `Balance Changes` entry.

For new `Saving` square creation, validation should run in this order:

1. Planned money amount is missing or `0.00$`.
2. Trimmed `Saving` name is empty.
3. Trimmed `Saving` name duplicates another saved `Saving` square name, ignoring uppercase or lowercase differences.

The first matching rule should decide the result. Duplicate-name create behavior should run only after the planned money amount is greater than `0.00$` and the trimmed `Saving` name is not empty. If a duplicate name is entered with a missing or `0.00$` planned money amount, the same `Saving` square create input step should stay open with no message and without writing browser storage.

Rendered `Saving` square names should wrap and stay contained inside the square layout. Long names, including long unbroken number or letter sequences, should not cause horizontal page overflow, overlap other square content, or force the website to reject the saved name. The implementation may use wrapping, breaking, or internal containment for very long names, but it should keep the saved name complete.

When saved browser data can be read and has data version `1`, the implementation should validate each saved `Saving` square before using it as a normal square.

A saved `Saving` square should be treated as a broken square if it has a missing ID, duplicate ID, missing name, empty trimmed name, duplicate trimmed name ignoring uppercase or lowercase differences, missing planned money amount, invalid planned money amount, planned money amount of `0.00$` or less, missing order, invalid order, or duplicate order.

For duplicate saved IDs, duplicate saved names, or duplicate saved orders, the first matching saved `Saving` square in the saved list order should stay normal if it is otherwise valid. Later matching saved `Saving` squares should be treated as broken squares.

Broken `Saving` squares should be kept in the loaded `Savings` view as error squares instead of making all saved browser data fail. If a broken square has a usable saved position, it should appear near that saved position. If no usable position exists, it should appear after the valid `Saving` squares.

A broken `Saving` square should render the exact text `Saving could not be loaded.` and actions exactly named `Fix` and `Delete`.

Broken `Saving` squares should be excluded from `Savings money amount` calculation, coverage calculation, `Saving` square duplicate-name checks for normal valid squares, and reorder calculation until fixed. Broken `Saving` squares should not render coverage bars.

Activating `Fix` on a broken `Saving` square should open a fix input flow that asks for a valid `Saving` name and planned money amount greater than `0.00$`. The fix flow should use the same required-name, duplicate-name, money amount, typing, paste-blocking, `Save`, and `Cancel` rules as creating a new `Saving` square.

When a broken `Saving` square is fixed successfully, the implementation should replace the broken saved square with a valid normal `Saving` square, save browser storage, recalculate the `Savings money amount`, recalculate coverage bars, create no `Balance Changes` entry, and show no message. The fixed square should keep the broken square's current displayed position when possible. If the broken square had a missing or duplicate ID, the fixed square should get a valid unique ID. If the broken square had a missing, invalid, or duplicate order, the fixed square should get a valid order based on its current displayed position.

Activating `Delete` on a broken `Saving` square should use the normal `Saving` square delete confirmation: exact message `Delete this Saving?`, buttons `Cancel` and `Delete`. Confirming delete should remove only that broken square from saved browser data, save browser storage, recalculate the `Savings money amount`, recalculate coverage bars, create no `Balance Changes` entry, update no other `Saving` squares, show no message, and offer no undo.

Opening `Savings` should render a full-screen `Savings` view instead of leaving `Savings` as only an inline dashboard section. The full-screen `Savings` view should include the `Savings money amount` and the visible `Saving` squares.

The full-screen `Savings` view should render a small `<` sign in the top-left corner as the back action. Activating `<` should return to the dashboard without writing browser storage, changing saved data, creating a `Balance Changes` entry, or updating dates.

The implementation should render the add-a-saving control as a circular `+` control inside the `Saving` squares area. If there are no visible `Saving` squares, the circular `+` control should be centered in that area and should be the empty state. The implementation should not render a separate empty-state text sentence for no `Saving` squares.

If one or more visible `Saving` squares exist, the circular `+` control should render at the top-left of the `Saving` squares area, before the ordered squares. Activating the `+` control from either position should open the same new `Saving` square create flow and should not save browser storage until the user successfully creates a valid `Saving` square.

In its default state, each visible `Saving` square should render the `Saving` name in the top-left corner, the planned money amount on the right side of the same top row, and the thin coverage bar at the bottom.

Activating a `Saving` square should change that same square into its action state. In the action state, the square should still render the `Saving` name in the top-left corner and the planned money amount on the right side of the same top row, should render `Delete` at the bottom center, and should not render the thin coverage bar.

In the action state, activating the rendered `Saving` name should open the rename flow for that square. Activating the rendered planned money amount should open the planned-money-amount change flow for that square. Activating `Delete` should start the delete confirmation for that square. The implementation should not open a separate action menu or larger action square for rename, planned-money-amount change, and delete.

The whole `Saving` square should be draggable for touch and mouse users. Touch users should reorder by holding and moving the whole square. Mouse users should reorder by clicking, holding, and dragging the whole square. Holding or dragging should not open rename, planned-money-amount change, or delete.

`Saving` square order should be saved in browser storage so the same order appears after refresh.

When the user finishes moving a `Saving` square and lets go, the website should save the final visible order in browser storage. Intermediate drag positions should not be saved as the source of truth.

After a reorder action finishes, coverage bars and the money amount shown inside `Savings` should be recalculated from the new visible order.

Reordering `Saving` squares should not change the current money amount and should not create a `Balance Changes` entry.

Coverage bars and `{money amount} needed` notes should be calculated from the current money amount and the ordered `Saving` square planned money amounts. They should not be stored as the source of truth.

Each `Saving` square coverage bar should be a thin horizontal bar at the bottom of the square in the square's default state.

The coverage bar fill should use width, not height. A 0% covered square should have a fully grey bar, a partly covered square should fill green from left to right by the covered percentage, and a 100% covered square should have a fully green bar.

When a `Saving` square is not fully covered, the `{money amount} needed` note should be rendered at the top-left of the bottom coverage bar. The note position should not depend on the width of the grey part of the bar, so it stays readable when the grey part is small.

The coverage calculation should:

- Start with the current money amount.
- Check `Saving` squares from top to bottom.
- Use the final visible order after any completed reorder action.
- Mark each square as fully covered, partly covered, or not covered.
- Reduce the remaining money amount used for display after each square.
- Stop the money amount shown inside `Savings` at `0.00$`.

The calculated money amount shown at the top of `Savings` should render with the exact user-facing label `Savings money amount`.

`Savings money amount` is display text only. It should not be stored as the source of truth for the calculated value.

Changing the current money amount should update coverage bars and the money amount shown inside `Savings`, but it should not automatically change the saved planned money amounts inside `Saving` squares.

Changing a `Saving` square planned money amount should replace the saved planned money amount for that square with the new total planned money amount.

Changing a `Saving` square planned money amount should update that square's updated date.

Changing a `Saving` square planned money amount should recalculate coverage bars and the money amount shown inside `Savings`.

Changing a `Saving` square planned money amount should not create a `Balance Changes` entry and should not change the current money amount.

If a `Saving` square planned money amount is changed to `0.00$`, the website should remove that `Saving` square from saved browser storage instead of saving it with `0.00$`.

After removing a `Saving` square because its planned money amount became `0.00$`, the remaining `Saving` squares should keep their relative order. The website should recalculate the money amount shown inside `Savings` and coverage bars from the remaining `Saving` squares.

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

The default `0.00$` starting state is displayed when no saved data exists. It should not write browser storage just because the website opened.

Browser storage should be created after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The first version has no website settings to save. Do not save a general settings item or an empty settings object.

The website should save data after every successful:

- `Add`.
- `Subtract`.
- `Modify`.
- `Saving` square change.
- `Balance Changes` delete that removes only the visible history entry.

Clicking `Yes` while the main money action input is `0.00$` in `Add` or `Subtract` is not a successful action and should not write browser storage.

Trying to save a new `Saving` square with a planned money amount of `0.00$` is not a successful action and should not write browser storage.

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

Visible `Balance Changes` entries should be displayed newest first by created date and time.

For visible `Balance Changes` history, one month means 30 days, not a calendar month.

The user should not choose dates for `Add` or `Subtract` entries.

The website should store a `Balance Changes` created date and created time for visible display and 30-day clearing using the user's browser/device local date and time. The rendered visible format should use the full month name, day, year, `at`, and 12-hour time with uppercase `AM` or `PM`, like `July 21, 2026 at 3:45 PM`.

When a `Balance Changes` entry is created, its visible until date should be calculated as the created date and time plus 30 days using the user's browser/device local date and time.

The implementation should run `Balance Changes` cleanup when saved data is loaded while the website opens.

The implementation should also run `Balance Changes` cleanup after every successful saved user action.

During cleanup, any `Balance Changes` entry whose visible until date and time is at or before the current date and time should be deleted from browser storage and removed from the visible history list.

The first version does not need a background timer that checks old `Balance Changes` entries while the website stays open with no user action.

The current money amount should not change when old visible history entries are deleted.

## Data Safety

If saved browser data is broken or cannot be read, the website should not silently delete it.

The website should show this exact message:

`Saved data could not be loaded.`

The website should show a `Start again` action with that message.

When the user clicks `Start again`, the website should:

- Delete the broken saved data from browser storage.
- Immediately create fresh browser storage data with the money amount set to `0.00$` and data version `1`.
- Use an empty `Balance Changes` list.
- Use an empty `Saving` squares list.

This full saved-data error should be used when the saved browser data file cannot be read, has an invalid top-level shape, or has an invalid data version. It should not be used only because one saved `Saving` square is broken while the rest of the saved browser data can still be read.

The website should use exactly one stable storage key for website data: `cash-money-organizer-website-data`.

The storage key should stay the same when the saved data version changes. Future upgrade logic should use the data version field inside the saved data instead of changing the storage key.

The website should include a data version number so future versions can upgrade old saved data safely. The first saved data version value should be `1`.

The first version should load saved browser data only when the data version value is exactly `1`.

If saved browser data has a missing, wrong, future, unreadable, or unrecognized data version, the website should treat it as broken saved data. The first version should not try to upgrade or guess how to read saved browser data with any other data version.
