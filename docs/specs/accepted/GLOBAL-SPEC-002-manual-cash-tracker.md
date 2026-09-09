# GLOBAL-SPEC-002: Manual Cash Tracker

Spec ID: `GLOBAL-SPEC-002`

Spec type: `GlobalSpec`

Covers: the purpose, audience, main product directions, and boundaries of the Manual Cash Tracker website.

Status: `Accepted`

## Purpose

Manual Cash Tracker is a personal website for manually tracking and organizing cash.

It helps people understand how much cash they currently have, remember recent changes, and plan how that money may be used.

## Audience

The website is for people who manage their own cash and want a simple, clear view of it without needing financial knowledge.

It should remain useful whether a person returns rarely or uses it many times in one day, on mobile or desktop.

## Main Product Directions

- Keep the main money amount clear and easy to understand.
- Support deliberate manual changes and a useful view of recent changes.
- Provide `Savings` as a separate way to plan cash without moving it.
- Keep saved results dependable when the user returns to the website.
- Make each interaction calm, practical, and focused on one clear task.

## Global Boundaries

- The product is a website, not a native application.
- It is a manual cash organizer, not a real bank or financial account.
- It does not hold or move real money.
- It does not require an account or collect personal information in the first version.
- `Savings` is planning only and does not change the main money amount.
- Detailed behavior belongs in individual `FeatureSpec` files.
- Implementation decisions belong in matching `TechnicalSpec` files.
