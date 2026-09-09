# Draft Specifications

This folder holds new specifications and material revisions that are waiting for explicit user approval.

This README is workflow documentation, not a specification.

Every draft spec must follow the numbered type-and-scope naming rules in `../README.md`. Its H1 and metadata must identify its spec ID, type, one specific area of coverage, and `Draft` status.

A draft is not a source of truth and cannot override an accepted spec. Move it to `../accepted/` only after the user explicitly confirms that it is approved and may be accepted.

Draft `FeatureSpec` files each cover exactly one user-visible feature. Each draft `TechnicalSpec` links to exactly one feature spec, uses its own technical-spec number, and covers one focused technical part of that feature. Multiple technical specs may link to the same feature spec.
