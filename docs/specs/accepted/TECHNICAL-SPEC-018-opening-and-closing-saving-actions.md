# TECHNICAL-SPEC-018: Opening And Closing Saving Actions

Spec ID: `TECHNICAL-SPEC-018`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-008: Opening Saving Actions`](FEATURE-SPEC-008-opening-saving-actions.md)

Covers: recognizing a normal `Saving` selection and opening, continuing, or closing that square's temporary action state.

Status: `Accepted`

## Purpose

Give one existing normal `Saving` access to rename, planned-money-amount change, and delete while preventing scroll and reorder gestures from opening those actions.

The one-step Back behavior implements the linked accepted FeatureSpec 008.

## Eligible Target

Resolve the selected stable `Saving` ID against the latest TechnicalSpec 007 ready state's loaded slots. Only a current normal slot can open `{ kind: "saving-actions", savingId }` through TechnicalSpec 010.

A broken slot does not open this action state. Its `Fix` and `Delete` choices come from the TechnicalSpec 005 broken read model.

```ts
type SavingActionsState = {
  savingId: string;
  outsideListenerController: AbortController;
};

type SavingActionsViewModel = {
  savingId: string;
  nameText: string;
  plannedMoneyAmountText: string;
  deleteActionText: "Delete";
};
```

TechnicalSpec 010 owns the action token. The action state keeps only its stable ID and listener controller and copies no name, planned money amount, coverage, or saved record.

Derive `SavingActionsViewModel` through a pure selector from one coherent controller snapshot containing the current ready state, `Savings` primary view, action token, revision, and complete expected descriptor. Read the name and formatted planned money amount from the one matching normal slot in that snapshot. Do not change controller state from the selector or during component rendering.

Before publishing a replacement controller snapshot and before every handler, resolve the ID again. If it no longer identifies exactly one normal slot, use one controller transition to abort the listener controller and clear the action state and its TechnicalSpec 010 entry together. Then render the base `Savings` view from that same latest snapshot. Save nothing and show no message. TechnicalSpecs 008 and 009 normally perform this cleanup before installing newer information.

## Tap And Gesture Separation

Use one gesture owner shared with the TechnicalSpec 022 reorder interaction:

```ts
type SavingGestureResult =
  | {
      kind: "tap";
      savingId: string;
      pointerId: number;
      interactionContext: ActionInteractionContext;
    }
  | {
      kind: "not-tap";
      savingId: string;
      pointerId: number;
      interactionContext: ActionInteractionContext;
      reason:
        | "scroll"
        | "hold-without-reorder"
        | "reorder"
        | "canceled";
    };

type PendingSavingGesture = {
  savingId: string;
  pointerId: number;
  interactionContext: ActionInteractionContext;
  readyStateAtStart: Extract<WebsiteLoadState, { kind: "ready" }>;
  listenerController: AbortController;
};

type SuppressedCompatibilityClick = {
  savingId: string;
  pointerId: number;
  interactionId: symbol;
} | null;
```

Create `PendingSavingGesture` and the TechnicalSpec 010 interaction context when the initial press begins, before its final gesture is known. Keep that same context through movement, release, and any compatibility click generated from the sequence. Track only its accepted pointer ID. Only a primary touch contact or the main mouse button can produce `tap`; additional pointers are ignored without canceling or replacing the accepted pointer.

Only a completed normal click or tap may request `saving-actions`. A sequence classified as scrolling, hold-without-reorder, reordering, or cancellation must suppress its later generated click and cannot open this action state.

For a `not-tap` result, keep one `SuppressedCompatibilityClick` containing that sequence's Saving ID, pointer ID, and interaction ID. A capture-phase click handler suppresses and clears only a matching compatibility click. When a click lacks a pointer ID, it may match only the same target and interaction with no intervening primary press. Clear an older suppression record before accepting any new primary press and during lifecycle discard, so it can never suppress a later distinct interaction.

Treat `PendingSavingGesture` and its suppression record as temporary action resources even before `saving-actions` opens. Leaving `Savings`, a ready-state replacement, newer stored information, `pagehide`, back-forward-cache discard, refresh, or close aborts its listeners, releases pointer capture, clears its context and suppression, and makes every later release or click from that sequence do nothing. TechnicalSpec 008's shared discard operation owns lifecycle and newer-information cleanup.

TechnicalSpec 022 owns the exact hold, movement, scroll, and reorder thresholds plus drag state. This spec consumes only the final `tap` or `not-tap` result and never starts a reorder itself.

## Opening The Action State

For a confirmed tap on a normal default square:

1. reuse the interaction context created when that press began;
2. read one coherent controller snapshot and resolve the stable ID against its latest ready slots;
3. prepare one `SavingActionsState`, its outside handler, and its listener controller without exposing them;
4. request that state with `{ kind: "saving-actions", savingId }` through every TechnicalSpec 010 direct-start check; and
5. open only when no action existed at the start of the interaction and none is active now.

Perform the final target, interaction, update, history-operation, and coordinator checks plus listener and state installation without awaited work. TechnicalSpec 010 generates the token and publishes the complete state, descriptor, and active listeners together. If preparation or listener installation fails, abort the prepared controller, leave the coordinator empty with its prior revision, open nothing, and show no message. A blocked or stale request performs the same cleanup and changes nothing.

Opening builds no saved-data candidate and performs no browser-storage write, coverage recalculation, history creation, or message.

## Available Selections

For each selection, read one coherent controller snapshot. Continue only while a ready `Savings` view is current, TechnicalSpec 009 is not updating, TechnicalSpec 017 has no active history operation, the selected ID still resolves to one normal slot, and the captured token, coordinator revision, and complete expected `saving-actions` descriptor are current.

- selecting the current name prepares `{ kind: "saving-rename", savingId }` and its TechnicalSpec 019 state;
- selecting the current planned money amount prepares `{ kind: "saving-planned-money-change", savingId }` and its TechnicalSpec 020 state;
- selecting the exact action `Delete` prepares `{ kind: "saving-delete-confirmation", target: { kind: "normal", id: savingId } }` and its TechnicalSpec 021 state; and
- selecting a blank part inside the same square does nothing and keeps `saving-actions` open.

Keep the prepared state and descriptor unexposed. Ask TechnicalSpec 017 to push the one temporary action Back marker for the existing token. After that succeeds, TechnicalSpec 010 atomically replaces `saving-actions` and its model with the prepared descriptor and model under the same token, then increases the revision. Perform the final checks, marker write, and continuation without awaited work.

If preparing the next state fails, discard it and keep the current `saving-actions` state unchanged. If marker creation fails, do the same. If publication fails after marker creation, use TechnicalSpec 017's rollback rule and keep the last complete action state when it is still current. Every failure saves nothing and shows no message.

Use one interaction owner for each selection. Resolve it in this order: name, planned money amount, `Delete`, blank area inside the selected square, the `Savings` Back action, then ordinary outside interaction. The Back branch delegates the complete clear-and-return operation to TechnicalSpecs 010 and 017; the outside handler must not close or swallow it separately. Once one branch handles the interaction, no square, blank-area, navigation, or outside handler may handle it again.

## Closing The Action State

Only a completed outside click or tap closes the matching action state and coordinator entry. An initial press, pointer movement, wheel event, or scroll event alone never closes it. Wait for the sequence to finish as a click or tap so starting a scroll outside the selected square keeps the actions open.

Interaction containment is based on the selected square's event path, so its name, amount, `Delete`, and blank internal area are not outside interactions.

Scrolling does not close an already open action state. Leaving `Savings`, refresh, close, back-forward-cache restoration, and newer stored information discard it through TechnicalSpecs 008, 009, and 017.

When a different normal square is selected while one is open, the original interaction context records that an action existed at the initial press. The completed tap may close the old state as an outside interaction, but the same context makes TechnicalSpec 010 block opening the second square. A later tap is required.

The basic `saving-actions` state creates no temporary action Back marker. For the `Savings` Back action or browser/device Back, TechnicalSpec 010 returns `not-handled`; TechnicalSpec 017 clears the basic state and its listeners, then returns to the dashboard in the same step. Deeper rename, amount-change, and delete-confirmation flows use the marker created during continuation instead.

An action state blocks reorder and every unrelated action until it closes or continues into one of its own flows.

## Event Lifetime

Install outside-interaction handling only while a current `saving-actions` token exists. Every handler captures and checks that token, the coordinator revision, the complete expected `saving-actions` descriptor, and selected ID. Remove the handling when the state closes, continues, or is discarded.

A continuation changes the revision and descriptor, so a queued tap, generated click, or outside handler from `saving-actions` cannot close or change the rename, amount-change, or delete-confirmation step that follows under the same token.

The gesture owner and outside-interaction owner share the original interaction context and mark a completed activation handled exactly once. Pending gesture callbacks also capture the starting ready-state object and stop if it is no longer current. Clear pending pointer state, compatibility-click suppression, pointer capture, and listeners when the interaction ends, the view changes, the ready state is replaced, or the action is discarded.

## Essential Verification

Verify that:

- a normal click or tap opens actions for exactly one current normal `Saving`;
- a broken slot cannot open this action state;
- scroll, hold-without-reorder, reorder, and canceled gestures never open it;
- the interaction context starts with the initial press, secondary pointers are ignored, and a non-tap gesture suppresses only the generated click matching its pointer and interaction;
- leaving `Savings`, replacing the ready state, or lifecycle discard cancels a pending press so its later release or click cannot open actions;
- opening and closing change no saved or derived information and show no message;
- the action state, descriptor, and active outside listener appear and disappear together, and failed preparation exposes none of them;
- the action-state view model comes from one coherent snapshot, and stale-target cleanup occurs before rendering rather than during render;
- selecting the name, amount, or `Delete` uses the exact descriptor and normal target, creates one Back marker, and atomically continues under the same token;
- marker failure leaves the basic actions open, while failed publication rolls the marker back without exposing a partial next step;
- a blank internal interaction keeps the action state open;
- only a completed outside click or tap closes it, while an outside press that becomes scrolling does not;
- the `Savings` Back control uses its navigation branch once and is never reduced to an ordinary outside close;
- selecting another square closes the first without opening the second from the same interaction;
- the basic action state uses no temporary Back marker, and either Back action clears it while returning to the dashboard in one step;
- an open action state prevents reorder;
- leaving `Savings` and lifecycle or cross-tab discard close it; and
- a missing or newly broken target closes silently, and stale identities or handlers with an old token, revision, or descriptor cannot affect the current state or a continued step using the same token.

## Boundaries

TechnicalSpec 010 owns the shared action gate, accepted TechnicalSpec 017 owns `Savings` navigation and temporary Back markers, and TechnicalSpec 022 owns reorder thresholds and drag behavior while following the shared gesture-result contract above. TechnicalSpecs 019 through 021 own rename, planned-money-amount change, and deletion after their matching continuation begins.
