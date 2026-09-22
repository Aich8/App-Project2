# FEATURE-SPEC-021: Entering Main Money Amounts

Spec ID: `FEATURE-SPEC-021`

Spec type: `FeatureSpec`

Covers: the shared user-visible controls and interaction rules for entering a money amount in the `Add`, `Subtract`, and `Modify` flows.

Status: `Accepted`

## User Goal

The user can enter a precise money amount with the same predictable controls whether they are adding, subtracting, or modifying the main money amount.

This feature controls how the amount is entered and how the entry flow is left. The selected action still determines what the confirmed amount means.

## Starting The Entry Flow

- Choosing `Add`, `Subtract`, or `Modify` from the main money actions starts the matching money amount entry flow.
- The open main money actions close when the entry flow starts.
- The selected action is not repeated as a visible `Add`, `Subtract`, or `Modify` reminder inside the entry flow.
- The entry flow is temporary and does not change any information until a valid action is confirmed and successfully kept.

## Entry Flow Behavior

- The flow presents a money amount input.
- The input flow is the active interaction context while it is open, and other dashboard controls are inactive.
- The input starts at `0.00` without a `$` sign.
- The input is focused when it opens so the user can begin typing without another click or tap.
- On supported mobile devices, opening the input requests a keyboard suitable for entering digits.
- An action with the exact visible name `Cent` is available with the input on mobile and desktop.
- The flow shows the exact text `Save Changes` and provides the exact actions `Yes` and `Cancel`.
- The complete valid amount through `999,999.99` and every required control remain readable and usable.

## Entering Whole Money Amounts

- The user enters digits from `0` through `9`.
- The input can represent any money amount from `0.00` through `999,999.99`; the selected action decides whether the represented amount can be confirmed.
- Until the user chooses `Cent` or presses `Space`, every accepted digit is treated as part of the whole money amount rather than as cents.
- The input always displays two digits after the decimal point and adds comma separators automatically when needed.
- Typing `5` shows `5.00`.
- Typing `58` shows `58.00`.
- Typing `589` shows `589.00`.
- Typing `5895` shows `5,895.00`.
- Typing `58955` shows `58,955.00`.
- Typing `589550` shows `589,550.00`.
- `0` is accepted as a normal digit.
- Unneeded leading zeros are removed from the displayed whole money amount, so typing `0005` shows `5.00`.

## Entering Cents

- Choosing `Cent` after at least one entered digit starts cents entry.
- Pressing `Space` is the keyboard alternative to choosing `Cent` and has the same result.
- The initial `0.00` shown before the user types does not count as an entered digit.
- Choosing `Cent` or pressing `Space` before entering a digit is ignored without a message.
- Digits entered before cents entry starts are the whole money amount.
- After cents entry starts, the next one or two digits are cents.
- One entered cents digit is shown with a leading zero, and two entered cents digits are shown in their entered order.
- A third cents digit is ignored without changing the amount or showing a message.
- After cents entry has started, another `Cent` selection or `Space` press is ignored without changing the amount or showing a message.
- `Cent` and `Space` do not add a visible character or a comma separator to the input.
- Comma separators for the whole money amount are always added automatically.
- Choosing `Cent` keeps the input active and ready for continued typing.
- On supported mobile devices, the digit-entry keyboard remains available after the user chooses `Cent` when the browser permits it.
- The flow gives the user a visible indication that cents entry is active without requiring a particular shape, color, or visual treatment for that indication.
- Typing `5`, choosing `Cent`, then typing `5` shows `5.05`.
- Typing `5`, choosing `Cent`, then typing `50` shows `5.50`.
- Typing `58430`, choosing `Cent`, then typing `88` shows `58,430.88`.
- Typing `0`, choosing `Cent`, then typing `5` shows `0.05`.
- Typing `999999`, choosing `Cent`, then typing `99` shows `999,999.99`.
- Pressing `Space` in any of these examples has the same result as choosing `Cent`.

## Editing The Entered Amount

- Entry is append-only: accepted typing is added to the end of the entered sequence.
- Clicking, tapping, or focusing the input does not move the editing position into the middle of the formatted amount.
- Selecting part of the displayed amount does not allow that part to be replaced.
- Generated comma separators and the generated decimal point cannot be edited directly.
- `Backspace` or `Delete` removes only the last accepted digit or the action that started cents entry, then reformats the remaining amount.
- Deleting once from `5.50` entered as `5`, `Cent`, `5`, `0` returns the display to `5.05`.
- Deleting again returns the display to `5.00`.
- Deleting once more removes the accepted `Cent` action, ends cents entry, and removes its visible indication while the display remains `5.00`.
- Deleting all entered digits returns the input to `0.00`; the input does not become empty.

## Rejected Input

- Letters, minus signs, typed decimal points, typed comma separators, and typed `$` signs are ignored.
- A `Cent` selection or `Space` press before any entered digit or after cents entry has already started is ignored.
- A third digit entered as cents is ignored.
- A digit that would make the entered amount greater than `999,999.99` is ignored.
- Pasted content is ignored, whether it contains letters, digits, symbols, or a mixture.
- Rejected typing, a rejected `Cent` selection, and rejected pasted content leave the displayed amount unchanged and show no message.

## Confirming The Amount

- Choosing `Yes` attempts the selected action with the entered amount.
- `Add` applies the rules in `FEATURE-SPEC-001: Adding Money`.
- `Subtract` applies the rules in `FEATURE-SPEC-002: Subtracting Money`.
- `Modify` applies the rules in `FEATURE-SPEC-003: Modifying The Money Amount`.
- A successful action that changes the main money amount closes the entry flow, returns to the dashboard, shows the updated money amount, and resets the next money amount entry flow to `0.00`.
- A no-action or invalid confirmation follows the selected action's accepted rules and keeps the flow open when those rules require it.
- If a valid change cannot be kept, the flow follows `FEATURE-SPEC-016: Handling Changes That Cannot Be Saved`, including keeping the entered amount available for a retry or cancellation.
- A failed save shows the exact message `Changes could not be saved.`.
- The save-failure message remains visible while the user leaves the failed flow unchanged.
- Changing the entered amount removes the previous save-failure message.
- Choosing `Yes` again removes the previous save-failure message while the new attempt is made.
- If the new attempt also fails, the save-failure message appears again.
- A successful retry closes the flow and removes the save-failure message.
- A later new entry flow starts without the previous save-failure message.

## Canceling Or Leaving The Flow

- Choosing `Cancel` closes the entry flow and returns to the dashboard.
- Clicking or tapping outside the active entry flow acts like `Cancel`.
- An outside interaction that cancels the flow does not reopen the main money actions.
- Browser Back, a mobile browser back gesture, or a system Back action acts like `Cancel` while the flow is open.
- Canceling discards the entered amount, removes any save-failure message from that flow, changes and saves nothing, creates no `Balance Changes` entry, and shows no new message.
- One interaction that cancels the flow cannot also start another action, as defined by `FEATURE-SPEC-019: Keeping One Action Open At A Time`.
- Refreshing, closing, or reopening the website discards the unfinished flow and its save-failure message as defined by `FEATURE-SPEC-017: Discarding Unfinished Actions After Refresh Or Reopen`.
- An update from another open tab or window closes and discards the flow and its save-failure message as defined by `FEATURE-SPEC-018: Updating Other Open Tabs And Windows`.

## Boundaries

- This spec does not decide when `Add`, `Subtract`, or `Modify` is available or how the main money actions open.
- It does not redefine how any selected action changes the main money amount or `Balance Changes`.
- It does not define storage, data structures, parsing algorithms, source files, or framework choices.
- It does not require a particular shape, position, size, color, typography, or visual arrangement for the input, main money amount, or actions.
- Keyboard requirements are limited to the entry behavior described here; custom keyboard navigation, custom tab order, keyboard activation of clickable controls, and focus-return behavior are not defined.

## Acceptance Expectations

- `Add`, `Subtract`, and `Modify` use the same amount entry controls and typing rules.
- Every new entry flow starts focused at `0.00` without a `$` sign.
- `Cent` starts cents entry, and `Space` provides the same keyboard behavior.
- The next one or two digits become cents, while comma separators remain automatic.
- The input remains active after `Cent`, and the flow visibly indicates that cents entry is active.
- Editing remains append-only, and deleting removes the last accepted input before reformatting the amount.
- Rejected typing and paste leave the amount unchanged without showing a message.
- The entered amount cannot become greater than `999,999.99`.
- Choosing `Yes` applies the selected action's accepted rules.
- Successful changes close and reset the flow; failed saves keep the flow and entered amount available and show the save-failure message.
- The previous save-failure message disappears when the user changes the amount, retries, or closes the flow, and it appears again if the retry fails.
- `Cancel`, outside interaction, and Back leave all information unchanged.
