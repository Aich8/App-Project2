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
- An Add entry uses `+{money amount} added`.
- A Subtract entry uses `-{money amount removed} subtracted`.
- Each entry shows the date and time automatically recorded in the user's local time when the action succeeds.
- The user cannot edit any entry information.
- Entries do not contain notes.
- Entries do not show the previous or resulting main money amount.

## Ordering And New Entries

- Entries appear newest first.
- A new successful `Add` or `Subtract` entry appears above older entries.
- After a new entry is created, the `Balance Changes` scroll position moves to the top so the newest entry is visible.

## Visible Period

- An entry becomes expired 30 days after its successful action.
- Expiration does not change, recalculate, or reverse the main money amount.

## Acceptance Expectations

- The user can distinguish every recent successful `Add` and `Subtract` by its sign, action text, money amount, date, and time.
- Entries are newest first, and a newly created entry is immediately visible.
- `Modify` and unsuccessful money actions never appear in `Balance Changes`.
- An entry stops appearing in `Balance Changes` after 30 days.
- An entry expiring does not change the money amount.
