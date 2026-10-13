# TECHNICAL-SPEC-011: Dashboard Loading And Rendering

Spec ID: `TECHNICAL-SPEC-011`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-022: Viewing The Dashboard`](FEATURE-SPEC-022-viewing-the-dashboard.md)

Covers: selecting the initial website view and deriving one complete dashboard from the current loaded information.

Status: `Accepted`

## Purpose

Show a neutral first render while browser storage is checked, then render the dashboard only from one complete ready state.

## Root Render Selection

The browser-side root selector reads one coherent controller snapshot containing the `WebsiteLoadState` from TechnicalSpec 007, the `PrimaryView` from TechnicalSpec 017, and TechnicalSpec 009's `update-in-progress` value. Apply this render priority:

1. `loading` renders one neutral, non-actionable loading root. It renders no dashboard values, dashboard actions, or recovery controls.
2. `recovery` renders the TechnicalSpec 007 recovery view instead of the dashboard or `Savings`.
3. `ready` renders the dashboard or `Savings` according to `PrimaryView`.

Ignore `PrimaryView` while the load state is `loading` or `recovery`. Never combine values taken from different controller snapshots.

The server-rendered or pre-rendered output and the first client render both use the neutral loading root. They must not initialize a temporary ready state with `0.00$`. TechnicalSpec 009 installs its storage listener before TechnicalSpec 007 performs the first storage read.

A fresh website document selects the dashboard when its first ready state becomes active. Successful `Start again` installs the resulting ready state and selects the dashboard in the same controller operation, so no intermediate `Savings` view can render. Returning from `Savings` selects the dashboard using the latest active ready state, not a copy retained from when `Savings` opened.

During TechnicalSpec 009 reconciliation, an existing complete stable view may remain visible while the `update-in-progress` value from that same controller snapshot blocks its actions. No staged or partly loaded information is rendered. The replacement appears only after one complete newer ready or recovery state becomes active.

## Dashboard View Model

Derive the dashboard view model from one TechnicalSpec 007 ready state and one captured current time:

```ts
type DashboardBalanceChangeRow = {
  id: string;
  changeText: string;
  localDateTimeText: string;
};

type DashboardViewModel = {
  mainMoneyLabel: "Current Balance";
  mainMoneyAmountText: string;
  balanceChangesLabel: "Balance Changes";
  balanceChangeRows: readonly DashboardBalanceChangeRow[];
  savingsActionText: "Savings";
};
```

Use `ready.mainMoneyAmountCents` for `mainMoneyAmountText`. Use the TechnicalSpec 002 read model for `balanceChangeRows`, including its expiration filter, newest-first ordering, exact row text, and fixed English local date-and-time text.

The selector returns every currently visible row without a count limit. When the list is empty, it returns an empty array. The `Savings` action is independent of the number of rows.

Do not keep separate component copies of the main money amount or rows. The ready state and the view model derived from it remain the single source for one dashboard render.

## Main Money Amount Formatter

Use one shared money-formatting helper wherever this exact non-negative money text is needed. For the dashboard, the helper accepts validated integer cents from `0` through `99_999_999`.

1. Derive the whole-money part with `Math.floor(cents / 100)`.
2. Derive the cents part with `cents % 100` and pad it to two digits.
3. Add comma separators to the whole-money digits in groups of three.
4. Join the parts as `{whole}.{cents}$`.

The input is a validated safe integer, so these operations produce the exact whole-money and remaining-cents parts throughout the accepted range. The result is deterministic and does not depend on browser locale. Examples are `0.00$`, `14.50$`, `5,895.50$`, and `999,999.99$`. The formatter never adds a sign to the main money amount.

Only a validated ready state reaches this formatter. Invalid or unreadable stored information must use loading or recovery rather than being replaced with a false `0.00$` dashboard.

## Rendering The Dashboard

Render the dashboard from one `DashboardViewModel`:

- associate `Current Balance` with the complete formatted main money amount;
- show the `Balance Changes` section and render every row supplied by the view model;
- render no empty-state content when `balanceChangeRows` is empty; and
- always render the `Savings` action while the dashboard is active.

The main money amount and `Savings` action use normal semantic buttons. The history uses stable record IDs as row keys. Required text and the complete valid money amount must remain available without truncation or horizontal page overflow on supported phone-sized and desktop screens.

No layout or overflow rule may make a currently visible `Balance Changes` row or the `Savings` action unreachable. Normal page scrolling and a separately scrolling history area are both permitted; the visible design chooses between them.

Selecting the main money amount delegates to TechnicalSpec 012. Selecting `Savings` delegates to TechnicalSpec 017. Both requests pass through TechnicalSpec 010. Balance-change deletion delegates to TechnicalSpec 016.

Rendering the dashboard performs no browser-storage write and creates no `Balance Changes` record or message. In particular, rendering the ready state created from a missing storage key does not save the empty document.

## Re-rendering Current Information

After a successful local save or an accepted newer same-browser update, derive a new complete view model from the replacement ready state and publish it in one render. Never combine the main money amount from one ready state with history from another.

Canceled, invalid, no-action, unfinished, and failed changes do not replace the ready state, so they do not change the base dashboard values. Temporary action views may use their own in-memory state without copying it into the dashboard view model.

While the dashboard is active, schedule the TechnicalSpec 002 check for the next expiration boundary. Bind that scheduled check to the current ready state and dashboard activation. Cancel it when that ready state is replaced or the dashboard stops being active.

Before a scheduled, focus, or visibility check changes the rows, confirm that the dashboard is still active and the same ready state is still current. A stale check stops without changing anything. A current check captures a new current time, derives the rows again, and schedules the next expiration check. Returning to the dashboard performs the same current-time derivation from the latest ready state. Removing an expired row does not change the main money amount or temporary action state.

A dashboard activation is a transition from a non-dashboard state or view into the dashboard, including the first ready state, successful `Start again`, or returning from `Savings`. Every dashboard activation starts with the main money actions closed and no unfinished action restored. TechnicalSpecs 008 and 010 own that temporary-action state.

Rebuilding the dashboard view model while the dashboard is already active is a re-render, not another activation. Expiration checks and ready-state replacements do not clear an action merely because they cause a re-render; only the matching action rules or TechnicalSpecs 008 through 010 may clear it.

## Essential Verification

Verify that:

- the server output and first client render contain neither dashboard nor recovery content;
- loading and recovery override `PrimaryView`, while a ready state uses it, without combining different controller snapshots;
- the dashboard cannot appear before TechnicalSpec 007 produces one complete ready state;
- a missing storage key produces `0.00$` without a write, history entry, or message;
- valid saved information does not briefly show the default dashboard first;
- unreadable whole data shows recovery instead of any partial or default dashboard;
- one view model derives its money amount and history from the same ready state;
- money formatting is exact at zero, cent values, comma boundaries, and `999,999.99$`;
- history contains all unexpired rows in TechnicalSpec 002 order and no empty-state content when empty;
- every current history row and `Savings` remain reachable with an empty or long history;
- local and received ready-state replacements update the complete dashboard together;
- reconciliation never exposes a staged or mixed dashboard;
- returning from `Savings` uses the latest ready state;
- replacing the ready state or leaving the dashboard cancels its expiration check, and a stale scheduled, focus, or visibility check changes nothing;
- an ordinary dashboard re-render does not clear temporary action state by itself; and
- rendering and expiration updates never write storage or change the main money amount.

## Boundaries

TechnicalSpec 007 owns loading and recovery data, TechnicalSpec 009 owns reconciliation, and TechnicalSpec 002 owns `Balance Changes` row content and expiration. Later matching technical specs own dashboard actions, `Savings` navigation, deletion, and visible design.
