# Project Instructions

## Project

This project is a Cash Money Organizer website.

It is a personal cash tracking tool. It can look bank-like, but it must not pretend to be a real bank.

The project folder may be named `App Project2`, but the product should be treated as a website, not as a native mobile app or desktop app.

## Source Of Truth

Before changing features, read these files:

- `docs/specs/business/cash-money-organizer-website.md`
- `docs/specs/global.md`
- `docs/specs/functional/functional1.md`
- `docs/specs/technical/technical1.md`
- `docs/plans/active/active.md`

If behavior changes, update the specs and active plan so they stay consistent.

## Project Structure

- `website/`: Next.js website application.
- `website/app/`: Next.js App Router pages, layout, and global CSS.
- `website/public/`: static assets served by the website.
- `docs/specs/`: product, functional, business, and technical specifications.
- `docs/specs/business/`: business-level product requirements.
- `docs/specs/functional/`: user flows and feature behavior.
- `docs/specs/technical/`: implementation and storage rules.
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
