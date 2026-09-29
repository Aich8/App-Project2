# FEATURE-SPEC-012: Reordering Saving Squares

Spec ID: `FEATURE-SPEC-012`

Spec type: `FeatureSpec`

Covers: changing the order of existing `Saving` squares inside `Savings`.

Status: `Draft`

## User Goal

The user can arrange `Saving` squares in the order they want.

The order determines which `Saving` is shown as covered first.

## Starting Reorder

- On a touch screen, the user presses and holds a `Saving` for `600ms`, then moves it.
- With a mouse, the user clicks and holds a `Saving` for `600ms`, then drags it.
- Reordering starts only after the hold is completed and movement reaches at least `8px` from the initial press position.
- Reaching exactly `8px` is enough to start reordering after the completed hold.
- Holding or moving a `Saving` for reordering does not open its actions.
- Reordering cannot start while a `Saving` action, change flow, or delete confirmation is open.

## Actions That Do Not Reorder

- A normal click or tap does not reorder a `Saving`.
- On a touch screen, movement reaching `8px` before the hold is completed is treated as scrolling and does not reorder a `Saving`.
- Mouse movement before the hold is completed does not start reordering.
- Holding a `Saving` for `600ms` and releasing it before movement reaches `8px` does nothing.
- These actions do not change the order or show a message.

## Moving A Saving

- The selected `Saving` moves with the user's finger or mouse pointer.
- Moving within `40px` of the top or bottom edge of the `Saving` squares area automatically scrolls the list.
- Automatic scrolling moves exactly `8px` per animation frame in the matching direction.
- Top and bottom automatic scrolling use the same trigger distance and speed.
- Automatic scrolling speed does not change based on how close the pointer is to the edge.
- The user chooses the new position by releasing the `Saving` there.
- The order does not change permanently until the `Saving` is released in its new position.

## Successful Reorder

- The selected `Saving` takes its new place among the other `Saving` squares.
- Each `Saving` keeps its name and planned money amount.
- The complete saved `Saving` list is checked again after the new order is successfully kept.
- An unchanged record that is now valid is shown as a normal `Saving`.
- An unchanged record that is still invalid remains a broken `Saving`.
- Coverage is recalculated using the resulting normal `Saving` order.
- The `Saving` at the top is covered first.
- The main money amount remains unchanged.
- When the set of normal `Saving` squares is unchanged, `Savings money amount` and the total amount still needed remain unchanged.
- If an unchanged broken record becomes normal, the information inside `Savings` updates using the resulting normal `Saving` squares.
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
- The message remains visible without a timer until a new reorder or another action starts, the user leaves `Savings`, newer saved information arrives, or the website is refreshed, closed, or reopened.
- A new reorder or another action removes the previous message when that action successfully starts.
- Another failed reorder shows the message again.

## Acceptance Expectations

- A completed `600ms` hold followed by movement of at least `8px` lets the user reorder one `Saving`.
- A normal click, tap, pre-hold touch scroll, or hold without enough movement does not reorder a `Saving`.
- Automatic scrolling uses the same `40px` edge distance and fixed `8px` per-frame speed at the top and bottom.
- Releasing the moved `Saving` saves its new position.
- Dropping it in its original position finishes without changing anything.
- An interrupted or failed reorder keeps the previous order.
- A successful reorder changes coverage priority without changing the main money amount.
- A successful reorder checks every saved `Saving` again and updates `Savings` only if another record becomes normal.
- A save-failure message remains without a timer until another action starts or the user leaves the current `Savings` state, and another failed reorder shows it again.
- Reordering never creates a `Balance Changes` entry.
