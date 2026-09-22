| TechnicalSpec | Implements | Technical subject |
|---|---|---|
| 001 | FeatureSpec 021 | Money amount representation, formatting, and input state |
| 002 | FeatureSpec 004 | `Balance Changes` records, ordering, display, and expiration |
| 003 | FeatureSpec 007 | `Saving` data and creation |
| 004 | FeatureSpec 013 | Savings coverage calculations |
| 005 | FeatureSpec 015 | Detecting and repairing a broken `Saving` |
| 006 | FeatureSpec 016 | Safe browser-storage writes and rollback after failure |
| 007 | FeatureSpec 014 | Loading, validating, and recovering saved data |
| 008 | FeatureSpec 017 | Keeping temporary actions out of saved data |
| 009 | FeatureSpec 018 | Updating other tabs and windows |
| 010 | FeatureSpec 019 | Allowing only one unfinished action |
| 011 | FeatureSpec 022 | Dashboard loading and rendering |
| 012 | FeatureSpec 020 | Opening and closing main money actions |
| 013 | FeatureSpec 001 | Adding money safely |
| 014 | FeatureSpec 002 | Subtracting money safely |
| 015 | FeatureSpec 003 | Modifying the money amount |
| 016 | FeatureSpec 005 | Deleting a `Balance Changes` entry |
| 017 | FeatureSpec 006 | Opening, showing, and leaving `Savings` |
| 018 | FeatureSpec 008 | Opening and closing `Saving` actions |
| 019 | FeatureSpec 009 | Renaming a `Saving` |
| 020 | FeatureSpec 010 | Changing a `Saving` planned money amount |
| 021 | FeatureSpec 011 | Deleting a `Saving` |
| 022 | FeatureSpec 012 | Reordering `Saving` squares |

Each one starts in `docs/specs/drafts/`, links to exactly one FeatureSpec, and remains a draft until you approve it.

TechnicalSpecs 001 through 007 are accepted. TechnicalSpecs 008 and 009 are in draft review.

## What still needs clarification?

A few user-visible details are not fully settled:

1. **Other save-failure message lifetimes**

   The lifetime is settled for main money amount entry, `Saving` creation, broken-`Saving` fix, and `Start again` recovery flows. The remaining save-failure flows can be settled in their matching FeatureSpecs before their TechnicalSpecs are accepted.

2. **Browser and accessibility support**

   Exact supported browsers, screen-reader behavior, contrast, and reduced motion are not defined.
   Recommended: support current major mobile and desktop browsers and use normal accessible HTML controls.

Shapes, colors, spacing, exact positions, responsive breakpoints, and component appearance are intentionally open design choices. They do not need to be added to the FeatureSpecs.

## From now until completion

1. Review TechnicalSpec 008 with you.
2. Accept TechnicalSpec 008 only after your approval.
3. Review TechnicalSpec 009 with you.
4. Accept TechnicalSpec 009 only after your approval.
5. Repeat that process through TechnicalSpec 022.
6. Update the active implementation plan using the accepted TechnicalSpecs.
7. Decide the visible design while working on the dashboard and Savings technical work.
8. Build the website code in `website/`.
9. Test every feature against its FeatureSpec and TechnicalSpec.
10. Check mobile, desktop, browser storage, multiple tabs, failures, and recovery.
11. Complete the final review and move the active plan to done.

The immediate next piece of work is reviewing **TechnicalSpec 008: Temporary Action Lifetime And Discard**.
