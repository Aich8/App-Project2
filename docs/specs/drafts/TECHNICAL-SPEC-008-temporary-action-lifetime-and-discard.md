# TECHNICAL-SPEC-008: Temporary Action Lifetime And Discard

Spec ID: `TECHNICAL-SPEC-008`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-017: Discarding Unfinished Actions After Refresh Or Reopen`](../accepted/FEATURE-SPEC-017-discarding-unfinished-actions-after-refresh-or-reopen.md)

Covers: keeping unfinished actions only in runtime memory and discarding them safely when the website document ends or starts again.

Status: `Draft`

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

## Fresh Website Start

Every full document start begins with no unfinished action. The website loads only the stable saved-data key through TechnicalSpec 007 and does not look for an action draft.

After loading finishes:

- a ready result starts with all actions closed and no unsaved entered information;
- a recovery result starts with a fresh recovery state and no previous save-failure message; and
- only information from a completed successful save can return.

Stale browser history entries may control normal navigation, but they must not reconstruct an unfinished action. TechnicalSpec 011 owns the normal first dashboard view.

## Refresh, Close, And Later Reopen

When the current document receives `pagehide`, invalidate its unfinished action and any pending save-request identity without writing. Do not call an action's success handler, delete a selected item, finish a reorder, or create a `Balance Changes` record.

Correctness must not depend on a close event running, because a browser or device may end the document without delivering one. Temporary state exists only in memory, so losing the document automatically loses that state.

If a document returns from the browser back-forward cache, `pageshow` with `persisted === true` must not reuse its old runtime state. Discard that state and run the normal saved-data load again before making the website usable.

Do not use ordinary focus loss or a hidden-page event as proof that the website was closed. Merely switching tabs must not trigger a save or a leave warning.

## Pending Save At Session End

Before destroying temporary state, invalidate any save request that is still waiting for the TechnicalSpec 009 exclusive save turn. An invalidated request cannot reach the writer or apply a late result.

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

When TechnicalSpec 009 accepts newer saved information from another tab or window, it uses the same discard operation before applying the update. This closes the unfinished action, invalidates its pending request, and prevents its older candidate from replacing the newer information.

## Essential Verification

Verify that:

- no temporary action field can appear in an encoded version-1 document;
- refresh and a later full reopen start without every listed unfinished action;
- entered money amounts, names, planned money amounts, selected delete targets, failure messages, and reorder state are not restored;
- a pending deletion is not completed and an unfinished reorder returns to the saved order;
- discarding temporary state performs no write, creates no `Balance Changes` entry, and shows no message;
- a save request still waiting when the state is discarded never reaches the writer;
- a commit completed before discard remains available after loading again;
- a back-forward-cache restoration discards the preserved runtime state and reloads saved information;
- merely hiding or unfocusing the page does not write or show a leave warning; and
- a received cross-tab update uses the same discard path.

## Boundaries

This spec does not define action input rules, action calculations, the one-open-action priority, browser-storage encoding, save coordination, cross-tab delivery, or normal dashboard and `Savings` navigation.
