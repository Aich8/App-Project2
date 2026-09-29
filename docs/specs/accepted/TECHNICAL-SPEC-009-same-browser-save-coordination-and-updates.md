# TECHNICAL-SPEC-009: Same-Browser Save Coordination And Updates

Spec ID: `TECHNICAL-SPEC-009`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-018: Updating Other Open Tabs And Windows`](FEATURE-SPEC-018-updating-other-open-tabs-and-windows.md)

Covers: preventing stale writes and applying the latest successful save across open tabs and windows of the same website in the same browser.

Status: `Accepted`

## Purpose

Prevent one open copy of the website from overwriting newer saved information and make every open copy adopt the latest successful save as one complete state.

## Coordinator State

Reuse the stable browser-storage key, `StoredValueBaseline`, and `CoordinatedSaveResult` from TechnicalSpec 007. Use the exclusive Web Lock name `cash-money-organizer-website-data-write`.

Each tab or window keeps only these coordination values in memory:

- one registered request for each unique request `symbol`;
- the request kind: `user-action`, `start-again`, or `automatic-cleanup`;
- whether that request is still current;
- its `AbortController` and one result handler; and
- one reconciliation generation number for loads and received updates.

Submitting the same request identity again returns its existing pending handle and keeps its original kind and result handler. It does not create another lock request, writer call, or result delivery. Remove the registration after the request saves, fails, is superseded, or is silently invalidated.

## One Safe Save Turn

Every save uses `navigator.locks.request` in exclusive mode before calling the TechnicalSpec 006 writer. Production therefore uses HTTPS; trusted `localhost` may be used during development.

If Web Locks is unavailable, or acquiring the lock fails while the request is current, return `save-failed`. Never continue with an uncoordinated write or create a browser-storage lock-key fallback.

Pass the request's abort signal to the lock request. Intentionally canceling a no-longer-current request while it waits ends silently. It performs no write, delivers no result, and runs no success or failure effect.

For a request with a known baseline, perform these operations inside one exclusive lock callback:

1. stop silently if the request is no longer current;
2. read the stable storage key inside a failure boundary;
3. return `save-failed` if that read fails;
4. compare the exact returned string or `null` with the request's baseline;
5. if they differ, return `superseded` with the exact current value and do not call the writer;
6. check once more that the request is current;
7. call the TechnicalSpec 006 writer exactly once when it is current and the baseline still matches; and
8. return the writer's `saved` or `save-failed` result.

Do not await other work between the read, comparison, and synchronous writer call inside the lock. Two requests based on the same old value therefore cannot both save successfully.

After the lock releases, deliver a result only while its request is still current. TechnicalSpec 008 decides which requests become invalid when an action, document, or loaded state is discarded. Abort a request that is still waiting and check again before writing. A write already completed before invalidation remains saved, but its late result cannot affect discarded runtime state.

## Unknown-Baseline Start Again

An unknown baseline is allowed only for TechnicalSpec 007 `Start again` recovery. While holding the exclusive lock:

1. stop silently if the request is no longer current;
2. read the current storage value;
3. return `save-failed` if the read fails;
4. write the complete empty recovery candidate when the key is missing;
5. validate an existing string through the TechnicalSpec 007 whole-document rules;
6. return `superseded` with that string when it now contains usable information; or
7. replace it once with the empty recovery candidate when it still requires whole-data recovery.

Newly usable information is loaded instead of overwritten. A superseded `Start again` is not automatically retried.

## Applying Save Results

The tab or window that writes does not wait for its own storage event.

For a successful `user-action`, build one ready snapshot from the successful candidate and returned serialized baseline. Derive its main money amount, `Balance Changes`, `Saving` slots, cleanup state, and coverage from that candidate. Replace them together, then close the action and run only its accepted success effects.

Successful and failed `Start again` and automatic-cleanup requests use the TechnicalSpec 007 rules. A cleanup result is applied only while its load generation remains current. A failed normal action keeps its last successful information and uses that action's accepted failure behavior.

For any still-current `superseded` result:

1. show no save-failure message and run no success effect;
2. increase the reconciliation generation;
3. enter `update-in-progress` so no new action can start;
4. discard the replaced temporary state and pending requests through TechnicalSpec 008; and
5. load the returned `latestStoredValue` through TechnicalSpec 007 under that generation without reading storage again.

Only that generation or a later one may become active.

## Receiving And Reconciling Updates

Install one `storage` listener before TechnicalSpec 007 begins its first storage read. Process only events for `localStorage` and the stable website key. Treat the event only as a signal; do not load `event.newValue`, because a later save may already have replaced it.

Use this same reconciliation operation for a matching event and for the resume checks below:

1. increase the reconciliation generation, making every older staged load and cleanup result stale;
2. read the stable key exactly once inside a failure boundary;
3. if the read fails, leave any reconciliation `update-in-progress` state, keep the current stable information and any action that is still open, show no message, and do not reopen an action that was already discarded;
4. if the value exactly matches the active known baseline, leave any reconciliation `update-in-progress` state and keep the active information and any unfinished action that was not already discarded;
5. if it differs, enter `update-in-progress`, invalidate pending requests, and discard the unfinished action through TechnicalSpec 008;
6. load that exact returned value through TechnicalSpec 007 under the current generation, including its one permitted cleanup attempt; and
7. only if the generation remains current, replace the complete active state and leave `update-in-progress`.

The previous stable information may remain visible during `update-in-progress`, but no action may open or register a save. Actions become available after the latest complete ready or recovery result, or after a failed current-value read retains the previous state.

A later reconciliation trigger invalidates an older staged generation before comparing its newly read value. Receiving an update creates no message, history entry, or echo write. TechnicalSpec 007's one required silent cleanup attempt is the only permitted follow-up write. A canceled, invalid, failed, or no-action result produces no cross-tab update because it completes no successful storage write.

## Resuming A Page

After a back-forward-cache return, TechnicalSpec 008 first discards temporary runtime state. Then run exactly one reconciliation read. Even when the stored value is unchanged, the already discarded action stays closed.

When an ordinarily hidden page becomes visible again, reconcile once without first discarding its action. An unchanged value keeps that action open; a different value closes it through the normal update path.

## Newest Balance Change Signal

Compare the previous and replacement states only when both are ready. If the replacement contains a valid, unexpired Add or Subtract record ID absent from the previous state, publish the first such ID in new saved-list order after rendering. TechnicalSpec 002 then moves `Balance Changes` to the top.

Loading-to-ready and recovery-to-ready transitions, cleanup-only changes, deletions, `Modify`, and updates without a new Add or Subtract record publish no scrolling ID.

## Essential Verification

Verify that:

- one request identity produces one lock request, at most one writer call, and one result delivery;
- every write uses the same exclusive lock and exact baseline comparison;
- two requests using the same old baseline cannot both save;
- a current lock or storage-read failure returns `save-failed`, while intentional cancellation of an old waiting request ends silently;
- no unavailable-lock or error path performs an uncoordinated write;
- unknown-baseline `Start again` loads newly usable information instead of overwriting it;
- a successful local save installs one matching candidate, baseline, and set of derived values before running success effects;
- a superseded or invalidated request performs no old action effect;
- the storage listener exists before the first read and every event causes a fresh current-value read;
- only the latest reconciliation generation can become active;
- a different value discards unfinished work and applies one complete update without an echo write;
- actions remain blocked only while a different value is being loaded;
- back-forward-cache and ordinary visibility returns follow their different discard rules;
- received new Add and Subtract records alone publish the scrolling ID; and
- all open copies eventually show the same latest successfully saved information.

## Boundaries

TechnicalSpec 006 owns encoding and the single browser-storage write. TechnicalSpec 007 owns loading, validation, cleanup, and recovery. TechnicalSpec 008 owns temporary-state discard. TechnicalSpec 010 owns one-open-action priority. Individual action specs own their calculations and user-visible results.
