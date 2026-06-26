# Functional Spec: Cash Money Organizer Website

## Initial Amount and Money Amount Controls

When a new user has not entered any money amount yet, the website should ask for the first money amount.

When the first money amount is saved:

- The entered amount becomes the starting money amount.
- The action is saved in history as `+{money amount} added`.
- The history entry follows the same visible history rules as a normal `Add` entry.
- The history entry stays visible in `Balance Changes` for 30 days.
- The user can delete the history entry from visible history.
- Deleting the history entry does not change the main money amount.
- The website should then show the main money amount with the user-facing label `Current Balance`.

After the first money amount exists, the main money amount should be clickable.

When the user clicks the main money amount, the website should show three actions near the main money amount:

- `Modify` for correcting the total displayed amount.
- `Add` for adding money to the main money amount.
- `Subtract` for subtracting money from the main money amount.

The dashboard should use `Current Balance` as the label for the main money amount.

The `Current Balance` label is user-facing website text. In the specs, the concept should be described as the money amount.

These controls are manual money tracking actions. They do not represent a real bank deposit, bank withdrawal, or payment.

The user can add, subtract, or manually modify the money amount as often or as rarely as they want. There should be no daily, weekly, monthly, or yearly limit on money amount changes.

When the user chooses `Modify`:

- The website asks for the corrected total money amount.
- The entered amount replaces the current money amount.
- The new current money amount is saved in browser storage.
- The action should not create a history entry.
- The action should not create a `Balance Changes` entry.
- The action should not create a notification.
- The action should not display as `+{money amount} added`, `-{money amount} subtracted`, or a manual correction.

When the user chooses `Add`:

- The website asks for the amount to add.
- The entered amount is added to the main money amount.
- The action is saved in `Balance Changes` as a positive entry.
- The history entry should display as `+{money amount} added`.
- The history entry should stay separate from other add or subtract entries.

When the user chooses `Subtract`:

- The website asks for the amount to subtract.
- The website subtracts up to the current money amount.
- The action is saved in `Balance Changes` as a negative entry.
- The history entry should display as `-{money amount} subtracted`.
- The history entry should stay separate from other add or subtract entries.

The main money amount should never go below `$0`.

If the user enters a subtract amount greater than the current money amount:

- The website should set the main money amount to `$0`.
- The website should not show a negative money amount.
- The visible history entry should use the actual amount removed from the money amount.

Example:

- Current money amount is `$20`.
- User enters `$50` in `Subtract`.
- New money amount becomes `$0`.
- Visible history shows `-$20 subtracted`.
- Visible history should not show `-$50 subtracted`.

If the current money amount is already `$0`, `Subtract` should keep the money amount at `$0` and should not create a visible history entry.

The website should not combine separate add and subtract actions into one history result.

Example:

- Show `+$56 added`.
- Show `-$34 subtracted`.
- Do not replace those entries with only `+$22 net change`.

Visible add and subtract history entries should be kept for 30 days.

For visible `Balance Changes` history, one month means 30 days, not a calendar month.

The user should not choose a date when creating an `Add` or `Subtract` entry.

The website should not treat dates as a user-facing `Balance Changes` feature.

When a `Balance Changes` entry is created, the website should save the created date and time internally only so it can clear the entry after 30 days.

The visible until date should be the internal created date and time plus 30 days.

At or after the visible until date and time, the entry may be hidden or removed from the visible history list.

The current money amount should be saved separately from the visible history list so removing old history entries does not change the current money amount.

`Add`, `Subtract`, and `Balance Changes` entries should not have notes.

Each add or subtract action should include:

- Amount.
- Action type: added or subtracted.
- Internal created date and time used only for 30-day clearing.
- Internal visible until date and time.

Each `Modify` action should require:

- New total amount.

`Modify` actions should not be included in visible history.

Each `Balance Changes` entry should include:

- Previous money amount.
- New money amount.
- Difference.
- Internal created date and time used only for 30-day clearing.
- Internal visible until date and time.

The user should be able to delete a `Balance Changes` entry when they make a mistake.

Deleting a `Balance Changes` entry only removes that entry from the visible history. It does not change, recalculate, or reverse the main money amount.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it from the visible history. If the main money amount is wrong, the user should use `Modify` to correct the main money amount.

Deleting a `Balance Changes` entry is different from hiding or removing an old visible history entry after 30 days. Both actions remove entries from the visible history only. Neither action should change the main money amount.

The `Modify`, `Add`, and `Subtract` actions should appear visually connected to the main money amount so the user understands they directly change the displayed money amount.

## Balance Changes Behavior

`Balance Changes` is the user-facing history list for money amount changes.

`Balance Changes` should include:

- Added money.
- Subtracted money.

`Balance Changes` is a visible history record. It should not control or recalculate the main money amount.

`Balance Changes` should not include `Saving` square changes, because `Saving` square changes do not change the main money amount.

`Balance Changes` should appear directly under the main money amount on the dashboard.

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
- Modify amount.
- Add money.
- Subtract money.
- Click `Savings`.

The interface should work well on mobile.

Main user flow:

1. User opens the website.
2. If no money amount exists yet, user enters their first money amount.
3. Website records the first amount as `+{money amount} added`.
4. User sees their current money amount labeled as `Current Balance`.
5. User clicks the main money amount to show `Modify`, `Add`, and `Subtract`.
6. User uses `Add` for new cash, `Subtract` for spent or removed cash, and `Modify` to correct mistakes without creating history or notifications.
7. User sees money amount change history directly under the main money amount.
8. User scrolls down if the history list is longer than the screen.
9. User checks `Saving` squares.

## Savings Behavior

The website should have a `Savings` section that can be reached from the main dashboard.

`Savings` is the separate planning section of the website.

A `Saving` is one square inside the `Savings` section.

The user should click `Savings` to open the `Savings` section.

There should not be a separate `Open savings` action.

The `Savings` section is a planning view. It helps the user see which `Saving` squares are fully covered, partly covered, or not covered by the main money amount.

The `Savings` section should start from the current money amount.

The `Savings` section should show a money amount at the top. This money amount is what remains after the website checks the ordered `Saving` squares against the main money amount. It should never show less than `$0`.

The money amount shown at the top of `Savings` should use the exact user-facing label `Savings money amount`.

The `Savings money amount` label is user-facing website text. In the specs, the concept can still be described as the money amount shown inside `Savings`.

The `Savings money amount` label should not be used for the main money amount. The main money amount should keep the `Current Balance` label.

Under the money amount shown inside `Savings`, the website should show `Saving` squares.

The user should add a new `Saving` square with a circle action that contains a `+` sign.

When the user clicks the `+` action to add a new `Saving` square, the website should ask for:

- `Saving` name.
- Planned money amount.

The `Saving` square should be created only after the user enters a valid name and a planned money amount greater than `$0`.

The `Saving` square name must be unique inside `Savings`.

The website should treat names as duplicates when they match after trimming spaces and ignoring uppercase or lowercase letters.

If the user enters a name already used by another `Saving` square, the website should not create the new `Saving` square.

If the user cancels or does not enter both required values, the website should not create the `Saving` square.

If the user enters `$0` as the planned money amount while creating a `Saving` square, the website should not create the `Saving` square.

Each `Saving` square stores the planned money amount chosen by the user.

The planned money amount in a `Saving` square does not mean money has moved into a separate place. It does not change the main money amount.

Each `Saving` square should keep the planned money amount chosen by the user. If the main money amount changes, the `Saving` square planned money amount should not change automatically.

When the user renames an existing `Saving` square:

- The website should ask for the new `Saving` name.
- The new name should be required.
- The new name should be unique inside `Savings`.
- The website should treat names as duplicates when they match after trimming spaces and ignoring uppercase or lowercase letters.
- If the new name is empty or already used by another `Saving` square, the website should keep the old name.
- If the new name is valid, the website should replace the old name, update that square's updated date, and save the change in browser storage.
- Renaming a `Saving` square should not change the main money amount.
- Renaming a `Saving` square should not change the money amount shown inside `Savings` or coverage bars.
- Renaming a `Saving` square should not create a `Balance Changes` entry.

When the user changes the planned money amount in an existing `Saving` square:

- The website should ask for the new total planned money amount.
- The entered amount should replace the old planned money amount.
- The website should not add the entered amount to the old planned money amount.
- The website should not subtract the entered amount from the old planned money amount.
- The main money amount should not change.
- The money amount shown inside `Savings` should update.
- The coverage bars should update.
- The change should be saved in browser storage.
- The change should not create a `Balance Changes` entry.
- The change should not display as `+{money amount} added` or `-{money amount} subtracted`.

If the user changes an existing `Saving` square planned money amount to `$0`:

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
- The user should be able to change the order by holding and moving `Saving` squares.
- If the user moves a `Saving` square to the top, the website should check that square first.
- The moved square's coverage bar should be calculated before lower squares use any remaining money amount.
- When the user finishes moving a `Saving` square and lets go, the website should save the new order in browser storage.
- After the move finishes, the money amount shown inside `Savings` and the coverage bars should update from the new visible order.
- Reordering `Saving` squares should not change the main money amount and should not create a `Balance Changes` entry.

When the user deletes a `Saving` square:

- Only that `Saving` square and the details inside it should be removed from `Savings`.
- The main money amount should not change.
- `Balance Changes` should not get a new entry.
- Other `Saving` squares should keep their names, planned money amounts, and relative order.
- The money amount shown inside `Savings` and the coverage bars should update from the remaining `Saving` squares because they are calculated display values.

The website should calculate `Saving` square coverage like this:

- Start with the main money amount.
- Check the first `Saving` square.
- Use as much of the remaining money amount as needed to cover that square.
- Move to the next `Saving` square with whatever money amount is still remaining.
- Continue until all `Saving` squares have been checked or the remaining money amount is `$0`.

Each `Saving` square should show a thin horizontal coverage bar at the very bottom of the square:

- The bar is grey by default.
- The bar fills green from left to right based on the covered percentage of that square's planned money amount.
- If a square is fully covered, the bar is fully green.
- If a square is partly covered, the left part of the bar is green and the remaining right part is grey.
- If a square is not covered, the bar is fully grey.

If a `Saving` square is 80% covered, the left 80% of the coverage bar should be green and the right 20% should be grey.

If a `Saving` square is not fully covered, the square should show a small `{money amount} needed` note at the top-left of the bottom coverage bar.

The money amount shown inside `Savings` should be calculated as:

- Current money amount minus the total planned money amount in `Saving` squares, with the result stopped at `$0`.

Example:

- Current money amount is `$350`.
- Total planned money amount in `Saving` squares is `$0`.
- Money amount shown inside `Savings` is `$350`.

If the user creates a `Rent` `Saving` square with a planned money amount of `$40`:

- Current money amount stays `$350`.
- `Rent` square shows `$40`.
- Total planned money amount in `Saving` squares is `$40`.
- Money amount shown inside `Savings` becomes `$310`.

This means the user's money amount is still `$350`, but the `Savings` section shows that `$40` is planned for rent and `$310` is still available for other plans.

Example with ordered coverage:

- Current money amount is `$85`.
- `Rent` is the first `Saving` square and has `$50`.
- `Food` is the second `Saving` square and has `$20`.
- `School` is the third `Saving` square and has `$30`.
- `Rent` is fully covered, so its bar is 100% green.
- `Food` is fully covered, so its bar is 100% green.
- `School` is partly covered with `$15` of `$30`, so the left 50% of its bar is green and the right 50% is grey.
- `School` shows `$15 needed` at the top-left of its bottom coverage bar.
- Money amount shown inside `Savings` is `$0`.

If the user moves `School` to the top in this example:

- `School` is checked first and becomes fully covered.
- `Rent` is checked second and becomes fully covered.
- `Food` is checked third with `$5` of `$20`, so the left 25% of its bar is green and the right 75% is grey.
- `Food` shows `$15 needed` at the top-left of its bottom coverage bar.
- Money amount shown inside `Savings` stays `$0`.

Creating a `Saving` square with a planned money amount is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

Changing a `Saving` square planned money amount is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

`Saving` square changes should not display as `+{money amount} added` or `-{money amount} subtracted`.

`Saving` square changes should not appear in `Balance Changes`.

Each `Saving` square should include:

- ID.
- Name.
- Planned money amount greater than `$0`.
- Order.
- Created date.
- Updated date.

The user should be able to:

- Create a `Saving` square.
- Rename a `Saving` square.
- Change the planned money amount in a `Saving` square.
- Delete a `Saving` square.
- Reorder `Saving` squares by holding and moving them.

If the current money amount becomes lower than the total planned money amount in `Saving` squares, the website should not change the `Saving` square planned money amounts automatically. It should update the coverage bars and show the money amount inside `Savings` as `$0`.

## Saved Data User Behavior

The website should restore saved data when the user refreshes, closes, or opens the website again later and the saved data is available.

If no saved data exists, the website should show the first money amount setup.

The website should not show user-facing text that explains where saved data is stored.

The website should not show user-facing text that says saved data belongs only to the same browser or device.

The website should not show user-facing text that warns saved data may disappear after changing browser, changing device, using private browsing, clearing browser data, clearing site data, or uninstalling the browser.

Technical browser storage rules are defined in `docs/specs/technical/technical1.md`.
