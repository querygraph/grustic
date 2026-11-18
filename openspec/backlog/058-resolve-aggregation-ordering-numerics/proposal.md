# Proposal: Aggregation, ordering and numeric semantics

## Why

Grouping, ordering by non-returned variables, `DISTINCT` and numeric overflow or division are where Wave 3 found subtle dialect differences. They need explicit, tested rules.

## What Changes

- Implicit grouping keys, `DISTINCT` aggregates and aggregation in `WITH` and `RETURN`.
- `ORDER BY` on variables not returned (hidden ordering columns); `SKIP` and `LIMIT` with parameters.
- Numeric semantics: integer overflow, division by zero and mixed integer and float comparison, as errors or values per dialect profile.
- A dialect profile as session data, with openCypher as the default.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `query-resolution`: Adds aggregation, ordering, numeric and dialect-profile semantics.

## Impact

- **Crates:** `grustic-resolver`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `055-resolve-optional-match-and-uniqueness`, `047-session-state-and-planning-entry`.
- **Ported from:** Grust v2 Wave 3 ordering and numeric controls. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, `wave-3/sketch` under `docs/reviews/grust-v2/`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
