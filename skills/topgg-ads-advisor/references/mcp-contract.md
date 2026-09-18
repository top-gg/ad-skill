# Integration and maintenance notes

The contract lives in the connected server's `tools/list` definitions and `topgg://guides/create-campaign`. Read them before edits; read the stats response's notes before analysis. Do not add copied schemas, enum catalogs, unit tables, or lists of unavailable tools here. Clients expose different tool prefixes and resource interfaces; use the connected Top.gg server's equivalent operation.

## Remaining delivery caveat

The service maintainer clarified on 2026-09-18 that front-page-style delivery is available only when all four surface filters are unset. Setting any surface filter narrows delivery to that surface. The guide's example combining front-page placement with a music category should therefore not be treated as a promise of both surfaces. Consult the current guide/tool semantics and full campaign state before offering such a combination. This is distinct from the documented meanings of omitted, false and empty-list values; do not normalize those values into each other. Remove this caveat once the server documents the interaction unambiguously.

## Behavioral maintenance checks

Evaluate these with fictional inputs and simulated responses, without live mutations:

- Bid-only edit: emit only the exact campaign ID and approved bid. No unrelated targeting confirmation or write; use the returned campaign state for verification.
- Headline edit with a shared ad: account for all affected campaigns and keep the approved scope. No bid, wallet or targeting questions just to change copy.
- Surface choice: retain the live schema's distinct states; explain effective placement scope rather than combining labels as if they always form a union.
- Extend an ended campaign: distinguish an active-but-ended campaign from a paused one. The proposed outcome must match the user's spending intent; do not add an automatic resume step.
- Sparse daily rows: compare consecutive calendar windows, applying the server's omitted-day rule only within the returned coverage. Never equate row count with elapsed days.
- Custom-ad conversions equal clicks: use the documented event meaning rather than diagnosing a tracking fault or calling them acquired users.
- Placement CPA request: explain which evidence is unavailable; do not convert source CTR into a cost metric.
- Lifetime budget appears in a read: do not invent a write parameter or promise a new enforceable cap without write-tool support.
- Mutation returns structured state: report it without a redundant verification fetch; if it lacks the field needed for a separate content inspection, state that limitation.
- Skill maintenance request: no live campaign changes. Refresh this guidance against live tools and remove assumptions superseded by the server.

Also validate skill frontmatter, metadata, relative links and installation documentation. These checks describe acceptance cases, not a claim of automated or independent behavioral evaluation.
