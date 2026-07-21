# Functional Spec: Cash Money Organizer Website

## Starting State And Money Amount Controls

When a new user has no saved data yet, the website should show the dashboard with the money amount set to `0.00$`.

Visible money amounts should use two digits after the decimal point, such as `0.00$`, `5.00$`, and `14.50$`.

Showing the default `0.00$` money amount should not create saved browser data by itself.

The default `0.00$` money amount should not create a `Balance Changes` entry.

The dashboard should show the main money amount with the user-facing label `Current Balance`.

The main money amount should be clickable.

When the money amount is `0.00$` and the user clicks the main money amount, the website should show only:

- `Add` for adding money to the main money amount.

When the money amount is greater than `0.00$` and the user clicks the main money amount, the website should show three actions near the main money amount:

- `Add` for adding money to the main money amount.
- `Subtract` for subtracting money from the main money amount.
- `Modify` for correcting the total displayed amount.

The visible main money actions should be buttons lined together horizontally near the main money amount.

When all three main money actions are visible, the left-to-right order should be:

- `Add`
- `Subtract`
- `Modify`

The main money action buttons should not appear as a modal, bottom sheet, separate page, or large expanded area.

If the main money action buttons are visible and the user clicks or taps anywhere else on the screen, the buttons should go away, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

The dashboard should use `Current Balance` as the label for the main money amount.

The `Current Balance` label is user-facing website text. In the specs, the concept should be described as the money amount.

These controls are manual money tracking actions. They do not represent a real bank deposit, bank withdrawal, or payment.

The user can use the available money actions as often or as rarely as they want. `Add` is available at `0.00$` or more. `Modify` and `Subtract` are available only after the money amount is greater than `0.00$`. There should be no daily, weekly, monthly, or yearly limit on money amount changes.

`Add`, `Subtract`, and `Modify` money amount input flows should show a horizontal money amount input square in the middle of the screen.

The main money amount circle should stay visible in the background while the horizontal input square is open.

The horizontal input square should start by showing `0.00$`.

The user should type only numbers from `0` through `9` into the horizontal input square. The user should not type the decimal point or the `$` sign.

The horizontal input square should automatically format the typed number as a whole money amount with `.00$` at the end. For example, if the user types `5`, the square should show `5.00$`. If the user types `50`, the square should show `50.00$`.

If the user deletes one typed number from the horizontal input square, the remaining typed numbers should be formatted again with `.00$`. For example, if the square shows `50.00$` and the user deletes the `0`, the square should show `5.00$`.

If the user deletes all typed numbers from the horizontal input square, the square should return to `0.00$`. The horizontal input square should not become empty.

If the user types letters, a minus sign, a decimal point, the `$` sign, or any other blocked character into a main money amount input, the field should not change and no message should appear.

The user should not be able to paste into main money amount inputs. If the user tries to paste letters, numbers, symbols, or any other content into a main money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear.

Under the horizontal input square, the website should show the exact text `Save Changes`.

Under `Save Changes`, the website should show two buttons exactly named `Yes` and `Cancel`.

In `Add`, `Subtract`, and `Modify`, clicking `Yes` should try to apply the typed money amount using the rules for that action.

In `Add`, `Subtract`, and `Modify`, clicking `Cancel` should close the money amount input flow, return to the dashboard money amount view, change nothing, save nothing, create no `Balance Changes` entry, and show no message.

Clicking or tapping outside the horizontal input square and its `Save Changes`, `Yes`, and `Cancel` controls should act like `Cancel`.

When the user chooses `Modify`:

- The website asks for the corrected total money amount.
- The entered amount must be `0.00$` or more.
- The horizontal input square should start at `0.00$`.
- The user should type only numbers, and the website should automatically show the typed amount with `.00$`.
- If the user deletes all typed numbers, the horizontal input square should return to `0.00$`.
- If the user clicks `Yes` while the input is `0.00$`, the main money amount should become `0.00$`.
- A negative entered amount should be blocked.
- The entered amount replaces the current money amount.
- The new current money amount is saved in browser storage.
- The action should not create a history entry.
- The action should not create a `Balance Changes` entry.
- The action should not create a notification.
- The action should not display as `+{money amount} added`, `-{money amount} subtracted`, or a manual correction.
- If `Modify` changes the money amount to `0.00$`, the next click on the main money amount should show only `Add`.

When the user chooses `Add`:

- The website asks for the amount to add.
- The entered amount must be greater than `0.00$`.
- The horizontal input square should start at `0.00$`.
- The user should type only numbers, and the website should automatically show the typed amount with `.00$`.
- If the user deletes all typed numbers, the horizontal input square should return to `0.00$`.
- If the user clicks `Yes` while the input is `0.00$`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a money amount greater than `0.00$` or cancels.
- A negative entered amount should be blocked.
- The entered amount is added to the main money amount.
- The action is saved in `Balance Changes` as a positive entry.
- The history entry should display as `+{money amount} added`.
- The history entry should stay separate from other add or subtract entries.
- If the money amount was `0.00$` before a successful `Add`, then after the successful `Add` the next click on the main money amount should show `Add`, `Subtract`, and `Modify`.

When the user chooses `Subtract`:

- The website asks for the amount to subtract.
- The entered amount must be greater than `0.00$`.
- The horizontal input square should start at `0.00$`.
- The user should type only numbers, and the website should automatically show the typed amount with `.00$`.
- If the user deletes all typed numbers, the horizontal input square should return to `0.00$`.
- If the user clicks `Yes` while the input is `0.00$`, the website should do nothing. No message should appear, the money amount should stay unchanged, saved data should stay unchanged, no `Balance Changes` entry should be created, and the same money amount input step should stay open until the user enters a money amount greater than `0.00$` or cancels.
- A negative entered amount should be blocked.
- The website subtracts up to the current money amount.
- The action is saved in `Balance Changes` as a negative entry.
- The history entry should display as `-{money amount} subtracted`.
- The history entry should stay separate from other add or subtract entries.

The main money amount should never go below `0.00$`.

If the user enters a subtract amount greater than the current money amount:

- The website should set the main money amount to `0.00$`.
- The website should not show a negative money amount.
- The visible history entry should use the actual amount removed from the money amount.

Example:

- Current money amount is `20.00$`.
- User enters `50.00$` in `Subtract`.
- New money amount becomes `0.00$`.
- Visible history shows `-20.00$ subtracted`.
- Visible history should not show `-50.00$ subtracted`.

If the current money amount is already `0.00$`, `Subtract` should not be shown. If a subtract action still runs while the money amount is `0.00$`, it should keep the money amount at `0.00$`, show no message, leave saved data unchanged, and not create a visible history entry.

The website should not combine separate add and subtract actions into one history result.

Example:

- Show `+56.00$ added`.
- Show `-34.00$ subtracted`.
- Do not replace those entries with only `+22.00$ net change`.

If `Balance Changes` has no entries, the history list should show no rows.

Empty `Balance Changes` should not show a sentence, placeholder, icon, or any other empty-state content.

Each visible `Balance Changes` row should show the signed money amount, action text, and both the created date and created time for that entry:

- `+{money amount} added`.
- `-{money amount} subtracted`.

Visible `Balance Changes` rows should not show the previous money amount or new money amount.

Visible `Balance Changes` rows should show both the created date and created time for that entry. A date-only display is not enough. The visible date and time format should be like `July 21, 2026 at 3:45 PM`.

Each visible `Balance Changes` entry should be compact, not too large. The signed money amount and action text should appear in the top-left corner. The visible created date and created time should appear in the bottom-right corner of the same entry, below the money change text and not too far from it. On mobile, the entry should keep the same layout. Text may wrap only as needed to stay readable, but the money change text should remain in the top-left area and the visible date and time should remain in the bottom-right area.

Visible `Balance Changes` rows should not show the internal visible until date and time.

Visible add and subtract history entries should be kept for 30 days.

The newest visible `Balance Changes` entry should appear first.

For visible `Balance Changes` history, one month means 30 days, not a calendar month.

The user should not choose a date when creating an `Add` or `Subtract` entry.

The website should treat both the created date and created time as user-facing `Balance Changes` details.

The visible date and time format should use the full month name, day, year, `at`, and 12-hour time with uppercase `AM` or `PM`, like `July 21, 2026 at 3:45 PM`.

Visible `Balance Changes` dates and times should use the user's browser/device local date and time.

When a `Balance Changes` entry is created, the website should save both the created date and created time using the user's browser/device local date and time so it can show both values in the visible row and clear the entry after 30 days.

The visible until date should be the created date and time plus 30 days using the same browser/device local date and time rule.

The website should run `Balance Changes` cleanup when it opens and loads saved data.

The website should also run `Balance Changes` cleanup after every successful saved user action.

During cleanup, entries at or after their visible until date and time should be deleted from browser storage and removed from the visible history list.

The first version does not need a background timer that checks old `Balance Changes` entries while the website stays open with no user action. If the website stays open past an entry's visible until date and time, that old entry may remain visible until the next website open or successful saved user action runs cleanup.

The current money amount should be saved separately from the visible history list so removing old history entries does not change the current money amount.

`Add`, `Subtract`, and `Balance Changes` entries should not have notes.

Each add or subtract action should include:

- Amount.
- Action type: added or subtracted.
- Created date and created time shown in the visible row using the format `July 21, 2026 at 3:45 PM` and used for 30-day clearing.
- Internal visible until date and time.

Each `Modify` action should require:

- New total amount.

`Modify` actions should not be included in visible history.

Each saved `Balance Changes` entry should include these data fields:

- Previous money amount.
- New money amount.
- Difference.
- Created date and created time shown in the visible row using the format `July 21, 2026 at 3:45 PM` and used for 30-day clearing.
- Internal visible until date and time.

The previous money amount, new money amount, and internal visible until date and time should not be shown in the visible `Balance Changes` row.

The user should be able to delete a `Balance Changes` entry when they make a mistake.

Touch users should start deleting a `Balance Changes` entry by pressing and holding that entry.

Mouse users should start deleting a `Balance Changes` entry by clicking and holding that entry.

After the press-and-hold or click-and-hold, the website should show a little square on the screen with the exact action texts:

`Delete`

`Cancel`

Clicking `Delete` in the little square should not delete the entry immediately.

Clicking `Cancel` in the little square should close the little square, change nothing, save nothing, and show no message.

Before deleting a `Balance Changes` entry, the website should ask the user to confirm the delete action.

The confirmation message should be exactly `Delete this Balance Change?`.

The confirmation buttons should be exactly `Cancel` and `Delete`.

Deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

After a `Balance Changes` entry is deleted, the website should not offer undo.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it from the visible history. If the main money amount is wrong, the user should use `Modify` to correct the main money amount.

Deleting a `Balance Changes` entry manually is different from the website deleting an old visible history entry after 30 days. Both actions remove entries from the visible history only. Neither action should change the main money amount.

The `Add`, `Subtract`, and `Modify` actions should appear visually connected to the main money amount so the user understands they directly change the displayed money amount.

## Balance Changes Behavior

`Balance Changes` is the user-facing history list for money amount changes.

`Balance Changes` should include:

- Added money.
- Subtracted money.

`Balance Changes` is a visible history record. It should not control or recalculate the main money amount.

`Balance Changes` should not include `Saving` square changes, because `Saving` square changes do not change the main money amount.

`Balance Changes` should appear directly under the main money amount on the dashboard.

`Balance Changes` entries should be ordered newest first by their created date and time.

If there are no `Balance Changes` entries, the `Balance Changes` history should simply be empty.

There should not be a separate `Recent Activity` area for the first version.

There should not be a separate `View history` action or button for `Balance Changes`.

If the history list has too many entries to fit on the screen, the user should be able to scroll down to see more entries.

## Dashboard And User Flow

The first screen should look like a simple bank dashboard.

The main money amount should be the most visible information on the dashboard.

The dashboard should show the main money amount with the label `Current Balance`.

On the dashboard, `Savings` should appear under `Balance Changes`.

The website should feel trustworthy, calm, and practical.

The user should not need financial knowledge to use it.

Main actions should be easy to find:

- Click the money amount.
- Add money.
- Subtract money.
- Modify amount.
- Click `Savings`.

The interface should work well on mobile.

Main user flow:

1. User opens the website.
2. If no saved data exists yet, user sees `0.00$` labeled as `Current Balance`.
3. User clicks the main money amount.
4. Website shows only `Add`.
5. User uses `Add` to add money.
6. Website saves the added money as `+{money amount} added`.
7. After the money amount is greater than `0.00$`, clicking the main money amount shows `Add`, `Subtract`, and `Modify`.
8. User uses `Add` for new cash, `Subtract` for spent or removed cash, and `Modify` to correct mistakes without creating history or notifications.
9. User sees money amount change history directly under the main money amount.
10. User scrolls down if the history list is longer than the screen.
11. User checks `Saving` squares.

## Savings Behavior

The website should have a `Savings` section that can be reached from the main dashboard.

`Savings` is the separate planning section of the website.

A `Saving` is one square inside the `Savings` section.

The user should click `Savings` to open the `Savings` section.

There should not be a separate `Open savings` action.

When the user opens `Savings`, the `Savings` view should take the entire screen.

The full-screen `Savings` view should show a small `<` sign in the top-left corner.

When the user clicks `<`, the website should return to the dashboard. This back action should change nothing, save nothing, create no `Balance Changes` entry, update no dates, and show no message.

The `Savings` section is a planning view. It helps the user see which `Saving` squares are fully covered, partly covered, or not covered by the main money amount.

The `Savings` section should start from the current money amount.

The `Savings` section should show a money amount at the top. This money amount is what remains after the website checks the ordered `Saving` squares against the main money amount. It should never show less than `0.00$`.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`.

The `Savings money amount` label is user-facing website text. In the specs, the concept can still be described as the money amount shown inside `Savings`.

The `Savings money amount` label should not be used for the main money amount. The main money amount should keep the `Current Balance` label.

Under the money amount shown inside `Savings`, the website should show `Saving` squares.

The full-screen `Savings` view should keep showing the money amount inside `Savings` and the `Saving` squares together. The money amount shown inside `Savings` should go to the visible `Saving` squares from top to bottom as a planning display only.

When there are no visible `Saving` squares in the `Saving` squares area, the website should show the circle `+` action in the middle of that area.

The centered `+` action is the empty state for no `Saving` squares. The website should not show a separate empty-state text sentence for no `Saving` squares.

After at least one `Saving` square exists, the circle `+` action should appear at the top-left of the `Saving` squares area, above or before the visible squares.

In its default state, each visible `Saving` square should show the `Saving` name in the top-left corner.

In its default state, each visible `Saving` square should show the planned money amount on the right side of the same top row.

In its default state, each visible `Saving` square should show the thin coverage bar at the bottom.

Clicking a `Saving` square should change that same square into its action state.

In the action state, the square should still show the `Saving` name in the top-left corner and the planned money amount on the right side of the same top row.

In the action state, the square should show a `Delete` text action at the bottom center.

In the action state, the square should not show the thin coverage bar.

In the action state, clicking the `Saving` name should open the rename flow for that square.

In the action state, clicking the planned money amount should open the planned-money-amount change flow for that square.

In the action state, clicking `Delete` should start the delete confirmation for that square.

The website should not open a separate action menu or larger action square for rename, planned-money-amount change, and delete.

The user should add a new `Saving` square with a circle action that contains a `+` sign.

Clicking the `+` action should open the same create flow whether the `+` is centered in the empty `Saving` squares area or placed at the top-left when `Saving` squares already exist.

When the user clicks the `+` action to add a new `Saving` square, the website should ask for:

- `Saving` name.
- Planned money amount.

The `Saving` square should be created only after the user enters a valid name and a planned money amount greater than `0.00$`.

If the user tries to save a new `Saving` square without a `Saving` name, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a `Saving` name or cancels creating that square.

If the user tries to save a new `Saving` square without a planned money amount, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, and the create flow should stay open until the user enters a planned money amount or cancels creating that square.

The planned money amount may include cents, with up to two digits after the decimal point. The user should enter only the number, without typing the `$` sign. The user may type a decimal point, such as `14.56`. The planned money amount input should request a decimal numeric keyboard on devices that support it, so the user gets number keys `0` through `9` and a decimal point. The planned money amount input should use its own planned-money typing filter: letters do not change the field, and the first accepted character must be a number from `0` through `9`. The user should not be able to paste into the planned money amount input. If the user tries to paste letters, numbers, symbols, or any other content into the planned money amount input, the pasted content should not appear, the input should keep its previous value, and no message should appear. The website should save and show that money amount with the `$` sign at the end, such as `14.56$`, not as integer cents like `1456`. The zero money amount should show as `0.00$`, nonzero whole money amounts should show with two digits after the decimal point such as `14.00$`, and nonzero money amounts with cents should show with two digits after the decimal point such as `14.50$`.

`Saving` square create, rename, planned-money-amount change, and broken-square fix input flows should show two text actions at the bottom: `Save` and `Cancel`.

In `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows, `Save` should try to save the entered values using the rules for that action.

In `Saving` square create, rename, planned-money-amount change, and broken-square fix input flows, `Cancel` should close the input flow, return to the `Saving` squares view, change nothing, save nothing, create no `Balance Changes` entry, update no dates, and show no message.

The `Saving` square name must be unique inside `Savings`.

The `Saving` square name should not have a maximum length. The user can enter a one-letter name, a number-only name, a name with numbers before or after words, multiple words, a full sentence, symbols, punctuation, emoji characters, or a very long name.

The website should trim spaces at the beginning and end of the `Saving` square name before saving it. Spaces inside the trimmed name should stay.

Very long `Saving` square names should stay usable in the `Saving` square layout. The website should wrap or contain the name so it does not create horizontal page overflow, overlap other square content, or get rejected only because it is long.

The website should treat names as duplicates when they match after trimming spaces and ignoring uppercase or lowercase letters.

If the user enters a name already used by another `Saving` square, the website should not create the new `Saving` square.

If the user enters a duplicate name while creating a `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message. No new `Saving` square should appear, saved data should stay unchanged, and `Balance Changes` should not get a new entry.

If the user cancels or does not enter both required values, the website should not create the `Saving` square.

If saved browser data can be loaded but one saved `Saving` square is broken, the website should keep the rest of the saved data.

The broken `Saving` square should still appear in `Savings` as an error square with this exact text:

`Saving could not be loaded.`

The broken `Saving` square should show actions exactly named `Fix` and `Delete`.

A broken `Saving` square should not use any money amount inside `Savings`, should not lower the `Savings money amount`, should not show a coverage bar, and should not affect coverage bars for valid `Saving` squares.

Clicking `Fix` on a broken `Saving` square should ask the user for:

- `Saving` name.
- Planned money amount greater than `0.00$`.

If the user saves valid fix values, the website should turn the broken square into a normal `Saving` square and keep it in the same visible position when possible.

Saving a fixed `Saving` square should save browser storage, recalculate the `Savings money amount`, recalculate coverage bars, create no `Balance Changes` entry, and show no message.

If the user clicks `Cancel` while fixing a broken `Saving` square, the broken square should stay broken and nothing should change.

Clicking `Delete` on a broken `Saving` square should ask for confirmation with the exact message `Delete this Saving?` and buttons exactly named `Cancel` and `Delete`.

Confirming delete for a broken `Saving` square should remove only that broken square, save browser storage, recalculate the `Savings money amount`, recalculate coverage bars, create no `Balance Changes` entry, update no other `Saving` squares, show no message, and offer no undo.

If the user enters `0.00$` as the planned money amount while creating a `Saving` square, the website should not create the `Saving` square.

If the user tries to save a new `Saving` square with a planned money amount of `0.00$`, the website should do nothing. No message should appear, no new `Saving` square should be created, saved data should stay unchanged, `Balance Changes` should not get a new entry, and the same `Saving` square create input step should stay open until the user enters a planned money amount greater than `0.00$` or cancels.

When a new `Saving` square create attempt has more than one invalid value, the website should check the create inputs in this order:

1. Planned money amount is missing or `0.00$`.
2. `Saving` name is missing.
3. `Saving` name is duplicate.

The first matching rule decides what happens. If the user enters a duplicate `Saving` name and also leaves the planned money amount empty or enters `0.00$`, the website should keep the same `Saving` square create input step open with no message, no saved data change, no new `Saving` square, and no `Balance Changes` entry. Duplicate-name create behavior should happen only after the planned money amount is greater than `0.00$` and the `Saving` name is not empty.

Each `Saving` square stores the planned money amount chosen by the user.

The planned money amount in a `Saving` square does not mean money has moved into a separate place. It does not change the main money amount.

Each `Saving` square should keep the planned money amount chosen by the user. If the main money amount changes, the `Saving` square planned money amount should not change automatically.

When the user renames an existing `Saving` square:

- The user should start renaming by opening the square's action state and clicking the `Saving` name shown in the square.
- The website should ask for the new `Saving` name.
- The new name should be required.
- The new name should be unique inside `Savings`.
- The website should treat names as duplicates when they match after trimming spaces and ignoring uppercase or lowercase letters.
- If the user tries to save an empty new name, the website should do nothing.
- If the user tries to save an empty new name, no message should appear, the old name should stay saved, saved data should stay unchanged, and the rename flow should stay open until the user enters a `Saving` name or cancels renaming that square.
- If the new name is already used by another `Saving` square, the website should return to the `Saving` squares view without showing a duplicate-name error message.
- If the new name is already used by another `Saving` square, the `Saving` square should keep its old name, saved data should stay unchanged, and `Balance Changes` should not get a new entry.
- If the new name is valid, the website should replace the old name, update that square's updated date, and save the change in browser storage.
- Renaming a `Saving` square should not change the main money amount.
- Renaming a `Saving` square should not change the money amount shown inside `Savings` or coverage bars.
- Renaming a `Saving` square should not create a `Balance Changes` entry.

When the user changes the planned money amount in an existing `Saving` square:

- The user should start changing the planned money amount by opening the square's action state and clicking the planned money amount shown in the square.
- The website should ask for the new total planned money amount.
- If the user tries to save an empty planned money amount, the website should do nothing.
- If the user tries to save an empty planned money amount, no message should appear, the old planned money amount should stay saved, saved data should stay unchanged, and the planned-money-amount change flow should stay open until the user enters a planned money amount or cancels changing that square.
- The entered amount should replace the old planned money amount.
- The website should not add the entered amount to the old planned money amount.
- The website should not subtract the entered amount from the old planned money amount.
- The main money amount should not change.
- The money amount shown inside `Savings` should update.
- The coverage bars should update.
- The change should be saved in browser storage.
- The change should not create a `Balance Changes` entry.
- The change should not display as `+{money amount} added` or `-{money amount} subtracted`.

If the user changes an existing `Saving` square planned money amount to `0.00$`:

- The `Saving` square should disappear from `Savings`.
- The `Saving` square should be removed from browser storage.
- The main money amount should not change.
- The money amount shown inside `Savings` should update.
- Coverage bars for remaining `Saving` squares should update.
- The website should not create a `Balance Changes` entry.

`Saving` squares should have a visible order:

- The first `Saving` square the user creates should stay at the top by default.
- The visible order should be the coverage order.
- The website should check `Saving` squares from top to bottom.
- Touch users should be able to change the order by holding and moving the whole `Saving` square.
- Mouse users should be able to change the order by clicking, holding, and dragging the whole `Saving` square.
- Holding or dragging a `Saving` square should not open rename, planned-money-amount change, or delete.
- If the user moves a `Saving` square to the top, the website should check that square first.
- The moved square's coverage bar should be calculated before lower squares use any remaining money amount.
- When the user finishes moving a `Saving` square and lets go, the website should save the new order in browser storage.
- After the move finishes, the money amount shown inside `Savings` and the coverage bars should update from the new visible order.
- Reordering `Saving` squares should not change the main money amount and should not create a `Balance Changes` entry.

When the user deletes a `Saving` square:

- The user should start deleting by opening the square's action state and clicking the `Delete` text action shown at the bottom center of the square.
- The website should ask the user to confirm the delete action.
- The confirmation message should be exactly `Delete this Saving?`.
- The confirmation buttons should be exactly `Cancel` and `Delete`.
- Only that `Saving` square and the details inside it should be removed from `Savings`.
- The main money amount should not change.
- `Balance Changes` should not get a new entry.
- Other `Saving` squares should keep their names, planned money amounts, and relative order.
- The money amount shown inside `Savings` and the coverage bars should update from the remaining `Saving` squares because they are calculated display values.
- After the `Saving` square is deleted, the website should not offer undo.

The website should calculate `Saving` square coverage like this:

- Start with the main money amount.
- Check the first `Saving` square.
- Use as much of the remaining money amount as needed to cover that square.
- Move to the next `Saving` square with whatever money amount is still remaining.
- Continue until all `Saving` squares have been checked or the remaining money amount is `0.00$`.

Each `Saving` square should show a thin horizontal coverage bar at the very bottom of the square in its default state:

- The bar is grey by default.
- The bar fills green from left to right based on the covered percentage of that square's planned money amount.
- If a square is fully covered, the bar is fully green.
- If a square is partly covered, the left part of the bar is green and the remaining right part is grey.
- If a square is not covered, the bar is fully grey.

If a `Saving` square is 80% covered, the left 80% of the coverage bar should be green and the right 20% should be grey.

If a `Saving` square is not fully covered, the square should show a small `{money amount} needed` note at the top-left of the bottom coverage bar.

The money amount shown inside `Savings` should be calculated as:

- Current money amount minus the total planned money amount in `Saving` squares, with the result stopped at `0.00$`.

Example:

- Current money amount is `350.00$`.
- Total planned money amount in `Saving` squares is `0.00$`.
- Money amount shown inside `Savings` is `350.00$`.

If the user creates a `Rent` `Saving` square with a planned money amount of `40.00$`:

- Current money amount stays `350.00$`.
- `Rent` square shows `40.00$`.
- Total planned money amount in `Saving` squares is `40.00$`.
- Money amount shown inside `Savings` becomes `310.00$`.

This means the user's money amount is still `350.00$`, but the `Savings` section shows that `40.00$` is planned for rent and `310.00$` is still available for other plans.

Example with ordered coverage:

- Current money amount is `85.00$`.
- `Rent` is the first `Saving` square and has `50.00$`.
- `Food` is the second `Saving` square and has `20.00$`.
- `School` is the third `Saving` square and has `30.00$`.
- `Rent` is fully covered, so its bar is 100% green.
- `Food` is fully covered, so its bar is 100% green.
- `School` is partly covered with `15.00$` of `30.00$`, so the left 50% of its bar is green and the right 50% is grey.
- `School` shows `15.00$ needed` at the top-left of its bottom coverage bar.
- Money amount shown inside `Savings` is `0.00$`.

If the user moves `School` to the top in this example:

- `School` is checked first and becomes fully covered.
- `Rent` is checked second and becomes fully covered.
- `Food` is checked third with `5.00$` of `20.00$`, so the left 25% of its bar is green and the right 75% is grey.
- `Food` shows `15.00$ needed` at the top-left of its bottom coverage bar.
- Money amount shown inside `Savings` stays `0.00$`.

Creating a `Saving` square with a planned money amount is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

Changing a `Saving` square planned money amount is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

`Saving` square changes should not display as `+{money amount} added` or `-{money amount} subtracted`.

`Saving` square changes should not appear in `Balance Changes`.

Each `Saving` square should include:

- ID.
- Name.
- Planned money amount greater than `0.00$`.
- Order.
- Created date.
- Updated date.

The user should be able to:

- Create a `Saving` square.
- Rename a `Saving` square.
- Change the planned money amount in a `Saving` square.
- Delete a `Saving` square.
- Reorder `Saving` squares by holding and moving the whole square or by mouse dragging the whole square.

If the current money amount becomes lower than the total planned money amount in `Saving` squares, the website should not change the `Saving` square planned money amounts automatically. It should update the coverage bars and show the money amount inside `Savings` as `0.00$`.

## Saved Data User Behavior

The website should restore saved data when the user refreshes, closes, or opens the website again later and the saved data is available.

If no saved data exists, the website should show the dashboard with the money amount set to `0.00$`.

If no saved data exists, the website should not create saved browser data only because it showed the default `0.00$` money amount.

The website should start saving data after the first successful saved user action. In the normal first money flow, this is the first successful `Add`.

The website should load saved browser data only when the data version value is exactly `1`.

If saved browser data has a missing, wrong, future, unreadable, or unrecognized data version, the website should treat it as broken saved data.

If saved data is broken or cannot be read, the website should show this message:

`Saved data could not be loaded.`

The website should show a `Start again` action with that message.

When the user clicks `Start again`:

- The website should delete the broken saved data from browser storage.
- The website should immediately create fresh browser storage data with the money amount set to `0.00$` and data version `1`.
- `Balance Changes` should be empty.
- `Savings` should have no saved `Saving` squares.

If the saved browser data file can be read and has data version `1`, but one saved `Saving` square inside it is broken, the website should not show the full saved-data error message just because of that one broken square. It should load the rest of the saved data and show the broken `Saving` square error state in `Savings`.

The website should not show user-facing text that explains where saved data is stored.

The website should not show user-facing text that says saved data belongs only to the same browser or device.

The website should not show user-facing text that warns saved data may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

Technical browser storage rules are defined in `docs/specs/technical/technical1.md`.
