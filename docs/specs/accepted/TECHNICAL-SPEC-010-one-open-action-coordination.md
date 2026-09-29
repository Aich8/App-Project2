# TECHNICAL-SPEC-010: One-Open-Action Coordination

Spec ID: `TECHNICAL-SPEC-010`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-019: Keeping One Action Open At A Time`](FEATURE-SPEC-019-keeping-one-action-open-at-a-time.md)

Covers: coordinating unfinished actions so only one can be open and one interaction cannot close one action and start another.

Status: `Accepted`

## Purpose

Give every user action one shared gate while leaving its input, confirmation, saving, and closing rules in its matching feature code.

## Coordinator State

```ts
type SavingActionTarget =
  | { kind: "normal"; id: string }
  | { kind: "broken"; runtimeKey: symbol };

type OpenActionDescriptor =
  | { kind: "main-money-actions" }
  | { kind: "main-money-entry"; action: "add" | "subtract" | "modify" }
  | { kind: "balance-change-delete-actions"; recordId: string }
  | { kind: "balance-change-delete-confirmation"; recordId: string }
  | { kind: "saving-actions"; savingId: string }
  | { kind: "saving-create" }
  | { kind: "saving-rename"; savingId: string }
  | { kind: "saving-planned-money-change"; savingId: string }
  | { kind: "saving-fix"; runtimeKey: symbol }
  | { kind: "saving-delete-confirmation"; target: SavingActionTarget }
  | { kind: "saving-reorder"; savingId: string };

type ActiveAction = {
  token: symbol;
  descriptor: OpenActionDescriptor;
};

type ActionCoordinatorState = {
  activeAction: ActiveAction | null;
  revision: number;
};

type ActionInteractionContext = {
  interactionId: symbol;
  activeActionTokenAtStart: symbol | null;
  coordinatorRevisionAtStart: number;
  claimed: boolean;
};

type BackHandlingResult = "back-consumed" | "not-handled";
```

Begin with no active action and revision `0`. Increase the revision after each successful start, continuation, close, or discard. A blocked request changes neither value.

Keep this state in memory only. The coordinator owns the token and descriptor; the matching action owns its temporary model. Install, replace, or clear the descriptor and its model together so neither can exist without the other.

## Interaction And Direct Start

Create one `ActionInteractionContext` for each user activation. One pointer or touch context lasts from the initial press through its hold, release, and resulting click. A keyboard-generated click reuses its key context or is suppressed. Remove the context after that complete activation or after a cancellation that cannot produce another click.

The context permanently records the action token and coordinator revision present when the interaction began. It is `claimed` after it successfully starts an unrelated action or opens `Savings`.

Start an unrelated action only when all of these are true:

- one complete ready state is active, TechnicalSpec 009 is not updating it, and TechnicalSpec 017 is not performing an internal Back-step operation;
- the control and target are available in the current dashboard or `Savings` view under their matching action spec;
- the interaction began with no active action and is still unclaimed; and
- the coordinator is still empty and its revision has not changed since the interaction began.

Perform the checks and installation without awaited work between them. Create a new token and install the complete descriptor and temporary model together, increase the revision, and claim the interaction.

Otherwise return `blocked`. The blocked request creates no model, changes and saves nothing, and shows no message. An interaction that began while another action was open may close that action under its accepted outside-interaction rule, but it cannot start a second action. A later interaction is required.

## Continuing The Same Action

Allow only these continuations:

- `main-money-actions` to the selected `main-money-entry` action;
- `balance-change-delete-actions` to `balance-change-delete-confirmation` for the same record ID; and
- `saving-actions` to `saving-rename`, `saving-planned-money-change`, or `saving-delete-confirmation` for the same normal `Saving` ID.

Validate the current token, descriptor, and target. Then replace the old temporary model and descriptor together, keep the token, increase the revision, and never expose an empty coordinator between the two steps. Reject a stale, mismatched, or unlisted continuation without changing anything.

Steps inside one descriptor, such as confirming an entered amount, need no new action slot. A broken `Saving` fix or deletion, `Saving` creation, and reorder start directly when the coordinator is empty.

## Current-Step Ownership And Closing

Every action-owned handler, timer, scheduled callback, promise handler, and save-result handler captures its token, coordinator revision, and complete expected descriptor. It must confirm that all three still match before it changes temporary state, requests a save, continues, or closes. A mismatch stops silently without changing or saving anything, closing an action, or showing a message.

This check prevents a callback from an earlier step from affecting a later step that deliberately keeps the same token.

Cancel, completion, an accepted outside interaction, or another accepted close clears the matching model, action-only handlers, and coordinator entry together, then increases the revision. A save keeps the action active while pending. Its success, failure, invalid, and no-action results follow the matching action spec.

TechnicalSpec 008 uses the same combined clear for lifecycle discard. Newer information from another tab or window uses that discard path. Clearing an already empty coordinator or a replaced token changes nothing and does not increase the revision.

## Savings, Back, And Reorder

`Savings` is navigation state, not an open-action descriptor. Opening it uses the same interaction safeguards: the ready dashboard is current, updating and internal Back-step work are inactive, the interaction began with no action and is unclaimed, and the empty coordinator revision is unchanged. Opening it claims the interaction. Actions inside `Savings` use the shared coordinator.

Pass every website, browser, device, or system Back request to the coordinator before normal navigation. Return `back-consumed` when the current action accepts Back: clear that action and keep the current primary view. Return `not-handled` when no action consumes it; only then may TechnicalSpec 017 navigate normally.

For an action whose accepted feature lets Browser Back close it, TechnicalSpec 017 owns one temporary same-document Back step:

- create it before exposing the action, or before exposing the first continuation step that requires it;
- reuse it throughout continuations under the same token;
- let Browser Back consume it, and remove it internally after website Back, another normal close, lifecycle discard, or newer-information discard while the document remains active; and
- block new actions and normal navigation while an internal removal is finishing.

The step stores only its fixed marker, current document identity, and current primary view. It stores no action, target, input, money, or saved information and cannot restore an action in a later document.

A reorder requests `saving-reorder` only after its accepted gesture starts. It blocks every other action until drop or cancellation clears it. A broken `Saving` is never a reorder target.

## Essential Verification

Verify that:

- the coordinator never has more than one active action;
- one interaction, including a generated click, cannot close one action and start another;
- a direct start requires every current-state, view, target, interaction, and revision check above;
- each start, continuation, close, and discard changes the descriptor and matching model together;
- only the listed continuations keep a token, and each one increases the revision;
- stale callbacks and late save results cannot affect a newer action or later step;
- a pending or failed save keeps the slot for as long as its matching feature keeps the action open;
- `Savings` uses no slot, while its actions and reorder use the shared slot;
- a consumed Back closes only the action and keeps the primary view, while only an unhandled Back may navigate;
- TechnicalSpec 017 keeps at most one action Back step, stores no action information in it, and handles its internal removal only once; and
- blocking or rejecting work performs no save, changes no saved information, creates no `Balance Changes` entry, and shows no message.

## Boundaries

Individual action specs own their data, validation, calculations, success effects, and accepted closing behavior. TechnicalSpec 008 owns lifecycle discard, TechnicalSpec 009 owns received-update blocking and invalidation, and TechnicalSpec 017 owns the temporary Back step plus primary-view and browser-history handling after the coordinator returns a Back result.
