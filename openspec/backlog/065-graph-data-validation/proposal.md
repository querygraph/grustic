# Proposal: Explicit graph data validation

## Why

Inputs are assumed to satisfy their declared graph contract, and only free schema checks run by default. Users still need a way to check their data when they choose to, without hidden validation jobs inside queries or algorithms.

## What Changes

- An explicit validation operation over a table-backed graph: edge endpoints exist, declared keys are unique and non-null, and identity values belong to their declared namespace and domain.
- A typed report listing each violated constraint with counts and sample identities.
- Validation never runs as part of planning, queries or algorithm calls.
- A reference backend implementation; other backends may implement the same request.

## Capabilities

### New Capabilities

- `graph-data-validation`: Explicit, opt-in validation of graph data against its declared contract.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-schema` (request and report types), `grustic-reference`.
- **Human-scoped files:** `grustic-schema/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `062-reference-joins-and-aggregation`, `035-table-backed-graph-mapping`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
