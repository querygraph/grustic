# Proposal: Optional match and relationship uniqueness

## Why

Cypher's relationship uniqueness within a `MATCH` and the null semantics of `OPTIONAL MATCH` change answers. Wave 3 had tested behavior for both that should be preserved.

## What Changes

- Relationship uniqueness per `MATCH` clause as explicit plan semantics.
- `OPTIONAL MATCH` as optional expand or apply, with correct null propagation.
- Aliased entities across clauses keep their provenance.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `query-resolution`: Adds uniqueness and optional-match semantics.

## Impact

- **Crates:** `grustic-resolver`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `054-dogfood-plan-review`, `045-resolve-basic-patterns`.
- **Ported from:** Grust v2 Wave 3 uniqueness and optional-match behavior. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, `wave-3/sketch` under `docs/reviews/grust-v2/`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
