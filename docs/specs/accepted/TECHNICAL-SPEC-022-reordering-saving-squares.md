# TECHNICAL-SPEC-022: Reordering Saving Squares

Spec ID: `TECHNICAL-SPEC-022`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-012: Reordering Saving Squares`](FEATURE-SPEC-012-reordering-saving-squares.md)

Covers: hold-and-move recognition, temporary drag state, automatic scrolling, normal-record order allocation, and coordinated reorder saving.

Status: `Accepted`

## Purpose

Let the user move one normal `Saving` to a different coverage-priority position while broken `Saving` slots stay locked, then keep the new order only after one complete browser-storage save succeeds.

## Gesture State And Classification

Use the TechnicalSpec 018 gesture owner and interaction context. Handle touch through Touch Events with a non-passive active `touchmove` listener, handle mouse through Pointer Events, and ignore touch-originated Pointer Events so one physical touch sequence cannot enter both paths.

```ts
const SAVING_REORDER_HOLD_MS = 600;
const SAVING_REORDER_MOVE_PX = 8;
const SAVING_REORDER_EDGE_PX = 40;
const SAVING_REORDER_SCROLL_PX_PER_FRAME = 8;

type SavingGestureInput =
  | { kind: "touch"; touchIdentifier: number }
  | { kind: "mouse"; pointerId: number };

type PendingSavingReorderGesture = PendingSavingGesture & {
  phase: "pending";
  input: SavingGestureInput;
  scrollContainerElement: HTMLElement;
  sourceWrapperElement: HTMLElement;
  initialClientX: number;
  initialClientY: number;
  latestClientX: number;
  latestClientY: number;
  maximumDistancePx: number;
  initialEventTimeMs: number;
  lastEventTimeMs: number;
  touchSequenceReserved: boolean;
};
```

Only the first accepted touch contact or the primary mouse pointer using button `0` on a current normal default `Saving` can prepare a reorder. Require a ready `Savings` view, no TechnicalSpec 009 update, no TechnicalSpec 017 history operation, an unclaimed TechnicalSpec 010 interaction context, and an empty coordinator at its captured revision. Retain the ready-state object, stable `Saving` ID, exact input identity, connected source wrapper, connected list scroll container, and one listener controller. The inherited TechnicalSpec 018 `pointerId` mirrors the touch identifier or mouse pointer ID as an opaque interaction identity; only the mouse form may reach Pointer Events capture APIs. Ignore every additional touch or pointer.

Set `initialEventTimeMs` and `lastEventTimeMs` from the initial event's finite `timeStamp`; otherwise complete the prepared sequence as `not-tap` with reason `canceled` before publishing pending state. For each matching input movement, release, or cancellation while pending or dragging, require a finite `event.timeStamp` not earlier than `lastEventTimeMs`, then update that field. While pending, classify the event as post-hold only when `event.timeStamp - initialEventTimeMs >= 600`; an active drag needs only the continuing timestamp check. A non-finite or backward timestamp cancels pending input or interrupts an active drag. No timer is needed because no action occurs merely when `600ms` passes.

For each matching movement or release, update the latest client coordinates and the greatest straight-line distance from the initial point. Current distance at least `8px` satisfies the movement requirement; a tap is possible only when the greatest distance stayed below `8px` for the complete sequence.

### Touch Path

Install document-level `touchmove`, `touchend`, and `touchcancel` listeners under the session abort signal before drag rendering can change the list. Register `touchmove` with `{ passive: false }`. Track only the accepted `Touch.identifier` and leave native vertical scrolling available before the hold.

While pending, also observe actual scrolling of the retained list. A scroll before reservation completes the gesture as `not-tap: scroll`, including after movement below `8px`; a scroll after reservation but before promotion completes it as `not-tap: canceled`. Automatic scrolling after promotion is exempt.

- A pre-hold move below `8px` remains pending without `preventDefault()`.
- A pre-hold move reaching `8px` completes as `not-tap: scroll` without preventing native scrolling.
- On every post-hold matching `touchmove`, pending or dragging, immediately require a cancelable event, call `preventDefault()`, and continue only if `defaultPrevented` is `true`. The first success sets `touchSequenceReserved` while pending. Failure cancels pending input or an active drag.
- A pre-hold `touchend` is `tap` only when maximum movement stayed below `8px`; otherwise it is `not-tap: scroll`. A post-hold release while still pending is `not-tap: hold-without-reorder` and never starts a drop.
- A post-hold move promotes only when the sequence is reserved and current distance reaches `8px`.

Use the accepted `Touch.identifier` as TechnicalSpec 018's opaque `pointerId`, never as a Pointer Events capture ID. TechnicalSpec 018 owns matching generated-click fallback behavior.

### Mouse Path

Accept only Pointer Events with `pointerType: "mouse"`, `isPrimary: true`, and initial `button: 0`. Request pointer capture on the stable scroll container. If capture is unavailable or does not become active, install document-level movement, release, cancellation, and exit listeners under the same controller.

- Pre-hold movement never starts reorder. A pre-hold release is `tap` only when maximum movement stayed below `8px`; otherwise it is `not-tap: canceled`.
- A post-hold movement promotes only when its current distance reaches `8px`.
- A post-hold release while still pending is `not-tap: hold-without-reorder`, including after an earlier pre-hold excursion of at least `8px`.

### Completion, Interruption, And Promotion

Use one idempotent pending-completion operation. Mark the pending phase complete and give its exact `tap` or `not-tap` result to TechnicalSpec 018 before cleaning up resources. A terminal result aborts listeners, releases mouse capture when held, and clears pending state. Promotion transfers the same controller, input identity, retained elements, and mouse capture into dragging without clearing them. Mark terminal sessions finished before release so expected `lostpointercapture` does nothing.

TechnicalSpec 018 creates no compatibility-click suppression for `tap`. Every `not-tap` result creates or retains its exact `SuppressedCompatibilityClick`; that record remains independent through promotion, drop, cancellation, or failed promotion until TechnicalSpec 018 consumes, replaces, or lifecycle-clears it.

These signals interrupt either pending input or active dragging:

- matching `touchcancel` or mouse `pointercancel`;
- unexpected mouse `lostpointercapture` after capture became active;
- matching mouse movement without the main-button bit in `buttons`;
- document-level mouse `pointerout` or `mouseout` with `relatedTarget === null` while the button should be held;
- `window.blur`, hidden `document.visibilitychange`, or `pagehide`;
- layout-viewport resize, orientation change, or available `VisualViewport` `resize` or `scroll`;
- Back, navigation, ready-state or `Savings` replacement, lifecycle discard, or newer saved information; or
- a retained DOM element or other required session resource becoming unavailable.

Pending interruption returns `not-tap: canceled`. Dragging interruption restores the canonical view, saves nothing, and shows no message. TechnicalSpec 018 lifecycle discard may clear suppression with the session. Do not treat ordinary element-level `pointerleave` as interruption. While the accepted session is pending or dragging, prevent only its native `contextmenu`, element drag, and text-selection takeover.

Before promotion, recheck the ready state, primary view, target, update and history-operation gates, interaction context, empty coordinator revision, input identity, retained elements, current `8px` distance, the event's own post-hold timestamp, and touch reservation when applicable. Produce `not-tap: reorder`, prepare measurements and complete drag state, request `{ kind: "saving-reorder", savingId }`, and publish it atomically through TechnicalSpec 010. Failure removes prepared drag resources, starts no action, saves nothing, shows no message, and leaves only TechnicalSpec 018's required click suppression.

## Reorder State And Rendering

```ts
type SavingReorderState =
  | {
      phase: "dragging";
      targetSavingId: string;
      input: SavingGestureInput;
      readyStateAtStart: Extract<WebsiteLoadState, { kind: "ready" }>;
      scrollContainerElement: HTMLElement;
      sourceWrapperElement: HTMLElement;
      lastEventTimeMs: number;
      originalNormalSavingIds: readonly string[];
      projectedNormalSavingIds: readonly string[];
      latestClientX: number;
      latestClientY: number;
      grabOffsetX: number;
      grabOffsetY: number;
      draggedWidthPx: number;
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

Create `originalNormalSavingIds` from normal slots in visible order and exclude broken slots. Start the projected sequence as its immutable copy. Measure the selected wrapper once through a finite positive-size `getBoundingClientRect()` to capture its dimensions and input-to-square grab offset. Publish the complete drag state and TechnicalSpec 010 descriptor under one new token. Reorder uses no temporary Back marker and blocks every unrelated action while dragging or saving.

Keep feedback outside the action coordinator so a failure message does not block another action. A successfully started reorder or other action clears earlier reorder feedback in the same transition that publishes the new action.

The loaded document, canonical slots, coverage, and ready state remain unchanged while dragging or saving. Render only temporary drag or requested-order output.

Keep the original source wrapper mounted and connected as the same-size projected gap. Hide only its paint with opacity or an equivalent treatment; do not use `display: none`, `visibility: hidden`, or accessibility hiding. Disable its ordinary pointer hit-testing but retain it as the selected `Saving`'s only accessibility representation.

Render a separate purely visual clone in a viewport-level portal outside list layout. Give it fixed positioning, captured width and height, no pointer hits, no focusable or accessible content, and no reused DOM IDs. Position it at `latestClientX - grabOffsetX` and `latestClientY - grabOffsetY` without adding scroll offsets.

For each accepted movement, use the clone's viewport vertical center and current viewport rectangles for every other required normal square, including squares outside the visible scrollport. Missing, disconnected, non-finite, or unusable geometry cancels the drag. Remove the target ID, then insert it before the first remaining square whose center is numerically greater, or after the last square when none is greater; equal centers therefore place it after the equal square. Horizontal movement changes only the clone position.

Keep broken slots at their loaded positions and fill the other positions from `projectedNormalSavingIds`. Move the connected source wrapper to represent the projected gap. Broken slots are neither targets nor movable positions.

Every active callback is current only while its token, coordinator revision, exact descriptor, phase, starting ready-state object, target, input or request identity, and applicable connected DOM owners still match. Otherwise it stops silently.

## Automatic Scrolling

On each dragging frame, require a connected scroll container with a finite positive-height viewport rectangle. Use these inclusive bands:

- top: `rect.top` through `min(rect.top + 40px, rect.bottom)`;
- bottom: `max(rect.bottom - 40px, rect.top)` through `rect.bottom`; and
- no scroll outside the rectangle or inside it but outside both bands.

Exactly `40px` from an edge is inside its band. When bands overlap, choose the closer edge; an exact tie does not scroll. Run at most one animation-frame loop and request exactly `8px` per frame, clamped to the available range. Speed does not vary with edge distance. Stop at a boundary, outside the active band, on release, or when the session ends.

After every actual automatic or external list scroll, recompute projection from the latest accepted coordinates and current square rectangles.

## Drop, Cleanup, And Saving Transition

Use one idempotent drag-cleanup operation that marks the gesture finished, cancels animation, aborts listeners, removes the portal clone, restores the source wrapper's paint and hit testing in the requested canonical or temporary position, and then releases mouse capture. TechnicalSpec 018 click suppression is not part of this cleanup.

Only matching `touchend` or mouse `pointerup` during dragging can drop. Validate its timestamp, read final coordinates, recompute projection, and copy the requested sequence before cleanup.

- If requested and original normal-ID sequences are equal, restore canonical placement and clear the reorder state and TechnicalSpec 010 entry together. Create no candidate, request, or message.
- If they differ, capture one `attemptTimeMs` and fresh TechnicalSpec 009 request ID. In one transition, finish drag rendering in the requested position and replace dragging with saving under the same token. Render the requested order temporarily while saving.

An interruption from the shared list runs drag cleanup with canonical placement, clears the action, restores canonical order and coverage, saves nothing, and shows no message. Focus, visibility, viewport, and document-exit signals do not cancel saving after a completed drop.

During saving, Back, navigation, lifecycle discard, ready-state replacement, or newer saved information invalidates the request before clearing temporary order and action and restoring canonical output. Abort a waiting request and ignore every late result under TechnicalSpecs 008 and 009. TechnicalSpec 017 cancels a pending gesture or drag before leaving `Savings`; reorder consumes no action Back marker.

## Complete Candidate And Coordinated Result

Before construction, require the requested sequence to contain every and only original normal ID exactly once, including the target exactly once, and resolve each ID to one normal slot in `readyStateAtStart`. The attempt owns immutable token, revision, descriptor, ready state, baseline, target, sequences, time, request ID, and candidate values.

Inside one failure boundary, build a separate `StoredWebsiteDataV1` candidate from `readyStateAtStart`:

1. reserve every usable non-negative safe-integer `order` found in a broken raw `Saving` under TechnicalSpec 005;
2. assign normal records increasing unused safe orders from `0` in requested normal-ID order;
3. replace each normal record at its existing `sourceIndex` with a fresh record containing that order and its unchanged `id`, `name`, and `plannedMoneyAmount`;
4. retain every broken slot's exact `rawValue` at its existing `sourceIndex`;
5. keep the main money amount unchanged; and
6. remove only `Balance Changes` records expired at `attemptTimeMs`, preserving every other record in order.

Do not reorder the stored `savings` array. Construction, permutation, time, or safe-order failure clears temporary reorder state, restores the last saved view, shows `Changes could not be saved.`, and calls no writer. Otherwise pass the original loaded slots with the candidate so TechnicalSpec 007 can verify retained broken values; encoder rejection produces coordinated `save-failed` without `setItem`.

Immediately before registration, recheck every owned value and `phase: "saving"`, then register one TechnicalSpec 009 known-baseline `user-action` request without awaited work. The result handler acts only while the same attempt remains current:

- `saved`: build and completely revalidate one TechnicalSpec 007 ready state from the candidate and returned baseline, rebuild TechnicalSpec 004 coverage, set pending history cleanup to `false`, and install it while clearing temporary order, action state, coordinator entry, and failure feedback in one transition;
- `save-failed`: discard the candidate, restore the complete last saved order and coverage, clear temporary action state, and show `Changes could not be saved.`; or
- `superseded` or silent invalidation: publish no old result and let TechnicalSpecs 008 and 009 install newer information.

Revalidation may make a preserved broken record normal. It joins through its unchanged stored `order`; the requested sequence preserves only the relative order of records that were normal when dragging began. Final order and coverage always come from the complete revalidated ready state. If the normal-record set does not change, only coverage priority changes, so `Savings money amount` and the total amount needed remain unchanged.

Success preserves every name and planned money amount, changes no main money amount, creates no `Balance Changes` record or newest-history scrolling signal, and shows no success message.

## Failure Message Lifetime

Keep `Changes could not be saved.` visible without a timer after local or coordinated failure. Clear it only when a new reorder or another action successfully starts, the user leaves `Savings`, newer saved information arrives, or lifecycle discard, refresh, close, or reopen occurs. A blocked or canceled start does not clear it, and another failure shows it again.

## Essential Verification

Verify that:

- touch and mouse use only their assigned event paths, input identities never mix, and only mouse IDs reach pointer capture;
- eligibility, action gates, exact `600ms` event-time boundary, exact `8px` current-distance boundary, maximum-distance tap rule, and invalid or backward timestamp handling match this spec without a hold timer;
- pre-hold touch scrolling, actual list scrolling after sub-threshold jitter, post-hold reservation, failed prevention, `touchcancel`, and pending release produce their exact results;
- pre-hold mouse movement, capture and fallback handling, post-hold promotion, and release without qualifying post-hold movement produce their exact results;
- current mobile Safari, Chrome for Android, and Firefox for Android pass real-device pre-hold scrolling, jitter-then-reorder, reservation-failure, and post-hold reorder tests;
- unrelated input remains unaffected while matching context-menu, native-drag, selection, interruption, and compatibility-click behavior is contained and fully cleaned up;
- TechnicalSpec 018 creates suppression for every `not-tap` and none for `tap`, and only its accepted consume, replacement, or lifecycle rules clear that suppression;
- promotion is atomic, transfers one listener owner and stable DOM owners, and a failed promotion exposes no action or leaked resource;
- the original wrapper remains mounted, connected, correctly sized, and accessibility-visible while the portal clone remains visual-only and follows the original grab point through scrolling;
- projection measures every required normal square, uses the deterministic equal-center rule, and never moves or targets a broken slot;
- automatic scrolling applies the exact bands, overlap tie rule, fixed `8px` request, range clamping, single loop, and projection refresh after every actual scroll;
- touch and mouse drops share the same ordering result, expected capture loss cannot cancel an accepted drop, and common cleanup restores every listener, frame, overlay, wrapper, and capture resource;
- original-position drop, interruption, discard, and stale callbacks write nothing and restore the last saved view;
- saving invalidation aborts when possible and allows no late candidate, message, or action effect;
- every changed sequence is an exact permutation, order allocation skips usable broken orders, and the candidate preserves names, planned money amounts, broken raw values, the main money amount, and unexpired history;
- construction failure calls no writer, encoder rejection calls no `setItem`, and one changed drop creates at most one coordinated writer call;
- success installs one completely revalidated ready state, including any newly normal record through its unchanged order, while failure restores the complete previous order and coverage;
- failure-message lifetime and retry behavior match FeatureSpec 012; and
- reorder never changes the main money amount or creates a history entry or success message.

## Boundaries

TechnicalSpec 004 owns coverage calculation. TechnicalSpec 005 owns slot ordering, broken identity, reserved broken orders, and complete-list classification. TechnicalSpecs 006 and 007 own writing, encoding, and ready-state rebuilding. TechnicalSpec 009 owns save coordination. TechnicalSpecs 008 and 010 own discard and one-action priority. TechnicalSpec 017 owns `Savings` navigation. TechnicalSpec 018 owns the shared tap-versus-reorder result and compatibility-click contract. This spec owns exact reorder thresholds, drag behavior, candidate construction, and result handling.
