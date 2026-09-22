# TECHNICAL-SPEC-001: Main Money Amount Entry State

Spec ID: `TECHNICAL-SPEC-001`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-021: Entering Main Money Amounts`](FEATURE-SPEC-021-entering-main-money-amounts.md)

Covers: the internal representation, formatting, state transitions, and browser input handling for the shared main money amount entry flow.

Status: `Accepted`

## Purpose

Use one temporary entry model for the `Add`, `Subtract`, and `Modify` money amount flows.

The model owns input, formatting, and deletion. The selected action still owns what happens after confirmation.

## Entry State

```ts
type MainMoneyAmountEntryState = {
  action: "add" | "subtract" | "modify";
  wholeDigits: string;
  centsActive: boolean;
  centsDigits: string;
  confirming: boolean;
  saveFailureVisible: boolean;
};
```

- A new flow starts with empty digit strings, `saveFailureVisible` set to `false`, and a display of `0.00`.
- `wholeDigits` keeps accepted digits, including leading zeros, so deletion follows the real input order.
- `centsDigits` contains no more than two digits.
- The state exists only in memory and is never saved as a draft.

## Exact Money Amount

The entered value is derived as integer cents, not as a decimal floating-point value.

1. Normalize leading zeros in `wholeDigits` for display and calculation only.
2. Use `00` when no cents digit exists.
3. Left-pad one cents digit with `0`.
4. Calculate `whole amount × 100 + cents`.
5. Reject a digit if the result would exceed `99_999_999` cents.

The displayed input always uses two decimal digits, automatic comma separators, and no `$` sign. The formatted DOM text is never parsed back into state.

## Input Transitions

| Input | State change |
|---|---|
| Digit before cents entry | Append to `wholeDigits` |
| `Cent` or `Space` after at least one digit | Activate cents entry |
| First or second digit during cents entry | Append to `centsDigits` |
| `Backspace` or `Delete` | Remove the last cents digit, then the cents action, then the last whole digit |
| Invalid character, paste, repeated cents action, third cents digit, or over-limit digit | No change and no message |

Deleting all accepted input returns the display to `0.00`.

Any accepted digit, accepted cents action, or deletion that changes the entered amount also sets `saveFailureVisible` to `false`. Rejected input leaves the existing message state unchanged because it does not edit the amount.

## Browser Handling

- Render a controlled `type="text"` input with `inputMode="numeric"`.
- Focus it when the flow opens and keep accepted editing at the end.
- Convert each supported browser event into no more than one input transition.
- Prevent native insertion for handled digits, `Space`, `Backspace`, and `Delete`.
- Block paste and unexpected replacement input.
- Choosing `Cent` activates cents entry and restores input focus.
- Expose `centsActive` so the design can visibly indicate the active state.

## Flow Integration

The flow renders the exact required texts `Cent`, `Save Changes`, `Yes`, and `Cancel`. The selected action remains internal and is not repeated inside the flow.

Choosing `Yes` passes the selected action and integer cents to the matching action executor. The executor returns:

```ts
type ConfirmationOutcome = "saved" | "not-applied" | "save-failed";
```

- `saved`: close and destroy the entry state.
- `not-applied`: keep the flow and entered value unchanged with the previous failure message cleared.
- `save-failed`: keep the flow and entered value and set `saveFailureVisible` to `true` so `Changes could not be saved.` appears.

Choosing `Yes` clears any previous failure message before starting the new attempt. The `confirming` guard prevents duplicate confirmation. The entry model does not change the main money amount or write browser storage itself.

`Cancel`, outside interaction, Browser Back, refresh, reopen, or a newer cross-tab update discards the temporary state without saving. The shared action coordinator prevents the same outside interaction from opening another action.

## Save-Failure Message Lifetime

After a failed save, `Changes could not be saved.` remains visible while the user leaves the flow unchanged.

An accepted edit removes the old message. Retrying removes it while the new attempt is made; a failed retry shows it again, and a successful retry closes the flow. `Cancel`, outside interaction, Back, refresh, reopen, or a newer cross-tab update removes it by discarding the flow. A later new flow starts without the old message.

## Essential Verification

Verify that:

- all three actions use the same fresh `0.00` state;
- whole digits, leading zeros, commas, and cents produce exact integer cents;
- `Cent` and `Space` behave identically;
- deletion removes accepted input in reverse order;
- invalid, pasted, third-cents, and over-limit input changes nothing;
- focus, selection, and caret movement cannot replace middle content;
- confirmation produces the three required outcomes without duplicate execution;
- the save-failure message clears on an accepted edit, retry, or closure and reappears after another failed retry; and
- every cancel or external-close path discards unsaved input.

## Boundaries

This spec does not define action calculations, saved-data structure, browser-storage writes, or any shape, size, color, position, or layout.
