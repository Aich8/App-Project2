# Functional Spec: Cash Money Organizer Website

## Initial Amount and Money Amount Controls

When a new user has not entered any money amount yet, the website should ask for the first money amount.

When the first money amount is saved:

- The entered amount becomes the starting money amount.
- The action is saved in history as `+{money amount} added`.
- The website should then show the main money amount with the user-facing label `Current Balance`.

After the first money amount exists, the website should show three actions near the main money amount:

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
- The action should not create an activity entry.
- The action should not create a notification.
- The action should not display as `+{money amount} added`, `-{money amount} subtracted`, or a manual correction.

When the user chooses `Add`:

- The website asks for the amount to add.
- The entered amount is added to the main money amount.
- The action is saved in visible activity history as a positive entry.
- The history entry should display as `+{money amount} added`.
- The history entry should stay separate from other add or subtract entries.

When the user chooses `Subtract`:

- The website asks for the amount to subtract.
- The website subtracts up to the current money amount.
- The action is saved in visible activity history as a negative entry.
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

Each add or subtract activity entry should include:

- Previous money amount.
- New money amount.
- Difference.
- Date.
- Optional note.

The user should be able to edit or delete entries when they make a mistake.

The `Modify`, `Add`, and `Subtract` actions should appear visually connected to the main money amount so the user understands they directly change the displayed money amount.

## Dashboard And User Flow

The first screen should look like a simple bank dashboard.

The main money amount should be the most visible information on the dashboard.

The dashboard should show the main money amount with the label `Current Balance`.

The website should feel trustworthy, calm, and practical.

The user should not need financial knowledge to use it.

Main actions should be easy to find:

- Modify amount.
- Add money.
- Subtract money.
- Open savings.
- Manage envelopes.
- View history.

The interface should work well on mobile.

Main user flow:

1. User opens the website.
2. If no money amount exists yet, user enters their first money amount.
3. Website records the first amount as `+{money amount} added`.
4. User sees their current money amount labeled as `Current Balance`.
5. User uses `Add` for new cash, `Subtract` for spent or removed cash, and `Modify` to correct mistakes without creating history or notifications.
6. User checks savings folders or squares and cash envelopes.
7. User reviews history to understand money amount changes.

## Savings Behavior

The website should have a `Savings` section that can be opened from the main dashboard.

The `Savings` section is a planning view. It helps the user see what money would be left after setting some money aside.

The `Savings` section should start from the current money amount.

The `Savings` section should show a savings available amount at the top.

The savings available amount should be calculated as:

- Current money amount minus the total money set aside in savings folders or squares.

Example:

- Current money amount is `$350`.
- Total money set aside in savings folders or squares is `$0`.
- Savings available amount is `$350`.

If the user creates a `Rent` folder or square and puts `$40` inside it:

- Current money amount stays `$350`.
- `Rent` folder or square shows `$40`.
- Total money set aside in savings folders or squares is `$40`.
- Savings available amount becomes `$310`.

This means the user's money amount is still `$350`, but the `Savings` section shows that `$40` is planned for rent and `$310` is still available for other plans.

Putting money into a savings folder or square is a planning action. It is not an `Add`, `Subtract`, or `Modify` action.

Savings folder or square changes should not display as `+{money amount} added` or `-{money amount} subtracted`.

Savings folder or square changes may appear in activity history as savings organization entries, such as:

- `Set aside $40 for Rent`.
- `Removed $10 from Rent`.

Each savings folder or square should include:

- ID.
- Name.
- Amount set aside.
- Created date.
- Updated date.

The user should be able to:

- Create a savings folder or square.
- Rename a savings folder or square.
- Put money aside in a savings folder or square.
- Remove money from a savings folder or square so it becomes available again inside the `Savings` section.
- Delete a savings folder or square.

The user should not be allowed to put more money into savings folders or squares than the current savings available amount.

If the current money amount becomes lower than the total money set aside in savings folders or squares, the website should show a clear warning and ask the user to adjust the folders or squares.

## Browser Storage User Behavior

The website should restore saved data when the user refreshes, closes, or opens the website again later in the same browser.

If no saved data exists, the website should show the first money amount setup.

The website should explain that saved data belongs only to the same device and browser unless online accounts are added in a future version.

Technical browser storage rules are defined in `docs/specs/technical/technical1.md`.
