# Proposal: Reference backend: joins, apply and aggregation

## Why

arrow-rs provides kernels but no joins or aggregation, and almost every LDBC query needs them.

## What Changes

- Hash join: inner, left outer, semi and anti.
- Correlated apply by per-row evaluation.
- Grouping aggregation including `DISTINCT` aggregates, with Cypher null grouping semantics.
- Design goal G3 as a test-only extension: a relational-table catalog entry kind whose rows join with graph results through the core join operators, without converting the table into a graph.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `reference-execution`: Adds joins, apply and aggregation.

## Impact

- **Crates:** `grustic-reference`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `061-reference-relational-execution`, `057-resolve-subqueries-and-correlation`, `058-resolve-aggregation-ordering-numerics`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
