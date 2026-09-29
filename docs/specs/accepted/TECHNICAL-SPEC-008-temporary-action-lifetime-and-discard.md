# TECHNICAL-SPEC-008: Temporary Action Lifetime And Discard

Spec ID: `TECHNICAL-SPEC-008`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-017: Discarding Unfinished Actions After Refresh Or Reopen`](FEATURE-SPEC-017-discarding-unfinished-actions-after-refresh-or-reopen.md)

Covers: keeping unfinished actions only in runtime memory and discarding them safely when the website document ends or starts again.

Status: `Accepted`

## Purpose

An unfinished action must disappear when its current website session ends without changing successfully saved information.

## Runtime State Boundary

Keep loaded information and unfinished action state separate:

- TechnicalSpec 007 owns loaded saved information and recovery state.
- Action-specific technical specs own their temporary input, confirmation, action-menu, and reorder state.
- TechnicalSpec 009 owns the temporary identity of a pending coordinated save request.
- TechnicalSpec 010 owns which one action may be open.

Temporary action state includes:

- open main money actions;
- an `Add`, `Subtract`, or `Modify` entry state;
- `Balance Changes` delete actions or confirmation;
- open actions for one `Saving`;
- `Saving` creation, rename, planned-money-amount change, or fix state;
- a `Saving` delete confirmation; and
- an unfinished `Saving` reorder, including its dragged item, placeholder, pointer information, and auto-scroll state.

Entered values, selected targets, confirmation flags, `confirming`, save-failure visibility, drag data, and pending request identities are all temporary.

## Persistence Exclusion

Every save candidate is built from the canonical loaded document plus only the valid saved change requested by the current action. A candidate is never created by serializing the complete runtime state.

Temporary action information must not be written to:

- `StoredWebsiteDataV1` or another browser-storage key;
- `sessionStorage`, IndexedDB, cookies, or another persistent browser store;
- a URL, query string, or route value that can recreate the action; or
- browser history state in a form that can restore entered values, a pending confirmation, or a reorder.

The exact-field encoder from TechnicalSpec 007 rejects temporary fields that reach a candidate by mistake.

Temporary input controls take their values only from the current runtime action state. They must not copy values that the browser kept from an earlier unfinished action. If the browser restores an old input value or preserves old page controls, the website replaces that value with the fresh runtime state before the action can be used.

## Fresh Website Start

Every full document start begins with no unfinished action. The website loads only the stable saved-data key through TechnicalSpec 007 and does not look for an action draft.

After loading finishes:

- a ready result starts with all actions closed and no unsaved entered information;
- a recovery result starts with a fresh recovery state and no previous save-failure message; and
- only the latest successfully saved information can return; unfinished information cannot return.

Stale browser history entries may control normal navigation, but they must not reconstruct an unfinished action. TechnicalSpec 011 owns the normal first dashboard view.

## Shared Discard Operation

The pending-save scope depends on why the discard happens:

- `pagehide` or a back-forward-cache discard marks every pending save request owned by the current document as no longer current; and
- newer stored information marks every pending save request belonging to the current ready, loading, or recovery state as no longer current, including a `Start again` request with an unknown stored-value baseline.

Use one shared discard operation whenever a live website document must discard an unfinished action. It runs these steps in order:

1. mark the temporary action as being discarded;
2. mark every pending save request in the required scope above as no longer current;
3. cancel every timer and scheduled animation frame owned by the action, including hold timers and reorder auto-scroll;
4. remove every temporary action-only event listener;
5. release any pointer capture;
6. clear entered values, selected targets, confirmations, `confirming`, save-failure visibility, drag data, and every other temporary action value;
7. close the unfinished action; and
8. when the document remains usable, render again from its canonical loaded information.

The discard mark exists only in runtime memory. Every timer callback, scheduled callback, pointer handler, drag handler, and other action-owned callback must first confirm that its action is still current and is not being discarded. A callback that is stale or belongs to an action being discarded stops silently before changing state, requesting a save, completing a deletion, or finishing a reorder.

The operation performs no browser-storage write, action success effect, or `Balance Changes` creation and shows no message. Calling it more than once has the same result as calling it once.

## Refresh, Close, And Later Reopen

When the current document receives `pagehide`, run the shared discard operation without writing. Do not call an action's success handler, delete a selected item, finish a reorder, or create a `Balance Changes` record.

Correctness must not depend on a close event running, because a browser or device may end the document without delivering one. Temporary state exists only in memory, so losing the document automatically loses that state.

If a document returns from the browser back-forward cache, `pageshow` with `persisted === true` must not reuse its old runtime state. Run the shared discard operation first. TechnicalSpec 009 then owns exactly one reconciliation read of the stable storage key and compares it with the active known baseline. TechnicalSpec 008 must not start another storage read or a separate TechnicalSpec 007 load. If the value differs, TechnicalSpec 009 processes that value through TechnicalSpec 007. If it is unchanged, the canonical loaded information remains active with every unfinished action closed.

Do not use ordinary focus loss or a hidden-page event as proof that the website was closed. Merely switching tabs must not trigger a save or a leave warning.

## Pending Save At Session End

Before clearing temporary state on `pagehide`, mark every pending save request owned by the current document as no longer current, whether it is waiting for the TechnicalSpec 009 exclusive save turn or has already started preparing the save. TechnicalSpec 009 aborts a request that is still waiting and checks the request again before calling the writer. A request invalidated before the writer call cannot write or apply a late result.

If the TechnicalSpec 006 writer already completed its successful `setItem` call before invalidation, the commit remains saved and must not be reversed. Any result delivered after the document or action was discarded is ignored and cannot run an action success effect in that discarded runtime.

Do not start, retry, or flush a save from `pagehide`, `beforeunload`, or another unload path.

Do not call `preventDefault`, set `returnValue`, or register another `beforeunload` behavior solely because an unfinished action exists. The unfinished action must not cause a browser leave warning.

If the TechnicalSpec 006 commit boundary completed before the session ended, that change is already successfully saved and returns on the next load. If it did not complete, the candidate and its temporary input do not return.

## Restoring The Saved View

Discarding an unfinished action renders from the latest canonical loaded information:

- typed money amounts and `Saving` inputs are absent;
- pending deletions remain unperformed;
- an unfinished reorder uses the last successfully saved order;
- open action states and confirmations are absent; and
- a previous temporary save-failure message is absent.

The discard itself performs no storage write, creates no `Balance Changes` record, and shows no message.

## Cross-Tab Integration

When TechnicalSpec 009 accepts newer saved information from another tab or window, it uses the same discard operation before applying the update. This closes the unfinished action, invalidates every pending request belonging to the replaced ready, loading, or recovery state, including an unknown-baseline `Start again` request, and prevents an older candidate from replacing the newer information.

## Essential Verification

Verify that:

- no temporary action field can appear in an encoded version-1 document;
- refresh and a later full reopen start without every listed unfinished action;
- entered money amounts, names, planned money amounts, selected delete targets, failure messages, and reorder state are not restored;
- a pending deletion is not completed and an unfinished reorder returns to the saved order;
- discarding temporary state performs no write, creates no `Balance Changes` entry, and shows no message;
- `pagehide` invalidates every pending save request owned by that document, and newer stored information invalidates every pending save request belonging to the replaced ready, loading, or recovery state, including an unknown-baseline `Start again` request;
- a save request invalidated before its writer call never writes or applies a late result, whether it was waiting or already preparing the save;
- a commit completed before discard remains available after loading again;
- browser-restored input values cannot recreate or refill an unfinished action;
- discarding an action cancels its hold timers, scheduled animation frames, reorder auto-scroll, pointer capture, and temporary event listeners before any of them can continue the old action;
- a queued callback or final pointer event from an action being discarded stops silently and cannot reopen the action, complete a deletion or reorder, request a save, or show a message;
- calling the shared discard operation more than once causes no extra effect;
- a back-forward-cache restoration runs the shared discard operation and delegates exactly one reconciliation read to TechnicalSpec 009 without starting a second read or separate load;
- merely hiding or unfocusing the page does not write or show a leave warning;
- refreshing or closing with any listed unfinished action does not produce a website-created leave warning, call `preventDefault`, or set `returnValue` because of that action, and the next start does not restore the action; and
- a received cross-tab update uses the same discard path.

## Boundaries

This spec does not define action input rules, action calculations, the one-open-action priority, browser-storage encoding, save coordination, cross-tab delivery, or normal dashboard and `Savings` navigation.
