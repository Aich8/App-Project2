# FEATURE-SPEC-022: Viewing The Dashboard

Spec ID: `FEATURE-SPEC-022`

Spec type: `FeatureSpec`

Covers: viewing the normal dashboard, its main money amount, recent money changes, and the entry point to `Savings`.

Status: `Accepted`

## User Goal

The user can open one clear dashboard to understand how much cash they currently track, review recent changes, and find `Savings` without needing financial knowledge.

The dashboard is for a manual cash tracker. It does not represent a real bank account or money held by the website.

## When The Dashboard Appears

- When the website first opens, it stays in a neutral loading state until it finishes checking browser storage.
- During that check, it shows neither the normal dashboard nor the saved-data recovery state.
- The default `0.00$` dashboard is not briefly shown while the website is still checking for saved information.
- The normal dashboard is the first view when no saved-data recovery is required.
- When no saved information exists, the dashboard shows the main money amount as `0.00$`.
- Merely showing this default state does not create saved information or a `Balance Changes` entry and does not show a message.
- When usable saved information exists, the dashboard shows the latest successfully kept information.
- Returning from `Savings` shows the dashboard again without changing information.
- The dashboard opens with no unfinished action restored and with the main money actions closed.
- When saved information cannot be loaded as a usable whole, the recovery state from `FEATURE-SPEC-014: Recovering Unreadable Saved Data` appears instead of the normal dashboard.

## Website Identity And Trust

- The dashboard shows the exact website name and short trust label `Manual Cash Tracker`.
- The dashboard presents the website as a tool for manually tracking cash, not as a real bank or financial account.
- It does not suggest that the website holds or moves real money.
- It does not ask the user for personal information, an account, an email address, or a login.
- It does not explain where the user's saved information is stored.
- It does not show a warning about losing information after changing browser or device, using private browsing, clearing browser or site data, or uninstalling a browser.

## Main Money Amount

- The main money amount is the most visually prominent information on the dashboard.
- It is shown with the exact user-facing label `Current Balance`.
- The main money amount is always shown with two digits after the decimal point and a `$` sign at the end.
- Amounts of `1,000.00$` or more use comma separators every three digits before the decimal point.
- Examples include `0.00$`, `5.00$`, `14.50$`, `5,895.50$`, and `999,999.99$`.
- The displayed main money amount is never below `0.00$` or above `999,999.99$`.
- The complete amount remains readable without horizontal page overflow, overlapping other content, or hiding required actions.
- Selecting the main money amount uses `FEATURE-SPEC-020: Opening Main Money Actions`.

## Updating The Displayed Amount

- A successful `Add`, `Subtract`, or `Modify` shows the resulting main money amount on the dashboard.
- A successful saved-data recovery shows `0.00$`.
- A successful saved change received from another open tab or window shows the latest main money amount as defined by `FEATURE-SPEC-018: Updating Other Open Tabs And Windows`.
- A canceled, invalid, no-action, unfinished, or failed change does not change the displayed main money amount from the latest successfully kept value.
- Creating, renaming, changing, deleting, fixing, or reordering a `Saving` does not change the main money amount.
- Deleting or expiring a `Balance Changes` entry does not change the main money amount.

## Dashboard Content And Access

- The dashboard includes the main money amount, a `Balance Changes` section, and the exact action `Savings`.
- The content and ordering of `Balance Changes` entries follow `FEATURE-SPEC-004: Viewing Balance Changes`.
- When no `Balance Changes` entries exist, the section shows no rows, sentence, placeholder, icon, or other empty-state content.
- Every entry that is still within its visible period remains reachable.
- The `Savings` action remains reachable whether `Balance Changes` is empty or contains entries.
- Choosing `Savings` follows `FEATURE-SPEC-006: Viewing Savings`.

## Mobile And Desktop Use

- The dashboard remains understandable and usable on phone-sized and desktop screens.
- The main money amount remains easy to find at both sizes.
- The full valid money amount, `Balance Changes`, and `Savings` action remain usable without horizontal page overflow or overlapping content.
- Dashboard controls support mouse clicks and finger taps.
- Exact component shapes, positions, scrolling implementation, breakpoints, dimensions, spacing, colors, typography, and decorative styling remain design decisions.

## Boundaries

- This spec does not redefine the contents, retention, or deletion behavior of `Balance Changes`.
- It does not redefine the contents or behavior of `Savings` or individual `Saving` squares.
- It does not define the available main money actions or the shared money amount entry controls.
- It does not define saved-data recovery behavior beyond when that accepted recovery view replaces the dashboard.
- It does not require a particular shape, size, fixed placement, or separate scrolling area for dashboard information or actions.
- It does not define storage, data structures, algorithms, source files, or implementation technology.

## Acceptance Expectations

- While the website is checking browser storage after opening, it shows neither the normal dashboard nor the saved-data recovery state and does not briefly show the default `0.00$` dashboard.
- Opening the website normally shows a clear dashboard with `Manual Cash Tracker` and the main money amount labeled `Current Balance`.
- With no saved information, the dashboard shows `0.00$` without creating saved information or history.
- With usable saved information, the dashboard shows the latest successfully kept main money amount.
- The main money amount uses two decimal digits, required comma separators, and a trailing `$` sign and remains fully readable through `999,999.99$`.
- The dashboard includes `Balance Changes` and keeps all current entries reachable.
- Empty `Balance Changes` shows no empty-state content.
- The `Savings` action remains reachable whether `Balance Changes` is empty or contains entries.
- The dashboard remains usable on mobile and desktop and clearly presents the product as a manual cash tracker rather than a real bank.
