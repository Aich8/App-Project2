# FEATURE-SPEC-012: Reordering Saving Squares

Spec ID: `FEATURE-SPEC-012`

Spec type: `FeatureSpec`

Covers: changing the order of existing `Saving` squares inside `Savings`.

Status: `Accepted`

## User Goal

The user can arrange `Saving` squares in the order they want.

The order determines which `Saving` is shown as covered first.

## Starting Reorder

- On a touch screen, the user presses and holds a `Saving` for `600ms`, then moves it.
- With a mouse, the user clicks and holds a `Saving` for `600ms`, then drags it.
- Reordering starts only after the hold is completed and the `Saving` is moved.
- Holding or moving a `Saving` for reordering does not open its actions.
- Reordering cannot start while a `Saving` action, change flow, or delete confirmation is open.

## Actions That Do Not Reorder

- A normal click or tap does not reorder a `Saving`.
- Moving a finger to scroll before the hold is completed does not reorder a `Saving`.
- Holding a `Saving` for `600ms` and releasing it without moving it does nothing.
- These actions do not change the order or show a message.

## Moving A Saving

- The selected `Saving` moves with the user's finger or mouse pointer.
- Moving near the top or bottom of the `Saving` squares automatically scrolls the list.
- The user chooses the new position by releasing the `Saving` there.
- The order does not change permanently until the `Saving` is released in its new position.

## Successful Reorder

- The selected `Saving` takes its new place among the other `Saving` squares.
- Each `Saving` keeps its name and planned money amount.
- Coverage is recalculated using the new order.
- The `Saving` at the top is covered first.
- The main money amount remains unchanged.
- The `Savings money amount` and the total amount still needed remain unchanged.
- No `Balance Changes` entry is created, and no success message is shown.
- The new order remains after the website is refreshed, closed, or opened later.

## Original Position

- If the user drops the `Saving` back in its original position, reordering finishes without changing the order.
- No `Balance Changes` entry is created, and no message is shown.

## Interrupted Reorder

- If reordering is interrupted before the `Saving` is released, the `Saving` returns to its original position.
- Automatic scrolling stops.
- The order remains unchanged.
- No `Balance Changes` entry is created, and no message is shown.

## Failure Result

- If the website cannot keep the new order, every `Saving` returns to the last successfully kept order.
- Names, planned money amounts, coverage, and all money amounts return to their previous state.
- The exact message `Changes could not be saved.` is shown.
- The user can try to reorder the `Saving` again.

## Acceptance Expectations

- A completed `600ms` hold followed by movement lets the user reorder one `Saving`.
- A normal click, tap, scroll, or hold without movement does not reorder a `Saving`.
- Releasing the moved `Saving` saves its new position.
- Dropping it in its original position finishes without changing anything.
- An interrupted or failed reorder keeps the previous order.
- A successful reorder changes coverage priority but does not change any money amount.
- Reordering never creates a `Balance Changes` entry.
