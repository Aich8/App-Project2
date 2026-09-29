# TECHNICAL-SPEC-022: Reordering Saving Squares

Spec ID: `TECHNICAL-SPEC-022`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-012: Reordering Saving Squares`](FEATURE-SPEC-012-reordering-saving-squares.md)

Covers: hold-and-move recognition, temporary drag state, automatic scrolling, normal-record order allocation, and coordinated reorder saving.

Status: `Draft`

## Purpose

Let the user move one normal `Saving` to a different coverage-priority position while broken `Saving` slots remain locked, then keep the new order only after one complete browser-storage save succeeds.

## Shared Gesture Recognition

Use the TechnicalSpec 018 gesture owner and interaction context with these FeatureSpec 012 constants:

```ts
const SAVING_REORDER_HOLD_MS = 600;
const SAVING_REORDER_MOVE_PX = 8;
const SAVING_REORDER_EDGE_PX = 40;
const SAVING_REORDER_SCROLL_PX_PER_FRAME = 8;

type PendingSavingReorderGesture = PendingSavingGesture & {
  pointerType: "touch" | "mouse";
  initialClientX: number;
  initialClientY: number;
  latestClientX: number;
  latestClientY: number;
  holdStartedAtMs: number;
  holdTimerId: number | null;
  holdCompleted: boolean;
};
```

Only a primary touch contact or the main mouse button on a current normal default `Saving` can prepare a reorder. At the initial press, require a ready `Savings` view, no TechnicalSpec 009 update, no TechnicalSpec 017 history operation, and an empty TechnicalSpec 010 coordinator. The inherited `PendingSavingGesture` retains the ready-state object, stable `Saving` ID, accepted pointer ID, interaction context, and listener controller.

Use `performance.now()` or another monotonic clock for `holdStartedAtMs`. A hold-timer callback checks the elapsed time and, if it runs early, replaces itself with a timer for the remaining duration. Movement is the straight-line distance from the initial pointer coordinates in CSS pixels; reaching exactly `8px` satisfies the threshold.

- Before `600ms`, touch movement reaching `8px` is scrolling. Cancel the pending reorder, return the TechnicalSpec 018 `not-tap` result with reason `scroll`, and allow normal scrolling to continue.
- Before `600ms`, mouse movement cannot start reordering. A release produces `tap` only when movement stayed below `8px`; otherwise it produces `not-tap` with reason `canceled`.
- At `600ms`, mark the hold complete but do not start reordering until a later accepted movement event is at least `8px` from the initial point.
- After the hold completes, reserve the accepted touch sequence for the possible reorder so its later movement does not become native scrolling. Prevent default handling only for that matching pointer and only when its event is cancelable.
- Releasing after the completed hold but before movement reaches `8px` produces `not-tap` with reason `hold-without-reorder`.
- Once both requirements are met, produce `not-tap` with reason `reorder`, suppress only the matching compatibility click, and request `{ kind: "saving-reorder", savingId }` through TechnicalSpec 010.

Ignore every other pointer ID without canceling or replacing the accepted pointer. Prevent a matching native `dragstart`, text-selection sequence, or long-press context action from taking over a completed hold or active reorder, while leaving pre-hold touch scrolling available.

Before starting the action, recheck the ready state, primary view, target, update gate, history-operation gate, interaction context, empty coordinator, and captured coordinator revision. Prepare the reorder state and all pointer handling without exposing them. Use pointer capture when supported; otherwise install document-level movement, release, and cancellation listeners. If supported capture fails, a required listener cannot be installed, or any current check fails, clean up the pending gesture and start nothing. Show no message and save nothing.

## Runtime Reorder And Feedback State

```ts
type SavingReorderState =
  | {
      phase: "dragging";
      targetSavingId: string;
      pointerId: number;
      readyStateAtStart: Extract<WebsiteLoadState, { kind: "ready" }>;
      originalNormalSavingIds: readonly string[];
      projectedNormalSavingIds: readonly string[];
      latestClientX: number;
      latestClientY: number;
      grabOffsetX: number;
      grabOffsetY: number;
      draggedHeightPx: number;
      listenerController: AbortController;
      autoScrollFrameId: number | null;
    }
  | {
      phase: "saving";
      targetSavingId: string;
      readyStateAtStart: Extract<WebsiteLoadState, { kind: "ready" }>;
      originalNormalSavingIds: readonly string[];
      requestedNormalSavingIds: readonly string[];
      attemptTimeMs: number;
      requestId: symbol;
    };

type SavingReorderFeedbackState = {
  saveFailureVisible: boolean;
};
```

Create `originalNormalSavingIds` from the normal slots in their current visible order and exclude every broken slot. Start `projectedNormalSavingIds` as an immutable copy of that sequence. Capture both pointer-to-target offsets and the selected square's current height so it follows the accepted pointer without jumping.

Publish the complete dragging state, listeners, and TechnicalSpec 010 descriptor together under a new action token. A reorder uses no temporary action Back marker. While either phase exists, block every unrelated action and another reorder.

Keep `SavingReorderFeedbackState` outside the action coordinator so a failure message does not block another action. It is temporary in-memory state and is never saved. A successfully started reorder or other action clears an earlier reorder failure message in the same controller transition that publishes that action.

The ready state and base `Savings` model remain unchanged during dragging and saving. Render only the temporary dragged or requested order from `SavingReorderState`; never mutate the loaded document, loaded slots, or canonical coverage result.

## Drag Position And Projected Order

Position the selected square's temporary drag representation from `latestClientX - grabOffsetX` and `latestClientY - grabOffsetY`. Use its vertical center, calculated from that temporary top plus half `draggedHeightPx`, to decide its projected order. Horizontal movement affects only the temporary drag position; normal coverage priority remains a vertical order.

For every accepted movement, remove the target ID from the projected normal sequence. Insert it before the first remaining normal square whose current vertical center is below the dragged square's center, or after the final normal square when none is below it.

Broken slots are never movement targets and never change position. Build the temporary visible sequence by keeping each broken slot at its loaded position and filling the remaining positions from `projectedNormalSavingIds`. Derive one temporary gap at the target's projected position so the possible drop position remains represented; the gap is temporary action state, has no saved value, and disappears on drop, cancellation, or discard.

Do not publish new canonical coverage while dragging or saving. Successful result handling owns complete revalidation and coverage recalculation.

## Automatic Scrolling

While dragging, compare `latestClientY` with the current bounds of the element that scrolls the `Saving` list:

- within `40px` of the top edge, request upward scrolling;
- within `40px` of the bottom edge, request downward scrolling; and
- outside both edge areas, stop automatic scrolling.

If the two edge areas overlap, use the closer edge; an exact tie does not scroll. Run at most one `requestAnimationFrame` loop and move the list by exactly `8px` per frame in the selected direction, clamped to its scroll range. The speed does not change with edge distance. Every frame first verifies the current token, revision, descriptor, phase, ready-state object, target ID, and pointer ID.

Listen to the active `Saving` list's scroll events while dragging. After every actual scroll, whether automatic or caused by another scroll input, recalculate the projected position from the same latest pointer coordinates and current square centers.

Stop the loop when the pointer leaves the active edge area, the matching scroll boundary is reached, the pointer is released, or the reorder is interrupted or discarded.

## Drop, No-Write Result, And Interruption

Only `pointerup` from the accepted pointer while `phase` is `dragging` performs a drop. Update the final pointer coordinates and projected sequence first, stop automatic scrolling, copy the requested sequence, mark the drag as completed, and abort the drag listeners before releasing pointer capture. This ordering prevents the expected `lostpointercapture` event from canceling an accepted drop.

If the requested normal ID sequence exactly equals the original sequence, clear the reorder state and TechnicalSpec 010 entry together, remove the temporary gap, restore the canonical `Savings` model, and then release capture when held. Create no candidate or request and show no message.

For a changed sequence, capture one `attemptTimeMs` and one fresh TechnicalSpec 009 request identity, then atomically replace the dragging phase with the saving phase under the same token. Only after the drag listeners can no longer cancel it should capture be released. Keep the requested order as temporary output while saving.

During dragging, cancel and restore the canonical model after `pointercancel`, unexpected `lostpointercapture`, window focus loss, the document becoming hidden, Back or navigation, ready-state or `Savings` replacement, lifecycle discard, or newer saved information. Cancel the timer, animation frame, listeners, temporary gap, and pointer handling before clearing the action. Save nothing and show no message. Window focus loss or hiding applies only to an active drag; it does not cancel a save after the user has released the `Saving`.

During the saving phase, Back, navigation, lifecycle discard, ready-state replacement, or newer saved information first marks the pending request as no longer current, then clears the temporary requested order and action. A waiting request is aborted, and a late result is ignored. TechnicalSpecs 008 and 009 govern a writer call that already finished before invalidation.

TechnicalSpec 017 cancels an active reorder before leaving `Savings`; no action Back marker is consumed. Every stale callback checks the action token, coordinator revision, exact descriptor, phase, ready-state object, target ID, and pointer or request identity before doing anything.

## Complete Reorder Candidate

Before candidate construction, require `requestedNormalSavingIds` to contain every and only the IDs in `originalNormalSavingIds`, with each ID exactly once and the target ID exactly once. Resolve every ID to one normal slot in `readyStateAtStart`. A mismatch is an internal construction failure and never produces a save request.

The complete attempt owns immutable copies of the token, coordinator revision, exact descriptor, ready-state object, known baseline, target ID, original and requested sequences, `attemptTimeMs`, request identity, and candidate.

Inside one failure boundary, build a separate `StoredWebsiteDataV1` candidate only from `readyStateAtStart`:

1. collect every usable non-negative safe-integer `order` found in a broken raw `Saving` and reserve those numbers through the TechnicalSpec 005 rule;
2. in requested normal-ID order, assign each normal record the next available non-negative safe integer beginning at `0` and skipping every reserved number;
3. at each normal record's existing `sourceIndex`, create a fresh record with that assigned `order` and the same `id`, `name`, and `plannedMoneyAmount`;
4. keep every broken slot's exact `rawValue` at its existing `sourceIndex`;
5. keep the main money amount unchanged; and
6. remove only `Balance Changes` records expired at `attemptTimeMs`, preserving every other history record in order.

Do not reorder the stored `savings` array itself. The new valid normal-record `order` values restore the requested normal sequence while broken slots remain fixed.

If permutation checking, time capture, safe-order allocation, or candidate construction throws, cannot finish, or produces an invalid candidate before registration, create no save request and call no writer. In one transition, clear the temporary reorder and coordinator entry, restore the last successfully saved order and coverage, and set `saveFailureVisible` to `true`.

Otherwise, pass the loaded slots from `readyStateAtStart` with the candidate so the TechnicalSpec 007 encoder can verify every retained broken raw value. If the shared encoder rejects the registered candidate, `setItem` is not called and the coordinated request returns `save-failed`.

## Coordinated Save And Result

Immediately before registration, confirm that the attempt's token, revision, descriptor, ready-state object, target, immutable requested sequence, request identity, and `phase: "saving"` remain current. A mismatch stops silently. Otherwise, register one TechnicalSpec 009 `user-action` request with the known baseline from that same ready state. Perform the final check and registration without awaited work.

The active ready information remains unchanged while saving. The result handler acts only while every value owned by that same attempt remains current.

- `saved`: First build one complete TechnicalSpec 007 ready-state snapshot from the candidate and returned baseline. Revalidate the complete saved `Saving` list through TechnicalSpecs 005 and 007, rebuild TechnicalSpec 004 coverage, and set pending history cleanup to `false`. Then use one controller transition to install the complete new ready state while clearing the temporary requested order, reorder model, matching TechnicalSpec 010 entry, and failure feedback together. Show no success message.
- `save-failed`: Discard the candidate. In one controller transition, clear the temporary requested order, reorder model, and matching TechnicalSpec 010 entry, render the complete last successfully saved order and coverage, and set `saveFailureVisible` to `true`.
- `superseded` or silent invalidation: Run no reorder success or failure effect. TechnicalSpecs 008 and 009 discard the old action and load the newer information.

Success preserves every saved name and planned money amount and never changes the main money amount. If the same records remain normal, only coverage priority changes and the derived `Savings` amounts stay the same. If an unchanged broken record becomes valid after the order conflict is removed, show it normally and recalculate `Savings` from the complete resulting valid list as required by FeatureSpec 012.

Success creates no `Balance Changes` record or newest-history scrolling signal. A later reorder is a new hold-and-move gesture with a fresh snapshot, candidate, time, and request identity.

## Failure Message Lifetime

Both local candidate failure and coordinated `save-failed` show `Changes could not be saved.` through `SavingReorderFeedbackState`. Keep it visible without a timer while no later accepted rule removes it.

Clear the previous message only when a new reorder or another action successfully starts, the user leaves `Savings`, newer saved information replaces the ready state, or lifecycle discard, refresh, close, or reopen occurs. A blocked or canceled attempt that never starts an action does not clear it. Another failed reorder sets it to `true` again. A new `Savings` runtime view starts with it set to `false`.

## Essential Verification

Verify that:

- only a current normal default `Saving`, primary pointer, completed monotonic `600ms` hold, and later movement of at least `8px` can start reordering;
- an early timer is corrected, pre-hold touch scrolling remains available, and post-hold touch movement can become reorder instead of native scrolling;
- tap, pre-hold touch scroll, pre-hold mouse release after large movement, and hold without enough later movement produce their exact non-reorder results;
- pointer capture or its document-listener fallback follows only the accepted pointer and every listener is cleaned up;
- reorder claims the shared action gate and suppresses only its matching compatibility click;
- the dragged representation keeps its original grab point, the temporary gap follows the projected drop position, and neither becomes saved information;
- broken slots never become drag targets or move during the temporary or saved order;
- projected normal order follows the dragged square's vertical center while canonical loaded information remains unchanged;
- top and bottom automatic scrolling use the same `40px` trigger and fixed `8px` per-frame speed, and every actual scroll recalculates the projected position;
- cleanup always stops timers, animation frames, scroll handling, pointer handling, and temporary drag state;
- accepted drop cleanup cannot be mistaken for unexpected capture loss;
- an original-position drop, interrupted drag, stale callback, or discarded action writes nothing and restores the last saved view;
- interruption during saving invalidates the request, restores canonical output, and permits no late result effect;
- every changed requested sequence is an exact permutation of the current normal IDs before candidate construction;
- every usable broken-record order is reserved and normal records receive unique increasing safe orders in requested visible order;
- the candidate preserves every name, planned money amount, broken raw value, main money amount, and unexpired history record;
- local construction failure calls no writer, encoder rejection calls no `setItem`, and both follow the same failure-message behavior;
- a changed drop creates one coherent attempt and at most one coordinated writer call;
- success builds one complete revalidated ready state and installs it while clearing temporary reorder information in one transition;
- failure restores the complete previous order and coverage while publishing one separately owned failure message;
- stale, invalidated, or superseded requests cannot publish a candidate or message;
- the failure message has no timer, clears only through an accepted lifetime rule, and appears again after another failed reorder; and
- reorder never changes the main money amount or creates a history entry or success message.

## Boundaries

TechnicalSpec 004 owns coverage calculation. TechnicalSpec 005 owns loaded slot ordering, broken-slot identity, usable broken order values, and complete-list classification. TechnicalSpec 006 owns the atomic writer. TechnicalSpec 007 owns encoding and ready-state rebuilding. TechnicalSpec 009 owns save coordination. TechnicalSpecs 008 and 010 own lifecycle discard and one-action priority. TechnicalSpec 017 owns `Savings` navigation and reorder cancellation before leaving. TechnicalSpec 018 owns the shared tap-versus-reorder gesture contract; this spec owns its exact thresholds, drag behavior, candidate, and result handling.
