# Project Instructions

## Project

This project is a Cash Money Organizer website.

It is a personal cash tracking tool. It can look bank-like, but it must not pretend to be a real bank.

The project folder may be named `App Project2`, but the product should be treated as a website, not as a native mobile app or desktop app.

## Source Of Truth

Before changing features, read these files:

- `BUSINESS-Specs.md/cash-money-organizer-website.md`
- `Docs/Specs/global-Spec.md`
- `Docs/Specs/Functional/functional1.md`
- `Docs/Specs/Technical/technical1.md`
- `Docs/Plans/Active/Active.md`

If behavior changes, update the specs and active plan so they stay consistent.

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
- Use `money amount` in specs and planning when explaining the tracked value.
- Use `Current Balance` only as the user-facing website label for the main money amount.
- Use simple wording like add, subtract, modify, savings, and folders.
- The first version saves data only in browser storage.
- Do not add email accounts, login, cloud sync, or server database unless explicitly requested.
- Do not assume users update money on a regular schedule; the website should work for rare use and very frequent use.
- `Add` and `Subtract` should show separate signed history entries.
- `Add` and `Subtract` history entries should stay visible for one month.
- Subtracting more than the current money amount should set the money amount to `$0`, not a negative amount.
- Subtracting when the current money amount is already `$0` should not create a history entry.
- `Modify` should silently correct the current money amount without creating history, activity, or notification entries.
- `Savings` is one planning section with user-named folders or squares.
- Do not add a separate goals feature unless the user explicitly asks for it later.
- Savings folders or squares show money set aside and reduce only the savings available amount, not the main money amount.

## Work Style

- Keep changes simple and easy to understand.
- Prefer updating existing specs instead of creating unnecessary new files.
- Keep the website mobile-friendly.
- Explain important changes in easy words.
