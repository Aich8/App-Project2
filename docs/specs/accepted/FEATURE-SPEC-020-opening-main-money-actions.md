# FEATURE-SPEC-020: Opening Main Money Actions

Spec ID: `FEATURE-SPEC-020`

Spec type: `FeatureSpec`

Covers: opening the available actions for the main money amount and closing them without choosing an action.

Status: `Accepted`

## User Goal

The user can see which actions are currently available for the main money amount and choose the action they need.

## Opening The Actions

- The main money actions are closed when the dashboard first opens.
- Selecting the main money amount opens its available actions.
- The actions use the exact names `Add`, `Subtract`, and `Modify`.
- Opening the actions does not change or save any information.
- Opening the actions creates no `Balance Changes` entry and shows no message.

## Available Actions

- When the main money amount is `0.00$`, `Add` and `Modify` are available. `Subtract` is not available.
- When the main money amount is greater than `0.00$` and less than `999,999.99$`, `Add`, `Subtract`, and `Modify` are available.
- When the main money amount is `999,999.99$`, `Subtract` and `Modify` are available. `Add` is not available.
- Only the actions available for the current main money amount are shown.

## Choosing An Action

- Choosing `Add` starts the Add flow defined by `FEATURE-SPEC-001: Adding Money`.
- Choosing `Subtract` starts the Subtract flow defined by `FEATURE-SPEC-002: Subtracting Money`.
- Choosing `Modify` starts the Modify flow defined by `FEATURE-SPEC-003: Modifying The Money Amount`.
- The open main money actions close when the selected flow starts.

## Closing Without Choosing

- Selecting the main money amount again closes its actions.
- Clicking or tapping outside the main money amount and its actions closes them.
- Scrolling the dashboard closes the actions.
- Closing the actions without choosing one does not change or save any information.
- It creates no `Balance Changes` entry and shows no message.
- If the closing click or tap selects something else, that same click or tap does not also start the other action.

## Acceptance Expectations

- Selecting the main money amount shows only the actions available for its current value.
- The available actions change correctly at `0.00$`, between the two limits, and at `999,999.99$`.
- Choosing an available action starts its matching flow and closes the main money actions.
- Selecting the main money amount again, clicking or tapping outside, or scrolling closes the actions without changing information.
- Opening or closing the actions creates no saved change, message, or `Balance Changes` entry.
- One click or tap cannot close the main money actions and also start another action.
