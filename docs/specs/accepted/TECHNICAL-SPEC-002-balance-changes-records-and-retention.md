# TECHNICAL-SPEC-002: Balance Changes Records And Retention

Spec ID: `TECHNICAL-SPEC-002`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-004: Viewing Balance Changes`](FEATURE-SPEC-004-viewing-balance-changes.md)

Covers: the saved `Balance Changes` record, exact output formatting, newest-first ordering, 720-hour visibility, and newest-entry scrolling.

Status: `Accepted`

## Purpose

Turn each successfully saved `Add` or `Subtract` result into one exact `Balance Changes` record and derive the current rows shown on the dashboard.

## Stored Record

```ts
type StoredBalanceChangeV1 = {
  id: string;
  action: "add" | "subtract";
  amount: string;
  createdAtMs: number;
  createdLocalDateTime: {
    year: number;
    month: number;
    day: number;
    hour: number;
    minute: number;
  };
  visibleUntilMs: number;
};
```

- `id` is unique and created with `crypto.randomUUID()`.
- `amount` is a positive normalized decimal string such as `34.50`, without a sign, comma, or `$` sign.
- `amount` must have exactly two decimal digits and no unnecessary leading zeros. `0.01`, `5.00`, and `999999.99` are valid; `005.00`, `5.0`, and `5,000.00` are invalid.
- A Subtract record stores the money amount actually removed.
- `createdAtMs` is the exact creation timestamp used for ordering and expiration.
- `createdLocalDateTime` preserves the user's local date and time from creation.
- `visibleUntilMs` equals `createdAtMs + 2_592_000_000`.

The record does not store display text, notes, or previous and resulting main money amounts.

## Validation And Creation

The decoder accepts a record only when:

- its ID is a non-empty string and its action is exactly `add` or `subtract`;
- its amount has the normalized stored format and converts to integer cents from `1` through `99_999_999`;
- its creation timestamp is a non-negative safe integer supported by the JavaScript `Date` API;
- its local parts form a real date from year `1000` through `9999` and a real time from `00:00` through `23:59`; and
- its expiration timestamp is a safe integer exactly `2_592_000_000` milliseconds after its creation timestamp.

After validation, the amount is converted to integer cents. Decimal floating-point arithmetic is not used. An invalid record is reported to the saved-data loader, which owns list-level recovery and duplicate-ID handling.

An Add or Subtract executor creates one candidate record only for a valid result that changes the main money amount. The record is included in the same candidate save as that money amount change and becomes real only when the complete save succeeds.

A new candidate record is placed at the beginning of the saved `balanceChanges` list. If two records have the same `createdAtMs`, their saved-list order is kept, so the newest record remains first.

A failed save discards the candidate record. A retry creates a new candidate with the retry's current time. `Modify`, invalid, canceled, and no-action results create no record.

## Read Model And Output

The read model:

1. excludes records when the current time is equal to or later than `visibleUntilMs`;
2. orders the remaining records by `createdAtMs`, newest first;
3. preserves saved list order when timestamps are equal; and
4. maps every remaining record to one non-editable row.

There is no entry limit or newest-only cut-off.

The visible change text is derived from the action and the exact integer cents:

- `add` becomes `+{money amount} added`, such as `+5,895.50$ added`;
- `subtract` becomes `-{money amount} subtracted`, such as `-5,895.50$ subtracted`.

The money formatter always supplies comma separators when needed, exactly two decimal digits, and the trailing `$` sign.

The visible date and time come from `createdLocalDateTime` and always use this fixed English pattern:

```text
{English month} {day}, {four-digit year} at {hour}:{two-digit minutes} {AM or PM}
```

For example, local values for September 18, 2026 at 14:30 produce `September 18, 2026 at 2:30 PM`. The day and 12-hour clock hour have no leading zero. The formatter does not apply another time-zone conversion, so a later time-zone change does not rewrite the recorded local time.

The dashboard renders the exact label `Balance Changes`, uses record IDs as stable row identities, and keeps every unexpired row in the read model.

## Expiration And Cleanup

For this feature, 30 days is exactly 720 hours. An entry is visible only while:

```ts
currentTimeMs < visibleUntilMs
```

The dashboard schedules a check for the next expiration. Because a browser timeout cannot safely cover every long delay, the implementation clamps long delays and reschedules until the boundary is reached. It also checks again when the window gains focus or the page becomes visible.

The visible selector always filters using the current time, so an expired entry cannot remain visible while waiting for saved-data cleanup.

A pure cleanup helper removes expired records when saved information is loaded and when the next successfully saved state is prepared. Expiration and cleanup never change the main money amount or create another `Balance Changes` entry.

## Newest-Entry Scrolling

After a successful local Add or Subtract save, the executor publishes the new record ID. After that row renders, the history view moves to the top.

When another open tab or window receives a newly committed Add or Subtract entry, the cross-tab updater publishes that new record ID and the receiving history view also moves to the top.

Initial loading, expiration, deletion, `Modify`, invalid, canceled, no-action, and failed actions do not trigger newest-entry scrolling.

## Essential Verification

Verify that:

- successful Add and Subtract results create one matching record, while other results create none;
- stored amounts round-trip through integer cents without decimal rounding;
- stored amounts with unnecessary leading zeros or anything other than exactly two decimal digits are rejected;
- invalid record fields are rejected for list-level recovery;
- entries appear newest first, and a newly created record remains first when timestamps are equal;
- output includes the exact signed money text and fixed English local date and time;
- entries disappear exactly at their 720-hour boundary, including after a background pause;
- expiration and cleanup never change the main money amount;
- local and cross-tab new entries move the receiving history view to the top; and
- initial loading and non-creation updates do not trigger that scrolling.

## Boundaries

Deletion behavior, the complete saved-data envelope, list-level recovery, and the atomic browser-storage write mechanism belong to their matching technical work.
