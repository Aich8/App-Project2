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
- `accepted/FEATURE-SPEC-004-viewing-balance-changes.md`: user-visible behavior and rules for reviewing successful `Add` and `Subtract` actions from the last 30 days.
- `accepted/FEATURE-SPEC-005-deleting-a-balance-change.md`: user-visible behavior and rules for deleting one `Balance Changes` entry.
- `accepted/FEATURE-SPEC-006-viewing-savings.md`: user-visible behavior and rules for opening, viewing, and leaving the `Savings` planning section.
- `accepted/FEATURE-SPEC-007-creating-a-saving.md`: user-visible behavior and rules for creating one new `Saving` square.
- `accepted/FEATURE-SPEC-008-opening-saving-actions.md`: user-visible behavior and rules for opening the available actions of one existing `Saving` square.
- `accepted/FEATURE-SPEC-009-renaming-a-saving.md`: user-visible behavior and rules for renaming one existing `Saving`.
- `accepted/FEATURE-SPEC-010-changing-the-planned-money-amount-of-a-saving.md`: user-visible behavior and rules for changing the planned money amount of one existing `Saving`.
- `accepted/FEATURE-SPEC-011-deleting-a-saving.md`: user-visible behavior and rules for deleting one existing `Saving`.
- `accepted/FEATURE-SPEC-012-reordering-saving-squares.md`: user-visible behavior and rules for changing the order of existing `Saving` squares.
- `accepted/FEATURE-SPEC-013-viewing-savings-coverage.md`: user-visible behavior and rules for viewing how the main money amount covers `Saving` plans.
- `accepted/FEATURE-SPEC-014-recovering-unreadable-saved-data.md`: user-visible behavior and rules for recovering when saved website data cannot be loaded as a whole.
- `accepted/FEATURE-SPEC-015-fixing-a-saving-that-could-not-be-loaded.md`: user-visible behavior and rules for fixing one saved `Saving` that cannot be loaded normally.
- `accepted/FEATURE-SPEC-016-handling-changes-that-cannot-be-saved.md`: shared user-visible behavior when the website cannot keep a requested change.
- `accepted/FEATURE-SPEC-017-discarding-unfinished-actions-after-refresh-or-reopen.md`: shared user-visible behavior for unfinished actions after refresh, close, or later reopen.
- `accepted/FEATURE-SPEC-018-updating-other-open-tabs-and-windows.md`: user-visible behavior for automatically updating other open copies of the website in the same browser.
- `accepted/FEATURE-SPEC-019-keeping-one-action-open-at-a-time.md`: shared user-visible behavior for allowing only one unfinished action to be open at a time.
- `accepted/FEATURE-SPEC-020-opening-main-money-actions.md`: user-visible behavior for opening, choosing, and closing the actions available for the main money amount.

No TechnicalSpec is currently accepted.

## Current Draft Specs

There are currently no numbered draft specifications.
