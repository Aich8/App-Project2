# FEATURE-SPEC-004: Viewing Balance Changes

Spec ID: `FEATURE-SPEC-004`

Spec type: `FeatureSpec`

Covers: reviewing the recent successful `Add` and `Subtract` actions that changed the main money amount.

Status: `Accepted`

## User Goal

`Balance Changes` lists each successful `Add` and `Subtract` action recorded during the last 30 days.

## History Access

- The history uses the exact user-facing label `Balance Changes`.
- Every entry that is still inside its visible period remains reachable, even when the history is long.

## Included Changes

- Every successful `Add` creates one separate positive entry.
- Every successful `Subtract` creates one separate negative entry using the money amount removed.
- Successful `Add` and `Subtract` actions are never combined into one entry.
- A successful `Modify` creates no entry.
- Invalid, canceled, no-action, or failed money actions create no entry.

## Entry Information

- Each entry shows its signed money amount and action text.
- Every shown money amount uses comma separators when needed, exactly two digits after the decimal point, and the `$` sign after the amount.
- An Add entry uses the exact format `+{money amount} added`, such as `+5,895.50$ added`.
- A Subtract entry uses the exact format `-{money amount removed} subtracted`, such as `-5,895.50$ subtracted`.
- Each entry shows the local date and time automatically recorded when the action succeeds.
- Every date and time uses the same English format: full month name, day without a leading zero, four-digit year, `at`, hour without a leading zero, two-digit minutes, followed by `AM` or `PM`.
- For example, the date and time is shown as `September 18, 2026 at 2:30 PM`.
- The user cannot edit any entry information.
- Entries do not contain notes.
- Entries do not show the previous or resulting main money amount.

## Ordering And New Entries

- Entries appear newest first.
- A new successful `Add` or `Subtract` entry appears above older entries.
- After a new entry is created, the `Balance Changes` scroll position moves to the top so the newest entry is visible.
- When another open tab or window receives a new saved `Add` or `Subtract` entry, its `Balance Changes` scroll position also moves to the top so the newest entry is visible.

## Visible Period

- An entry becomes expired 30 days after its successful action.
- For this rule, 30 days means exactly 720 hours.
- Expiration does not change, recalculate, or reverse the main money amount.

## Acceptance Expectations

- The user can distinguish every recent successful `Add` and `Subtract` by its sign, action text, money amount, date, and time.
- An Add of `5,895.50$` is shown as `+5,895.50$ added`.
- A Subtract that removes `5,895.50$` is shown as `-5,895.50$ subtracted`.
- Every date and time uses the fixed English format, such as `September 18, 2026 at 2:30 PM`, while showing the user's local date and time from when the action succeeded.
- Entries are newest first, and a newly created entry is immediately visible.
- A new saved entry received from another open tab or window also becomes immediately visible at the top.
- `Modify` and unsuccessful money actions never appear in `Balance Changes`.
- An entry stops appearing in `Balance Changes` exactly 720 hours after its successful action.
- An entry expiring does not change the money amount.
