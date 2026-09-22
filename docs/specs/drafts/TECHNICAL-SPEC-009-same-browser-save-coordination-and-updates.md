# TECHNICAL-SPEC-009: Same-Browser Save Coordination And Updates

Spec ID: `TECHNICAL-SPEC-009`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-018: Updating Other Open Tabs And Windows`](../accepted/FEATURE-SPEC-018-updating-other-open-tabs-and-windows.md)

Covers: coordinating writes and applying successfully saved information across open tabs and windows of the same website in the same browser.

Status: `Draft`

## Purpose

Prevent an older tab or window from overwriting newer saved information and make every open copy adopt the latest successful save without creating another user action.

## Shared Coordination State

Use these stable names:

```ts
const WEBSITE_STORAGE_KEY = "cash-money-organizer-website-data";
const WEBSITE_SAVE_LOCK_NAME = "cash-money-organizer-website-data-write";
```

Reuse `StoredValueBaseline` and `CoordinatedSaveResult` exactly as defined by TechnicalSpec 007.

Each tab or window keeps these values only in memory:

- the baseline attached to its active loaded state;
- one unique `symbol` for each coordinated save request;
- whether that request is still current; and
- a generation number for received updates.

Request symbols and generations are never serialized. The same request identity cannot be registered twice.

## Exclusive Save Turn

Use `navigator.locks.request` with `WEBSITE_SAVE_LOCK_NAME` in exclusive mode. Every action save, recovery save, and automatic cleanup save must use this coordinator before calling the TechnicalSpec 006 writer.

The read of the latest storage value, the baseline comparison, and the possible synchronous writer call happen inside one exclusive lock callback with no awaited work between them.

If the Web Locks API is unavailable, an active lock request is rejected, or obtaining the lock otherwise fails, return `save-failed` to a still-current request without calling the writer or changing its baseline.

## Known-Baseline Save Algorithm

For a request with a known baseline:

1. register one new request identity and reject another submission of that same request;
2. wait for the exclusive save turn;
3. stop silently if the request is no longer current;
4. read `WEBSITE_STORAGE_KEY` with `localStorage.getItem` inside a failure boundary;
5. return `save-failed` without calling the writer when that read fails;
6. compare the returned string or `null` with `baseline.storedValue` using exact equality;
7. when they differ, discard the candidate and return `superseded` with the exact current value as `latestStoredValue`;
8. check once more that the request is current;
9. when it is current and the values match, call the TechnicalSpec 006 writer exactly once while still holding the lock; and
10. return the writer's `saved` or `save-failed` result.

On `saved`, record the returned `serialized` value as the requesting tab or window's new known baseline before releasing the exclusive turn. The requesting action then activates its candidate and performs its accepted success-only effects.

On `save-failed`, keep the baseline and active information unchanged. On `superseded`, deliver no save-failure message; close any unfinished action and process `latestStoredValue` through the normal TechnicalSpec 007 loading rules.

## Unknown-Baseline Start Again

An unknown baseline is valid only for the TechnicalSpec 007 `Start again` recovery case.

Inside the exclusive save turn:

1. stop silently if the request is no longer current;
2. read the current storage value;
3. return `save-failed` if the read fails again;
4. if the key is missing, write the complete empty recovery candidate once;
5. if a string exists, validate it through the TechnicalSpec 007 whole-document rules;
6. return `superseded` and load it when it now produces usable information; and
7. replace it with the complete empty recovery candidate in one write when it still requires whole-data recovery.

The validation and possible writer call stay inside the same lock callback. `Start again` is not retried automatically after `superseded`.

## Request Invalidation

Canceling or closing an action, discarding a session under TechnicalSpec 008, or accepting newer stored information marks its request identity as no longer current.

Use an `AbortController` to stop a request that is still waiting for its lock. If the request already entered the lock callback, its current-state checks prevent a write whenever invalidation was observed before the writer call.

Only a still-current request receives a `CoordinatedSaveResult`. Ending an invalidated request without delivering a result is not `save-failed` and must not show a message or run success effects.

## Publishing A Local Success

The tab or window that performs `setItem` does not rely on receiving its own storage event. It uses the successful candidate and returned serialized baseline directly.

After the candidate becomes active:

- recompute every derived value from the same active document;
- close the completed action under its matching spec;
- publish a newly created Add or Subtract record ID for newest-entry scrolling; and
- do not create any extra write, message, or `Balance Changes` record.

## Receiving Storage Events

Install one `window` `storage` listener for the website document. Process an event only when it belongs to `localStorage` and its key is `WEBSITE_STORAGE_KEY`.

The event's `newValue` is the received known stored value and may be a string or `null`. Ignore it when it exactly matches the active known baseline.

For a different received value:

1. increase the received-update generation;
2. invalidate pending save requests and discard any unfinished action through TechnicalSpec 008;
3. stage a load of `newValue` through TechnicalSpec 007 without partially replacing active information;
4. discard the staged result if another storage event has already produced a newer generation; and
5. otherwise replace the active load state and baseline together after loading and any required single cleanup attempt finish.

A usable document produces one complete ready-state replacement. An unreadable whole document produces recovery. An update never mixes the old main money amount, history, or `Savings` with parts of the new document.

Receiving an update shows no message and creates no `Balance Changes` record. It performs no echo write merely because the value was received. The one silent cleanup attempt required by TechnicalSpec 007 is the only permitted follow-up write, and it also uses the coordinator.

## Several Updates And Resuming A Page

A later storage event invalidates an older staged load or cleanup request. Only the latest generation may become active.

When a back-forward-cached document returns, or when a previously hidden document becomes visible again, read the stable storage key once and compare it with the active known baseline. If it differs, process it exactly like a received storage event. If the reconciliation read fails, keep the current active information, show no message, and try again only after another storage event or later resume check.

An unchanged reconciliation value does not close an unfinished action.

## Newest Balance Change Scrolling

When an accepted received update contains a valid, unexpired Add or Subtract record ID that was absent from the previous active ready state, publish the first such ID in the new saved-list order after the new rows render. The TechnicalSpec 002 history view then moves to the top.

Initial loading, cleanup-only changes, deletions, `Modify`, and received updates without a new Add or Subtract record publish no scrolling ID.

## Essential Verification

Verify that:

- every write path uses the same exclusive lock and exact baseline comparison;
- two requests based on the same old value cannot both write successfully;
- the second concurrent request receives the newer value as `superseded`, makes no write, and shows no save-failure message;
- lock or latest-value-read failure returns `save-failed` without reaching the writer;
- an invalidated request waiting for the lock makes no write and delivers no late result;
- an unknown-baseline `Start again` loads newly usable information instead of overwriting it;
- the writing tab activates its successful candidate without waiting for its own storage event;
- another tab applies one complete update, silently cancels its unfinished action, and creates no echo write, message, or extra history entry;
- canceled, invalid, no-action, and failed changes produce no cross-tab update because they perform no successful `setItem`;
- several rapid events allow only the latest generation to become active;
- a resumed page reconciles a missed different value while an unchanged value leaves its action alone;
- a received new Add or Subtract record moves history to the top, while non-creation updates do not; and
- all open copies eventually show the same latest successfully saved information.

## Boundaries

TechnicalSpec 006 owns candidate encoding, the single browser-storage write, and failure rollback. TechnicalSpec 007 owns loading, validation, cleanup, and recovery results. TechnicalSpec 008 owns lifecycle discard of temporary actions. TechnicalSpec 010 owns one-open-action priority. Individual action specs own calculations and success effects.
