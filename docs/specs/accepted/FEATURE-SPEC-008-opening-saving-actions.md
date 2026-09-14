# FEATURE-SPEC-008: Opening Saving Actions

Spec ID: `FEATURE-SPEC-008`

Spec type: `FeatureSpec`

Covers: opening and closing the available actions for one existing `Saving` square.

Status: `Accepted`

## User Goal

The user can select an existing `Saving` to access the actions available for it.

Opening or closing these actions does not change the `Saving` or the main money amount.

## Opening Saving Actions

- Clicking or tapping an existing `Saving` opens its actions.
- Actions can be open for only one `Saving` at a time.
- Opening the actions does not change any saved information.
- Opening the actions creates no `Balance Changes` entry.
- Opening the actions shows no message.

## Available Actions

- To rename a `Saving`, the user first clicks or taps the `Saving`, then clicks or taps its name.
- Selecting the `Saving` money amount starts changing its planned money amount.
- The exact action `Delete` starts deleting that `Saving`.
- Opening these actions alone does not rename, change, or delete the `Saving`.

## Closing Saving Actions

- Clicking or tapping outside the selected `Saving` closes its actions.
- Closing the actions does not change any information.
- Closing the actions creates no `Balance Changes` entry.
- Closing the actions shows no message.
- Clicking or tapping a blank part of the selected `Saving` keeps its actions open.
- Scrolling keeps the actions open.
- Leaving `Savings` closes any open `Saving` actions.
- Refreshing, closing, or reopening the website clears any open `Saving` actions.

## Selecting Another Saving

- When one `Saving` has open actions, selecting another `Saving` closes the open actions.
- That same selection does not open the actions of the second `Saving`.
- The user must select the second `Saving` again to open its actions.

## Actions That Do Not Open

- Scrolling over a `Saving` does not open its actions.
- Holding a `Saving` for reordering does not open its actions.
- These interactions do not change any information or show a message.

## Acceptance Expectations

- Clicking or tapping an existing `Saving` opens the actions for that `Saving` only.
- The user can start renaming, changing the planned money amount, or deleting the selected `Saving`.
- Clicking or tapping outside closes the actions without changing anything.
- Scrolling and blank-space interaction do not close actions that are already open.
- Selecting another `Saving` closes the current actions without opening new actions from the same selection.
- Opening or closing `Saving` actions never changes the main money amount or creates a `Balance Changes` entry.
