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
| 017 | FeatureSpec 006 | `Savings` view and browser navigation |
| 018 | FeatureSpec 008 | Opening and closing `Saving` actions |
| 019 | FeatureSpec 009 | Renaming a `Saving` |
| 020 | FeatureSpec 010 | Changing a `Saving` planned money amount |
| 021 | FeatureSpec 011 | Deleting a `Saving` |
| 022 | FeatureSpec 012 | Reordering `Saving` squares |

Each new TechnicalSpec starts in `docs/specs/drafts/`, links to exactly one FeatureSpec, and remains a draft until you approve it.

TechnicalSpecs 001 through 022 are accepted. There are no TechnicalSpecs in draft review.

## Implementation Verification Still Required

The specifications are accepted. These implementation verification items remain before the website can be treated as complete.

1. **Touch reorder browser verification**

   Accepted TechnicalSpec 022 uses Touch Events with a non-passive active `touchmove` listener for touch, Pointer Events for mouse, native pre-hold scrolling, explicit successful touch reservation, and clean cancellation when prevention or retained gesture resources fail. Verify pre-hold scrolling, sub-threshold jitter followed by post-hold reorder, actual scroll after sub-threshold movement, every post-hold prevention check, stable event ownership, and visual-viewport interruption on real devices in current mobile Safari, Chrome for Android, and Firefox for Android before implementation is treated as complete.

2. **Browser and accessibility support**

   Exact supported browsers, screen-reader behavior, contrast, and reduced motion are not defined.
   Recommended: support current major mobile and desktop browsers and use normal accessible HTML controls.

Shapes, colors, spacing, exact positions, responsive breakpoints, and component appearance are intentionally open design choices. They do not need to be added to the FeatureSpecs.

## From now until completion

1. Decide the visible design while working on the dashboard and Savings technical work.
2. Build the website code in `website/`.
3. Test every feature against its accepted FeatureSpec and TechnicalSpec.
4. Check mobile, desktop, browser storage, multiple tabs, failures, and recovery.
5. Complete the final review and move the active plan to done.

TechnicalSpec review is complete through TechnicalSpec 022.
