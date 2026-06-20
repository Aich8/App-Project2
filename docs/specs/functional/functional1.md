# Functional Spec: Cash Money Organizer Website

## Initial Amount and Money Amount Controls

When a new user has not entered any money amount yet, the website should ask for the first money amount.

When the first money amount is saved:

- The entered amount becomes the starting money amount.
- The action is saved in history as `+{money amount} added`.
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

Visible add and subtract history entries should be kept for one month.

Entries older than one month may be hidden or removed from the visible history list.

The current money amount should be saved separately from the visible history list so removing old history entries does not change the current money amount.

Each add or subtract action should include:

- Amount.
- Date.
- Action type: added or subtracted.
- Visible until date.
- Optional note.

Each `Modify` action should require:

- New total amount.

`Modify` actions should not be included in visible history.

Each `Balance Changes` entry should include:

- Previous money amount.
- New money amount.
- Difference.
- Date.
- Optional note.

The user should be able to delete a `Balance Changes` entry when they make a mistake.

The user should not be able to edit a saved `Balance Changes` entry. If the saved entry is wrong, the user should delete it and record the correct `Add` or `Subtract` action.

Deleting a `Balance Changes` entry is different from hiding or removing an old visible history entry after one month. Deleting an entry because of a mistake should update the money amount consistently. Hiding or removing old visible history entries after one month should not change the money amount.

The `Modify`, `Add`, and `Subtract` actions should appear visually connected to the main money amount so the user understands they directly change the displayed money amount.

## Balance Changes Behavior

`Balance Changes` is the user-facing history list for money amount changes.

`Balance Changes` should include:

- Added money.
- Subtracted money.

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

The `Savings` section is a planning view. It helps the user see what money would be left after setting some money aside.

The `Savings` section should start from the current money amount.

The `Savings` section should show a money amount at the top.

Under the money amount shown inside `Savings`, the website should show `Saving` squares.

When the user clicks an unnamed `Saving` square, the website should ask them to `Name the saving`.

Each `Saving` square can hold a money amount.

The money amount shown inside `Savings` should be calculated as:

- Current money amount minus the total money set aside in `Saving` squares.

Example:

- Current money amount is `$350`.
- Total money set aside in `Saving` squares is `$0`.
- Money amount shown inside `Savings` is `$350`.

If the user creates a `Rent` `Saving` square and puts `$40` inside it:

- Current money amount stays `$350`.
- `Rent` square shows `$40`.
- Total money set aside in `Saving` squares is `$40`.
- Money amount shown inside `Savings` becomes `$310`.

This means the user's money amount is still `$350`, but the `Savings` section shows that `$40` is planned for rent and `$310` is still available for other plans.

Putting money into a `Saving` square is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

`Saving` square changes should not display as `+{money amount} added` or `-{money amount} subtracted`.

`Saving` square changes should not appear in `Balance Changes`.

Each `Saving` square should include:

- ID.
- Name.
- Amount set aside.
- Created date.
- Updated date.

The user should be able to:

- Create a `Saving` square.
- Rename a `Saving` square.
- Put money aside in a `Saving` square.
- Remove money from a `Saving` square so it becomes available again inside the `Savings` section.
- Delete a `Saving` square.

The user should not be allowed to put more money into `Saving` squares than the current money amount shown inside `Savings`.

If the current money amount becomes lower than the total money set aside in `Saving` squares, the website should show a clear warning and ask the user to adjust the `Saving` squares.

## Browser Storage User Behavior

The website should restore saved data when the user refreshes, closes, or opens the website again later in the same browser.

If no saved data exists, the website should show the first money amount setup.

The website should explain that saved data belongs only to the same device and browser unless online accounts are added in a future version.

Technical browser storage rules are defined in `docs/specs/technical/technical1.md`.
