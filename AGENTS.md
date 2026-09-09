# Project Instructions

## Project

This project is a Cash Money Organizer website.

It is a personal cash tracking tool. It can look bank-like, but it must not pretend to be a real bank.

The project folder may be named `App Project2`, but the product should be treated as a website, not as a native mobile app or desktop app.

## Specification System

The project uses exactly three specification types:

- `GlobalSpec`: a short, project-wide explanation of the product, its audience, its main directions, and its boundaries.
- `FeatureSpec`: a detailed explanation of exactly one user-visible feature and what that feature must do.
- `TechnicalSpec`: one focused part of the technical side of a matching `FeatureSpec`; it explains how to implement part or all of that feature's required result.

Every specification filename and H1 title must identify both the specification type and what it covers.

Use these numbered naming patterns:

- Global filename: `GLOBAL-SPEC-<NNN>-<project-scope>.md`.
- Feature filename: `FEATURE-SPEC-<NNN>-<feature-scope>.md`.
- Technical filename: `TECHNICAL-SPEC-<NNN>-<technical-scope>.md`.
- H1 title: `# GLOBAL-SPEC-<NNN>: <Project Scope>`, `# FEATURE-SPEC-<NNN>: <Feature Scope>`, or `# TECHNICAL-SPEC-<NNN>: <Technical Scope>`.

Use exactly three digits, beginning with `001`. Use uppercase for the type prefix and lowercase kebab-case for the filename scope, as in `FEATURE-SPEC-001-adding-money.md`.

Numbers are permanent and unique within each spec type. Never renumber an existing spec, fill an old gap, or reuse a retired number. `GlobalSpec`, `FeatureSpec`, and `TechnicalSpec` each have their own independent number sequence.

A feature may have one or many technical specs. Every `TechnicalSpec` must explicitly link to exactly one `FeatureSpec`, but it uses its own technical-spec number and a scope that names its technical responsibility. For example, `TECHNICAL-SPEC-001-adding-money-storage.md` and `TECHNICAL-SPEC-002-adding-money-input-handling.md` may both implement `FEATURE-SPEC-001-adding-money.md`.

Number `000` is reserved only for temporary legacy baseline specs created while migrating the old combined documents. Never assign `000` to a new spec.

Do not use vague or unnumbered specification names such as `global.md`, `functional1.md`, `technical1.md`, `feature-spec-of-feature.md`, or `new-spec.md`.

Keep each type within its boundary:

- Keep `GlobalSpec` brief and standalone. It may name the product's main directions, but it must not explain any one feature in detail and must not contain technical information.
- Put the user-visible behavior, rules, edge cases, failure behavior, and acceptance expectations for exactly one feature in each `FeatureSpec`. A `FeatureSpec` must not contain architecture, storage formats, data schemas, algorithms, file paths, framework choices, or other technical implementation information.
- Put architecture, data structures, storage, algorithms, interfaces, implementation constraints, and technical verification for one feature in one or more focused `TechnicalSpec` files.
- Every `TechnicalSpec` must identify and link to exactly one matching `FeatureSpec`. Multiple technical specs may link to the same feature spec. Each explains its named technical area and must not add, remove, or redefine user-visible behavior.
- Shared user-visible behavior must have its own numbered `FeatureSpec` instead of being hidden inside a technical spec or duplicated across unrelated feature specs.

### Specification Lifecycle

- Every new specification and every proposed material revision to an accepted specification must start in `docs/specs/drafts/`.
- Never create a new specification directly in `docs/specs/accepted/`.
- A draft is not an accepted source of truth and must not override an accepted specification.
- Only the user can approve a draft. Silence, continued discussion, a request to review, or the existence of a draft does not count as approval.
- Move a draft into `docs/specs/accepted/` only after the user explicitly confirms that the specification is approved and may be accepted.
- If material content changes after approval, return it to draft review and request approval again.
- After acceptance, keep only the accepted copy; Git history preserves older accepted versions.
- Information in `docs/raw-data/` may be used to prepare a draft, but it is not a specification and is never a source of truth by itself.

Full specification governance and the accepted-spec index are in `docs/specs/README.md`.

## Source Of Truth

Before changing product behavior, read:

- `docs/specs/README.md`.
- `docs/specs/accepted/GLOBAL-SPEC-002-manual-cash-tracker.md`.
- Every accepted `FeatureSpec` relevant to the requested behavior.
- Every accepted `TechnicalSpec` that implements those feature specs.
- `docs/plans/active/active.md`.

Only files in `docs/specs/accepted/` are accepted specifications. Draft specs and raw data are context only.

If behavior changes, first follow the draft and approval lifecycle, then update the accepted specs and active plan so they stay consistent before implementation.

## Project Structure

- `website/`: Next.js website application.
- `website/app/`: Next.js App Router pages, layout, and global CSS.
- `website/public/`: static assets served by the website.
- `docs/specs/`: specification governance and lifecycle folders.
- `docs/specs/accepted/`: approved `GlobalSpec`, `FeatureSpec`, and `TechnicalSpec` files; these are the specification source of truth.
- `docs/specs/drafts/`: new or revised specifications waiting for explicit user approval.
- `docs/raw-data/`: notes, references, exports, legacy material, and temporary project information that may later become a draft specification.
- `docs/plans/active/`: active implementation plans.
- `.vscode/`: workspace editor settings.

Do not recreate the old folders `WebSite Project/`, `BUSINESS-Specs.md/`, root `Docs/`, or root `AGENTS.md/`. The project now uses `website/`, `docs/`, and the root `AGENTS.md` file.

## GitFlow

- Use a single branch only: `main`.
- Do not create feature branches, release branches, or temporary work branches unless the user explicitly changes this rule.
- Before making Git changes, confirm the working branch is `main`.
- When the user asks for a commit, stage the relevant changes, create the commit on `main`, and push `main` to GitHub.
- Do not use `Accept all incoming changes` as a conflict strategy because it can overwrite local work.
- If Git reports conflicts, stop and resolve the files by preserving the user's intended local work and the needed incoming work, then continue only after the conflicts are actually resolved.
- Keep the Git process simple: edit files, test when relevant, commit on `main`, push `main`.

## Product Rules

- Do not use real banking language like deposit, withdrawal, bank transfer, real account number, or card-related money wording.
- Use `money amount` in specs, planning, explanations, and code comments when explaining the tracked value.
- Use `Current Balance` only as the exact user-facing website label for the money amount.
- Do not use `Current Balance` as the general name of the value outside website text.
- Example: write "Savings does not lower the money amount." Do not write "Savings does not lower `Current Balance`" unless explaining the label shown on the website.
- Use simple wording like add, subtract, modify, `Savings`, `Saving`, and squares.
- The first version saves data only in browser storage.
- Do not add email accounts, login, cloud sync, or server database unless explicitly requested.
- Do not assume users update money on a regular schedule; the website should work for rare use and very frequent use.
- `Add` and `Subtract` should show separate signed history entries.
- `Add` and `Subtract` history entries should stay visible for one month.
- Subtracting more than the current money amount should set the money amount to `$0`, not a negative amount.
- Subtracting when the current money amount is already `$0` should not create a history entry.
- `Modify` should silently correct the current money amount without creating history, activity, or notification entries.
- `Savings` is the separate planning section of the website.
- A `Saving` is one user-named square inside the `Savings` section.
- Do not add a separate goals feature unless the user explicitly asks for it later.
- `Saving` squares show money set aside and reduce only the money amount shown inside `Savings`, not the main money amount.

## Work Style

- Keep changes simple and easy to understand.
- Prefer updating the existing files under `docs/` instead of creating unnecessary new files.
- Keep the website mobile-friendly.
- Explain important changes in easy words.
