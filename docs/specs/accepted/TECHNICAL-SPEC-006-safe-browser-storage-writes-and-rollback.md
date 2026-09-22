# TECHNICAL-SPEC-006: Safe Browser-Storage Writes And Rollback

Spec ID: `TECHNICAL-SPEC-006`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-016: Handling Changes That Cannot Be Saved`](FEATURE-SPEC-016-handling-changes-that-cannot-be-saved.md)

Covers: writing one complete website-data candidate safely and leaving the previous information unchanged when the write fails.

Status: `Accepted`

## Purpose

A requested change becomes active only after its complete saved result has been written successfully.

## Writer Boundary

```ts
const WEBSITE_STORAGE_KEY = "cash-money-organizer-website-data";

type SaveWebsiteDataResult =
  | { status: "saved"; serialized: string }
  | { status: "save-failed" };

type SaveWebsiteData = (
  candidate: StoredWebsiteDataV1,
  currentSavingSlots: readonly LoadedSavingSlot[],
) => SaveWebsiteDataResult;
```

TechnicalSpec 007 defines `StoredWebsiteDataV1`, `LoadedSavingSlot`, and the shared encoder. `currentSavingSlots` is used only to confirm that any broken raw `Saving` value was preserved unchanged. It is never serialized.

`SaveWebsiteData` is an internal browser-only writer. Actions and automatic cleanup request writes through the coordinator owned by TechnicalSpec 009 rather than calling the writer directly.

## Complete Candidate

The code requesting the save builds a separate candidate from the latest active usable information without changing that active information.

The candidate contains every saved change that must succeed together. For example, a new main money amount and its new `Balance Changes` record belong to one candidate. A repaired `Saving` and its corrected order values also belong to one candidate.

The candidate contains saved information only. It excludes typed input, open flows, confirmations, drag state, messages, coverage results, and other temporary or derived information. It also excludes `bigint`, `symbol`, `undefined`, functions, non-finite numbers, and unsupported objects.

Each candidate `Saving` is either a valid normal record or an unchanged raw value from a currently loaded broken slot. Candidate construction must not create or edit an invalid raw value.

Invalid, canceled, no-action, and viewing-only results create no candidate and request no write. Every valid candidate that reaches the writer is encoded and written; the writer does not decide whether the action was a no-op.

## Coordination Requirement

Before calling the writer, the TechnicalSpec 009 coordinator must:

1. confirm that the request is still active;
2. take an exclusive save turn shared by the website's open tabs and windows;
3. check that browser storage still contains the value on which the candidate was based;
4. discard the candidate and load newer information silently when another tab or window has already saved something newer;
5. If the latest-value check cannot be completed, return `save-failed` without calling the writer.
6. Call the writer only if the request is still current and its saved-data starting point still matches.

Repeated confirmation of one pending request must not queue another write. Canceling the action or receiving newer saved information invalidates its pending request before it can write.

TechnicalSpec 009 owns the stored-value baseline, request identities, exclusive coordination mechanism, and received-update handling. TechnicalSpec 007 owns the special `Start again` behavior when the first storage read did not produce a known value.

## Write Algorithm

For one candidate:

1. Encode and validate the complete candidate inside a failure boundary.
2. If validation or encoding fails, return `save-failed` without calling `setItem`.
3. Call `window.localStorage.setItem` exactly once with `WEBSITE_STORAGE_KEY` and the serialized complete document.
4. If browser-storage access or `setItem` throws, return `save-failed`.
5. If `setItem` returns normally, return `saved` with the exact serialized value.

The writer never removes the existing key before writing and never saves separate fields under separate keys. The one successful `setItem` call is the commit boundary. No read-back verification or rollback write is performed.

## Result Handling

On `saved`, the coordinator records the returned serialized value as the new stored-data starting point. The code requesting the save then makes the candidate active and performs the matching action's success-only effects.

On `save-failed`:

- discard the candidate;
- leave browser storage and the stored-data starting point unchanged;
- keep the latest active usable information;
- perform no success-only effect;
- create no new `Balance Changes` record; and
- never try to restore the previous document with another write.

The requesting action then applies the accepted failure behavior from FeatureSpec 016, including `Changes could not be saved.` and keeping the relevant retry or cancel choice available. Automatic cleanup remains silent as defined by TechnicalSpec 007.

## Essential Verification

Verify that:

- one successful request produces exactly one complete `setItem` call;
- related saved changes either commit together or remain entirely inactive;
- validation failure, blocked or unavailable storage, quota failure, and a thrown `setItem` return `save-failed`;
- a failed first write does not create the storage key;
- a failed later write leaves the previous stored value and active information unchanged;
- invalid, canceled, no-action, viewing-only, stale, and invalidated requests never reach the writer;
- temporary state and unsupported values never reach serialization;
- recovery replaces unreadable data with one complete empty document and never removes the key first; and
- failure never triggers a rollback write.

## Boundaries

TechnicalSpec 007 owns the saved-data schema, encoder, loading, cleanup results, and recovery details. TechnicalSpec 009 owns cross-tab coordination and pending-request details. Individual action specifications own calculations, success effects, and failure-message lifetimes.
