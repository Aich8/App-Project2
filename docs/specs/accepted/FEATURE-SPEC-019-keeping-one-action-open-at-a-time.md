# FEATURE-SPEC-019: Keeping One Action Open At A Time

Spec ID: `FEATURE-SPEC-019`

Spec type: `FeatureSpec`

Covers: allowing only one unfinished user action to be open at a time.

Status: `Accepted`

## User Goal

The user always has one clear action to finish or leave before starting another action.

An accidental click or tap cannot close one action and immediately start a different action.

## What Counts As An Open Action

An action is open while the website is waiting for the user to choose, enter, confirm, or finish something.

Open actions include:

- The open actions for the main money amount.
- An open `Add`, `Subtract`, or `Modify` flow.
- Open `Balance Changes` delete actions or confirmation.
- Open actions for a `Saving`.
- An open `Saving` creation, rename, planned money amount change, or fix flow.
- An open `Saving` delete confirmation.
- A `Saving` reorder that is in progress.

## One Open Action Rule

- Only one action can be open at a time.
- While one action is open, an unrelated second action does not open.
- The user must first close, cancel, complete, or finish the current action.
- After the current action ends, the user can start another action with a new click, tap, or hold.

## One Interaction Cannot Start A Second Action

- A click or tap that closes the current action does not also start another action.
- If the user selects another action while one is open, that selection may close the current action under its normal rules.
- The user must select the other action again to open it.

## Continuing The Same Action

- Choosing `Add`, `Subtract`, or `Modify` replaces the open main money actions with the selected money amount flow.
- Choosing to delete a `Balance Changes` entry replaces its delete actions with the delete confirmation.
- Selecting a `Saving` name, planned money amount, or `Delete` replaces the open `Saving` actions with the selected flow or confirmation.
- In each case, the previous step closes as the next step opens.
- These steps belong to one continuing action and never leave two actions open together.

## Savings And Reordering

- The `Savings` section itself is not an open action.
- One action can be open inside `Savings`.
- If an action is open on the dashboard, `Savings` does not open until that action has ended.
- A `Saving` reorder cannot start while another `Saving` action, change flow, or delete confirmation is open.
- While a `Saving` reorder is in progress, another action does not open.

## Result

- Preventing a second action from opening does not change or save any information.
- It does not create a `Balance Changes` entry.
- It does not show a message.
- The current action follows its own rules for saving, canceling, closing, or failing.

## Acceptance Expectations

- No two unfinished actions are open at the same time.
- An unrelated second action cannot open until the current action ends.
- One click or tap cannot close the current action and also open another action.
- Moving from one step to the next step of the same action is allowed.
- `Savings` can contain one open action but is not itself an open action.
- Reordering and another open action cannot happen at the same time.
- Blocking a second action changes no information and creates no message or `Balance Changes` entry.
