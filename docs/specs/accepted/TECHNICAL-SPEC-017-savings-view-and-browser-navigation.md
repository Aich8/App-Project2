# TECHNICAL-SPEC-017: Savings View And Browser Navigation

Spec ID: `TECHNICAL-SPEC-017`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-006: Viewing Savings`](FEATURE-SPEC-006-viewing-savings.md)

Covers: coordinating the dashboard and `Savings` primary views, deriving a safe base `Savings` view, and managing same-document Back and Forward entries for primary views and temporary actions.

Status: `Accepted`

## Runtime Navigation State

```ts
const CASH_MONEY_ORGANIZER_HISTORY_KEY = "__cashMoneyOrganizer";

type PrimaryView = "dashboard" | "savings";

type CashMoneyOrganizerHistoryMarker = {
  owner: "cash-money-organizer";
  documentInstanceId: string;
  kind: "primary-view" | "action-back";
  view: PrimaryView;
};

type HistoryOperation = {
  kind:
    | "write-primary-marker"
    | "write-action-marker"
    | "rollback-new-marker"
    | "leave-savings"
    | "consume-action-back"
    | "restore-primary-view";
  expectedDocumentInstanceId: string;
  expectedMarkerKind:
    | "primary-view"
    | "action-back"
    | "unmarked-dashboard-base";
  expectedView: PrimaryView;
  watchdogTimerId: number | null;
} | null;
```

Keep this state in memory and never in the saved website-data document. Generate one `documentInstanceId` with `crypto.randomUUID()` for each full website document.

Store each marker under `CASH_MONEY_ORGANIZER_HISTORY_KEY` in `history.state`, keep every unrelated router-owned field, and keep the current URL. A `null` state may become an object. Treat an array, primitive, or failed structured clone as a marker failure instead of overwriting unknown state.

Before exposing the first ready view, use `history.replaceState` to mark the current entry as this document's dashboard. Fresh documents, successful `Start again`, and recovery exit select `dashboard`. Loading and recovery continue to override `PrimaryView`.

If that first replacement fails, still show the complete ready dashboard and remember the current entry as this document's unmarked initial dashboard base. Recognize it only when Back returns directly from a marker that this controller successfully pushed above it. It may select only `dashboard` and can never restore `Savings` or an action. Clear this fallback after a dashboard marker is successfully written.

Keep at most one `HistoryOperation`. Record the exact expected document, marker kind, and view before starting. Synchronous writes use no watchdog and clear the operation after their complete guarded transition finishes.

## Savings View Model

```ts
type SavingSquareViewModel =
  | {
      kind: "normal";
      savingId: string;
      nameText: string;
      plannedMoneyAmountText: string;
      coverageState: SavingCoverageState;
      neededText: string | null;
    }
  | {
      kind: "broken";
      targetRuntimeKey: symbol;
      messageText: "Saving could not be loaded.";
      fixActionText: "Fix";
      deleteActionText: "Delete";
    };

type SavingsViewModel = {
  title: "Savings";
  backAction: "return-to-dashboard";
  createSavingActionText: "+";
  moneyLabel: "Savings money amount";
  savingsMoneyAmountText: string;
  overallNeededText: string | null;
  savingSquares: readonly SavingSquareViewModel[];
};
```

Derive the whole view model from one TechnicalSpec 007 ready-state snapshot. Include every loaded normal and broken slot in visible order without a count limit.

For normal slots, run TechnicalSpec 004 once in that order. Require one matching same-ID result per slot, use the shared exact money formatter, and use that same calculation for every square, `savingsMoneyAmountText`, and `overallNeededText`. A mismatch publishes no partial view model.

For broken slots, use the safe TechnicalSpec 005 read model. Never expose their raw value, validation reasons, source index, or other saved-data internals. The Back command uses this spec, and `+` delegates to TechnicalSpec 003 through TechnicalSpec 010. Matching technical specs layer temporary actions over this base model.

Do not keep a separate ready-document copy after opening `Savings`. Re-derive the complete model from each accepted ready-state replacement.

## Opening And Updating Savings

Open `Savings` only through one TechnicalSpec 010 interaction context while a coherent ready dashboard is current, its `Savings` action still exists, TechnicalSpec 009 is not updating, `HistoryOperation` is empty, and the interaction and empty coordinator still have their captured unclaimed state and revision.

Without awaited work, claim the interaction, push one current-document `primary-view` marker for `savings`, and then select `savings` from that same ready snapshot. Treat the marker and controller publication as one guarded transition. Do not push another marker when `Savings` is already current. A failed marker leaves the dashboard usable; a successful marker followed by failed publication uses the rollback rule below.

When newer usable information arrives from another tab or window, TechnicalSpecs 008 and 009 discard any action first. Keep `Savings` open and derive one complete replacement view model.

When newer information requires recovery, keep it staged and keep the update gate active while these steps finish:

1. discard any action model and TechnicalSpec 010 entry;
2. consume a current `action-back` marker, waiting for its expected result or safe failure cleanup; and
3. in one controller transition, select `dashboard`, normalize the current marker when possible, and install recovery.

Skip steps that have nothing to clean. Never expose a ready dashboard between `Savings` and recovery. Leaving recovery therefore shows the dashboard.

## Temporary Action Back Step

For every action whose accepted behavior lets Browser Back close it, require no active history operation and a current-document marker for the active primary view, or the recorded unmarked dashboard base. Prepare the action without exposing it, push one `action-back` marker, and publish the action only after that succeeds. If the marker write fails, clear the prepared action and coordinator entry together. Use the rollback rule if publication fails after the push.

The marker contains only its fixed owner, document ID, kind, and primary view. It contains no action, target, input, confirmation, money, or saved information. Reuse the same marker while one TechnicalSpec 010 token continues between steps.

Browser, device, or system Back consumes that entry first. TechnicalSpec 010 then closes the matching action and keeps the recorded primary view. When website Back or another normal path closes the action without consuming browser history, run `consume-action-back` once. Its matching `popstate` is internal and cannot close another action, leave `Savings`, or change `PrimaryView`. Block new actions and normal navigation until it finishes. Newer-information discard uses the same cleanup while the document remains active.

Do not start a traversal during `pagehide`, unload, or another document-ending path. TechnicalSpec 008 still clears the action in memory. On back-forward-cache return, normalize a stale current `action-back` marker before TechnicalSpec 009 reconciliation. When Forward reaches such a marker with no live matching action, replace it with the recorded `primary-view` marker and never restore the action.

## Back And Forward

Pass every website, browser, device, or system Back request to TechnicalSpec 010 first:

- `back-consumed` closes only the action and keeps `PrimaryView`. Website Back also consumes its current temporary marker. If Browser Back reached the wrong entry because the marker was missing, restore a current-document marker for the unchanged view when possible.
- `not-handled` while `Savings` is current clears a basic `saving-actions` state or cancels an active reorder before leaving. Reorder cancellation stops its timers, pointer handling, and automatic scrolling and restores the last saved order.
- `not-handled` on the dashboard adds no synthetic step and allows normal browser navigation.

When no action consumes Back, require one ready state, a current-document `savings` marker, and no active history operation. The section's Back command starts `leave-savings` and moves back once; Browser Back uses the traversal already made. Select `dashboard` only after confirming the expected dashboard marker or recorded unmarked dashboard base, then render it from the latest ready state through TechnicalSpec 011. No temporary `Savings` state remains afterward.

Browser Forward selects `Savings` only from a valid current-document `savings` marker while a complete ready state exists. It uses the latest ready information and restores no action, input, confirmation, reorder, or failure message. Markers from an earlier document are ignored, so refresh, close, or reopen cannot restore an old view or action.

## Navigation Safety

Use one controller for marker writes, traversals, and `popstate`. Never start a second operation while one is active, and handle each matching result exactly once.

Before `pushState` or `replaceState`, preserve unrelated state and confirm that the candidate state can be structured-cloned. A failed write keeps the last complete view. If a push succeeds but its view or action cannot be published, change the operation to `rollback-new-marker`, move back once, and treat the result as internal. If rollback fails, replace the orphan marker with the previous primary-view marker when possible. Clear prepared action state, expose no partial view, show no message, and save nothing.

If any internal traversal throws or reaches an unexpected entry, keep the last complete primary view, normalize the current entry to its safest current-document primary marker when possible, and clear the operation without changing saved information.

Every asynchronous `leave-savings`, `consume-action-back`, `restore-primary-view`, or `rollback-new-marker` operation starts one `1,000ms` watchdog. A matching `popstate` cancels it and completes the operation. If the watchdog runs first, inspect the current entry: apply the expected result once if it already arrived; otherwise keep the last complete view and normalize the current marker when possible. Then clear the operation. A later `popstate` is handled as a new event and cannot restore a stale action. Completion, failure, and document shutdown cancel the watchdog.

Classify a reached history entry before using it:

- a current-document `primary-view` marker may select its view only while ready;
- a stale current-document `action-back` marker becomes its recorded primary marker and restores no action;
- the recorded unmarked initial base may select only `dashboard` after the direct return described above;
- every foreign, invalid, or other missing marker restores neither `Savings` nor an action; and
- loading and recovery override every marker.

## No Data Effects

Opening, viewing, updating, or leaving `Savings`, and every history cleanup in this spec, writes no website data, changes no money amount or `Saving`, creates no `Balance Changes` record, and shows no message.

## Essential Verification

Verify that:

- the first ready view selects the dashboard, and failed initial marker creation does not trap the website in loading;
- `Savings` opens from one current interaction and snapshot only after its marker succeeds;
- one safe view model contains every loaded slot and one matching coverage result for every normal slot;
- newer usable information replaces the complete `Savings` model, while recovery waits for action-history cleanup and exposes no intermediate dashboard;
- action continuation reuses one marker, and every close consumes or safely normalizes it once;
- Back closes a consuming action before leaving `Savings`, then returns to the latest dashboard exactly once;
- Forward restores `Savings` only from a valid marker in the current document and never restores temporary state;
- marker failure, publication failure, rollback failure, and watchdog completion leave one usable complete view and clear the operation gate;
- lifecycle shutdown starts no history traversal, and back-forward-cache return normalizes stale action markers; and
- no operation owned here changes or saves product information.

## Boundaries

TechnicalSpec 003 owns `Saving` creation. TechnicalSpec 004 owns coverage calculations, and TechnicalSpec 005 owns broken-slot handling. TechnicalSpec 007 owns ready and recovery information. TechnicalSpecs 008 through 010 own discard, received updates, and action coordination. TechnicalSpec 011 owns root and dashboard rendering. TechnicalSpecs 018 through 022 own the matching `Saving` actions and reorder behavior.
