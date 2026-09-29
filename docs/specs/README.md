# Specification System

This directory contains the specification workflow for the Cash Money Organizer website.

## Source Of Truth

Only specifications inside `accepted/` are approved sources of truth.

Files inside `drafts/` are proposals waiting for explicit user approval. Files inside `../raw-data/` are supporting information only. Neither can override an accepted specification.

## Specification Types

### GlobalSpec

A `GlobalSpec` explains the whole project briefly and in plain language. It covers the purpose, audience, main product directions, and global boundaries.

It must remain standalone and short. It may name major product directions, but it must not explain one feature in detail and must not contain feature flows, edge-case catalogs, input rules, UI timing, storage formats, source paths, framework choices, or implementation steps.

### FeatureSpec

A `FeatureSpec` explains exactly one user-visible feature and what that feature must do. It contains that feature's user-visible behavior, flows, business rules, edge cases, failure behavior, and acceptance expectations.

It explains the required result, not how to implement it. It must not contain architecture, storage formats, data schemas, algorithms, source paths, framework choices, or other technical details. Shared user-visible behavior receives its own numbered feature spec instead of being duplicated across unrelated features.

### TechnicalSpec

A `TechnicalSpec` is one focused part of the technical side of a matching `FeatureSpec`. It explains how to implement part or all of that feature's required result and may contain architecture, data structures, storage formats, algorithms, interfaces, implementation constraints, migration rules, and technical verification.

Each technical spec must identify and link to exactly one feature spec. Multiple technical specs may implement the same feature spec. Each technical spec should cover one clearly named technical responsibility and must not define new user-visible behavior or change the feature's accepted behavior.

## Naming Rules

Every specification name must answer two questions:

1. What type of specification is it?
2. What project, feature, system, or technical area does it cover?

Use one of these exact filename patterns:

- `GLOBAL-SPEC-<NNN>-<project-scope>.md`
- `FEATURE-SPEC-<NNN>-<feature-scope>.md`
- `TECHNICAL-SPEC-<NNN>-<technical-scope>.md`

Use a matching H1 title:

- `# GLOBAL-SPEC-<NNN>: <Project Scope>`
- `# FEATURE-SPEC-<NNN>: <Feature Scope>`
- `# TECHNICAL-SPEC-<NNN>: <Technical Scope>`

`<NNN>` is a permanent three-digit number beginning with `001`. The prefix stays uppercase; the filename scope uses lowercase kebab-case. Example: `FEATURE-SPEC-001-adding-money.md` with the H1 `# FEATURE-SPEC-001: Adding Money`.

Numbers are unique within each spec type and must never be renumbered or reused. `GlobalSpec`, `FeatureSpec`, and `TechnicalSpec` use independent number sequences.

A feature may have multiple technical specs. Every technical spec links to exactly one feature spec in its metadata, while using its own technical number and focused technical scope. For example, both `TECHNICAL-SPEC-001-adding-money-storage.md` and `TECHNICAL-SPEC-002-adding-money-input-handling.md` may contain `Implements: FEATURE-SPEC-001-adding-money.md`.

Number `000` is reserved only for temporary legacy baseline specs during this migration. Do not use it for any new specification.

The scope must be specific. Names such as `global.md`, `functional1.md`, `technical1.md`, `FEATURE-SPEC-001-feature.md`, and `new-spec.md` are not allowed.

Each spec starts with its type, coverage, and current status directly under the H1 title.

## Draft And Acceptance Workflow

1. Put unstructured notes, research, references, exports, and temporary information in `../raw-data/` when useful.
2. Create every new spec in `drafts/`. A proposed material revision to an accepted spec also starts in `drafts/`.
3. Keep the draft filename and H1 compliant with the type-and-scope naming rules.
4. Review the draft with the user. A draft remains unaccepted until the user explicitly says it is approved and may be accepted.
5. After explicit approval, change its status to `Accepted` and move it to `accepted/`.
6. Remove the draft copy after the accepted copy exists. Git history keeps previous accepted versions.

Do not infer approval from silence, discussion, review feedback, or a request to create a draft. If approved content is materially edited before the move, request approval again.

## Current Accepted Specs

- `accepted/GLOBAL-SPEC-002-manual-cash-tracker.md`: project purpose, audience, main directions, and global boundaries.
- `accepted/FEATURE-SPEC-001-adding-money.md`: user-visible behavior and rules for manually adding money.
- `accepted/FEATURE-SPEC-002-subtracting-money.md`: user-visible behavior and rules for manually subtracting money.
- `accepted/FEATURE-SPEC-003-modifying-the-money-amount.md`: user-visible behavior and rules for silently correcting the complete money amount.
- `accepted/FEATURE-SPEC-004-viewing-balance-changes.md`: user-visible behavior and rules for reviewing successful `Add` and `Subtract` actions, including exact entry text, fixed English date-and-time output, 720-hour visibility, and newest-entry scrolling.
- `accepted/FEATURE-SPEC-005-deleting-a-balance-change.md`: user-visible behavior and rules for deleting one `Balance Changes` entry, including stale-confirmation closure and save-failure message lifetime.
- `accepted/FEATURE-SPEC-006-viewing-savings.md`: user-visible behavior and rules for opening, viewing, updating, navigating back to, and leaving the `Savings` planning section.
- `accepted/FEATURE-SPEC-007-creating-a-saving.md`: user-visible behavior and rules for creating one new `Saving` square, including planned-money typing, last-place insertion, and save-failure message lifetime.
- `accepted/FEATURE-SPEC-008-opening-saving-actions.md`: user-visible behavior and rules for opening and closing the available actions of one existing `Saving` square, including its one-step Back result.
- `accepted/FEATURE-SPEC-009-renaming-a-saving.md`: user-visible behavior and rules for renaming one existing `Saving`, including starting input, pending-save interaction, failure-message lifetime, and complete-list rechecking.
- `accepted/FEATURE-SPEC-010-changing-the-planned-money-amount-of-a-saving.md`: user-visible behavior and rules for changing one `Saving`'s planned money amount, including starting input, accepted decimal forms, pending-save interaction, failure-message lifetime, and complete-list checking after zero removal.
- `accepted/FEATURE-SPEC-011-deleting-a-saving.md`: user-visible behavior and rules for deleting one existing `Saving`, including complete-list rechecking and save-failure message lifetime.
- `accepted/FEATURE-SPEC-012-reordering-saving-squares.md`: user-visible behavior and rules for changing the order of existing `Saving` squares, including exact reorder thresholds, complete-list rechecking, and save-failure message lifetime.
- `accepted/FEATURE-SPEC-013-viewing-savings-coverage.md`: user-visible behavior and rules for viewing how the main money amount covers `Saving` plans.
- `accepted/FEATURE-SPEC-014-recovering-unreadable-saved-data.md`: user-visible behavior and rules for whole-data recovery, `Start again` failure-message timing, and silently removing every broken `Balance Changes` entry when everything else remains usable.
- `accepted/FEATURE-SPEC-015-fixing-a-saving-that-could-not-be-loaded.md`: user-visible behavior and rules for fixing one saved `Saving` that cannot be loaded normally, including empty starting inputs, save-failure message lifetime, an unavailable target, and complete-list revalidation.
- `accepted/FEATURE-SPEC-016-handling-changes-that-cannot-be-saved.md`: shared user-visible behavior when the website cannot keep a requested change.
- `accepted/FEATURE-SPEC-017-discarding-unfinished-actions-after-refresh-or-reopen.md`: shared user-visible behavior for unfinished actions after refresh, close, or later reopen.
- `accepted/FEATURE-SPEC-018-updating-other-open-tabs-and-windows.md`: user-visible behavior for automatically updating other open copies of the website in the same browser.
- `accepted/FEATURE-SPEC-019-keeping-one-action-open-at-a-time.md`: shared user-visible behavior for allowing only one unfinished action to be open at a time.
- `accepted/FEATURE-SPEC-020-opening-main-money-actions.md`: user-visible behavior for opening, choosing, and closing the actions available for the main money amount.
- `accepted/FEATURE-SPEC-021-entering-main-money-amounts.md`: shared user-visible controls and interaction rules for entering a money amount in the `Add`, `Subtract`, and `Modify` flows.
- `accepted/FEATURE-SPEC-022-viewing-the-dashboard.md`: user-visible behavior for initial loading and the normal dashboard, including the main money amount, recent money changes, and access to `Savings`.
- `accepted/TECHNICAL-SPEC-001-main-money-amount-entry-state.md`: internal representation, formatting, state transitions, and browser input handling for `FEATURE-SPEC-021`.
- `accepted/TECHNICAL-SPEC-002-balance-changes-records-and-retention.md`: stored records, exact output formatting, ordering, 720-hour retention, expiration checks, and newest-entry scrolling for `FEATURE-SPEC-004`.
- `accepted/TECHNICAL-SPEC-003-saving-data-and-creation.md`: saved `Saving` records, temporary creation input, normalization, validation, last-place insertion, failure-message state, and creation results for `FEATURE-SPEC-007`.
- `accepted/TECHNICAL-SPEC-004-savings-coverage-calculations.md`: exact ordered coverage calculations, remaining `Savings money amount`, and overall amount needed for `FEATURE-SPEC-013`.
- `accepted/TECHNICAL-SPEC-005-detecting-and-repairing-a-broken-saving.md`: saved-record validation, broken-slot isolation, fix state and validation, safe order allocation, complete-list revalidation, and atomic replacement for `FEATURE-SPEC-015`.
- `accepted/TECHNICAL-SPEC-006-safe-browser-storage-writes-and-rollback.md`: complete-candidate browser-storage writes, failure isolation, commit handling, and rollback by discarding an unsuccessful candidate for `FEATURE-SPEC-016`.
- `accepted/TECHNICAL-SPEC-007-loading-validating-and-recovering-saved-data.md`: version-1 saved data, safe loading and validation, item recovery, silent cleanup, and `Start again` execution for `FEATURE-SPEC-014`.
- `accepted/TECHNICAL-SPEC-008-temporary-action-lifetime-and-discard.md`: in-memory temporary-action boundaries, safe lifecycle discard, pending-request invalidation, and saved-view restoration for `FEATURE-SPEC-017`.
- `accepted/TECHNICAL-SPEC-009-same-browser-save-coordination-and-updates.md`: exclusive save coordination, stale-write prevention, and received-update handling for `FEATURE-SPEC-018`.
- `accepted/TECHNICAL-SPEC-010-one-open-action-coordination.md`: shared action gating, safe same-action transitions, stale-callback protection, and one-interaction protection for `FEATURE-SPEC-019`.
- `accepted/TECHNICAL-SPEC-011-dashboard-loading-and-rendering.md`: neutral initial rendering, root-view selection, complete dashboard derivation, exact money formatting, and safe expiration re-rendering for `FEATURE-SPEC-022`.
- `accepted/TECHNICAL-SPEC-012-opening-and-closing-main-money-actions.md`: current-value action availability, coordinated opening and continuation, and safe click, tap, keyboard, outside-interaction, and scroll closing for `FEATURE-SPEC-020`.
- `accepted/TECHNICAL-SPEC-013-adding-money-safely.md`: exact Add validation and calculation, positive history-record creation, complete-candidate construction, and coordinated save-result handling for `FEATURE-SPEC-001`.
- `accepted/TECHNICAL-SPEC-014-subtracting-money-safely.md`: exact Subtract validation, non-negative calculation, actual-removed history-record creation, complete-candidate construction, and coordinated save-result handling for `FEATURE-SPEC-002`.
- `accepted/TECHNICAL-SPEC-015-modifying-the-money-amount.md`: exact full-amount replacement, complete candidate creation without a new history record, and coordinated save-result handling for `FEATURE-SPEC-003`.
- `accepted/TECHNICAL-SPEC-016-deleting-a-balance-change.md`: hold recognition, coordinated deletion steps, atomic removal of one selected `Balance Changes` record, and safe failure handling for `FEATURE-SPEC-005`.
- `accepted/TECHNICAL-SPEC-017-savings-view-and-browser-navigation.md`: safe `Savings` view derivation, primary-view navigation, temporary action Back steps, and same-document Back and Forward coordination for `FEATURE-SPEC-006`.
- `accepted/TECHNICAL-SPEC-018-opening-and-closing-saving-actions.md`: gesture-safe opening, coordinated continuation, one-step Back behavior, and closing of one normal `Saving` action state for `FEATURE-SPEC-008`.
- `accepted/TECHNICAL-SPEC-019-renaming-a-saving.md`: temporary rename state, name validation, coherent save attempts, complete candidate creation, revalidation, and coordinated result handling for `FEATURE-SPEC-009`.
- `accepted/TECHNICAL-SPEC-020-changing-a-saving-planned-money-amount.md`: temporary amount-change state, exact decimal parsing, coherent replacement or zero-removal candidates, complete-list revalidation, and coordinated save results for `FEATURE-SPEC-010`.
- `accepted/TECHNICAL-SPEC-021-deleting-a-saving.md`: normal and broken target confirmation, atomic deletion, complete-list revalidation, failure-message lifetime, and coordinated result handling for `FEATURE-SPEC-011`.
- `accepted/TECHNICAL-SPEC-022-reordering-saving-squares.md`: hybrid touch-and-mouse gesture recognition without a hold timer, stable fixed-overlay dragging, exact automatic-scrolling geometry, broken-slot-safe order allocation, complete-list revalidation, and coordinated save results for `FEATURE-SPEC-012`.

## Current Draft Specs

There are no current draft specifications.
