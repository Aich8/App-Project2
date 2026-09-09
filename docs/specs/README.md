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

No TechnicalSpec is currently accepted.

## Current Draft Specs

There are currently no numbered draft specifications.
