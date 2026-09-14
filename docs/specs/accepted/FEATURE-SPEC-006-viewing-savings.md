# FEATURE-SPEC-006: Viewing Savings

Spec ID: `FEATURE-SPEC-006`

Spec type: `FeatureSpec`

Covers: opening the `Savings` planning section, reviewing its available information, and returning to the dashboard.

Status: `Accepted`

## User Goal

The user can open `Savings` to review how their cash is being planned.

`Savings` is for planning only. Viewing it does not change the main money amount.

## Opening Savings

- The user opens the section by choosing the exact action `Savings`.
- `Savings` can be opened when there are no `Saving` squares.
- Opening `Savings` does not change the main money amount.
- Opening `Savings` does not change any `Saving` square.
- Opening `Savings` creates no `Balance Changes` entry.
- Opening `Savings` shows no message.

## Available Information

- The section uses the exact name `Savings`.
- The section shows a separate money amount with the exact label `Savings money amount`.
- The section makes every existing `Saving` square available to review.
- The `Savings money amount` and `Saving` squares are planning information.
- Viewing this information does not change the main money amount.

## Returning To The Dashboard

- While the user is only viewing `Savings`, the section's Back action returns the user to the dashboard.
- While the user is only viewing `Savings`, Browser Back also returns the user to the dashboard.
- Returning to the dashboard does not change the main money amount.
- Returning to the dashboard does not change any `Saving` square.
- Returning to the dashboard creates no `Balance Changes` entry.
- Returning to the dashboard shows no message.

## Acceptance Expectations

- Choosing `Savings` opens the `Savings` planning section.
- The section opens even when no `Saving` squares exist.
- The user can review `Savings money amount` and every existing `Saving` square.
- While the user is only viewing `Savings`, the section's Back action and Browser Back return the user to the dashboard.
- Opening, viewing, or leaving `Savings` does not change any `Saving` square or the main money amount.
- Opening, viewing, or leaving `Savings` creates no `Balance Changes` entry.
