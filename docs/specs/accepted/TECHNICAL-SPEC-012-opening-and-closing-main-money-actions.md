# TECHNICAL-SPEC-012: Opening And Closing Main Money Actions

Spec ID: `TECHNICAL-SPEC-012`

Spec type: `TechnicalSpec`

Implements: [`FEATURE-SPEC-020: Opening Main Money Actions`](FEATURE-SPEC-020-opening-main-money-actions.md)

Covers: deriving the available main money actions and opening, choosing, or closing them from the dashboard.

Status: `Accepted`

## Purpose

Show only the main money actions allowed by the current money amount and connect each choice to its matching entry flow without opening two actions.

## Available-Action Selector

Use the validated `mainMoneyAmountCents` from the current TechnicalSpec 007 ready state.

```ts
type MainMoneyAction = "add" | "subtract" | "modify";

type MainMoneyActionAvailability = Readonly<{
  add: boolean;
  subtract: boolean;
  modify: true;
}>;

const MAX_MAIN_MONEY_AMOUNT_CENTS = 99_999_999;

function selectMainMoneyActions(
  mainMoneyAmountCents: number,
): MainMoneyActionAvailability {
  if (mainMoneyAmountCents === 0) {
    return { add: true, subtract: false, modify: true };
  }
  if (mainMoneyAmountCents === MAX_MAIN_MONEY_AMOUNT_CENTS) {
    return { add: false, subtract: true, modify: true };
  }
  return { add: true, subtract: true, modify: true };
}
```

The selector accepts only an integer from `0` through `99_999_999`. A `true` value means that action is available. Map the internal actions to the exact visible texts `Add`, `Subtract`, and `Modify`.

The availability object is used only to decide which actions are available.

Do not store a second copy of the availability result. Derive it from the current ready state whenever the main money actions render.

## Open State

Use this temporary runtime model:

```ts
type MainMoneyActionsRuntime = {
  listenerController: AbortController;
};
```

It exists only in memory and stores no money, available-action result, or user information. Its controller owns the outside-interaction and dashboard-scroll listener lifetime.

The actions are open only when the TechnicalSpec 010 coordinator contains the active descriptor `{ kind: "main-money-actions" }` and the matching action-specific temporary state contains one `MainMoneyActionsRuntime`. Do not keep a separate open boolean.

Obtain one TechnicalSpec 010 interaction context when the pointer or touch press begins, or when keyboard activation begins. Reuse that context through release and any resulting click; do not create a new context after an earlier handler closes an action.

If the actions are closed, clicking the main money amount opens them. If the actions are open, clicking the main money amount closes them. Tapping the main money amount or activating it with the keyboard follows the same rule.

Use the captured interaction context to follow exactly one of those two paths. When closing, clear the matching runtime model and descriptor, then stop without requesting an open. When opening, request the descriptor and a new runtime model together. The open request succeeds only when the dashboard is ready, actions are permitted, and no action was open when the interaction began. Install the runtime model and descriptor together through TechnicalSpec 010.

On success, render one semantic button for every action whose availability value is `true`. Opening the actions changes no ready information, builds no saved-data candidate, performs no save, creates no `Balance Changes` entry, and shows no message.

After the open actions finish rendering, install the dashboard-scroll listeners with the runtime model's signal. The setup callback must first confirm the current token, coordinator revision, expected `main-money-actions` descriptor, and matching runtime model. Record each scrolling source's current position during setup. Only a later event that changes one of those positions closes the actions, so movement caused by rendering the open actions cannot immediately close them.

A newly activated dashboard has no active descriptor, so its main money actions are closed.

## Choosing An Action

When an action button is selected:

1. read one current controller snapshot;
2. confirm that the captured token and coordinator revision still match the expected `main-money-actions` descriptor and its runtime model;
3. confirm that the snapshot contains one complete ready state, the dashboard is current, TechnicalSpec 009 is not `update-in-progress`, and TechnicalSpec 017 is not performing an internal Back-step operation;
4. derive the availability result from that snapshot's ready state;
5. reject the selection if its internal action is no longer available;
6. create a fresh TechnicalSpec 001 entry state for that selected action; and
7. use the TechnicalSpec 010 continuation operation to replace the old runtime model and `main-money-actions` descriptor with the entry state and matching `main-money-entry` descriptor under the same token.

Perform the checks and continuation without awaited work between them. Creating the entry state and changing the descriptor must succeed together, and the entry state's action must equal the descriptor's action. A rejected or stale selection stops silently, creates no entry state, changes no coordinator state, performs no save, and shows no message.

The continuation starts with the TechnicalSpec 001 input display at `0.00` and shows no earlier save-failure message. TechnicalSpec 001 focuses the input after it renders. That focus callback captures the continued action's token, new coordinator revision, and complete expected `main-money-entry` descriptor and acts only while all three still match; a stale focus callback does nothing. The matching action executor owns what happens after confirmation.

## Closing Without A Choice

Close the active `main-money-actions` token when:

- the main money amount is selected again;
- a click or tap is outside both the main money amount control and its action controls; or
- any active dashboard scrolling source scrolls, whether the design uses page scrolling or a separately scrolling dashboard area.

Closing aborts the runtime model's listener controller and clears the matching runtime model and descriptor through one coordinated TechnicalSpec 010 close. It does not change the ready state, call the save coordinator, create a `Balance Changes` entry, or show a message.

An inside interaction does not run the outside-close path. Selecting an action uses the continuation path instead of closing first.

When an outside interaction also targets another action, pass that same interaction context through TechnicalSpec 010. The current main money actions may close, but the targeted second action cannot open from that interaction.

## Event Lifetime

Install outside-interaction handling only after the start succeeds. After the open actions render, install scroll handling for every scrolling source used by the active dashboard as described above. Register directly on each actual scrolling source or use a capture mechanism that observes it. Keep all of this handling only while `main-money-actions` is active, and abort the runtime model's listener controller as soon as the token closes, continues into an entry flow, or is discarded.

Every handler checks its captured token, coordinator revision, and expected `main-money-actions` descriptor before changing state. After continuation increases the revision and installs `main-money-entry`, a queued old handler stops silently instead of closing the entry flow.

Refresh, close, back-forward-cache restoration, and a newer cross-tab state discard the open actions through TechnicalSpec 008. TechnicalSpec 009 update-in-progress does not permit them to reopen.

## Essential Verification

Verify that:

- `0.00$` produces only `Add` and `Modify`;
- an amount between the limits produces `Add`, `Subtract`, and `Modify`;
- `999,999.99$` produces only `Subtract` and `Modify`;
- availability always comes from the latest ready-state integer cents and is used only to decide which actions are available;
- a ready dashboard begins with the actions closed;
- the interaction context begins with the initial pointer, touch, or keyboard activation and is reused through any resulting click;
- selecting the main money amount while the actions are closed opens exactly one matching runtime model and shared action descriptor without a saved change, history entry, or message;
- choosing an available action from one current controller snapshot atomically starts one fresh matching entry flow under the same token;
- a stale or unavailable choice creates no entry flow, save, or message;
- the input is focused only after rendering and a stale focus callback cannot affect a later action;
- rendering the open actions cannot close them through their scroll handling, while a later dashboard scroll-position change does close them;
- clicking, tapping, or using the keyboard on the main money amount opens the actions when they are closed or closes them when they are open, and one interaction never closes and immediately reopens them;
- using an outside interaction closes the actions without a saved change or message, and a later position change in any active dashboard scrolling source does the same;
- an inside interaction does not trigger outside closing;
- one outside interaction cannot close these actions and open another action; and
- discarded handlers and handlers from the earlier `main-money-actions` step cannot affect a newer action or its continued entry flow.

## Boundaries

TechnicalSpec 010 owns the shared action gate. TechnicalSpec 001 owns money amount entry, and TechnicalSpecs 013 through 015 own the confirmed `Add`, `Subtract`, and `Modify` results. TechnicalSpec 011 owns the base dashboard render.
