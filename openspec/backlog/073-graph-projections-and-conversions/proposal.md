# Proposal: Graph projections and conversions

## Why

CSR projections pay off when the graph fits in memory and repeated calls amortize construction. Their cost must be visible and bounded rather than assumed.

## What Changes

- A projection contract keyed by snapshot, selected groups, orientation, features, sort key and implementation version.
- Reference conversions: build CSR from scans, and re-sort adjacency by an edge property. Each is charged for memory, including peak use during construction.
- A bounded projection cache keyed by that identity.
- Explicit mapping between dense positions and identities. 32-bit positions only when bounds permit; otherwise wider positions or a refusal.
- Design goal G2 as a test-only extension: temporal path syntax, its resolver, a logical node, a time-window rule, and an implementation that requires adjacency sorted by an edge timestamp. The planner inserts the re-sort conversion only when no available projection already provides that order.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `graph-projections`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-plan`, `grustic-reference`.
- **Human-scoped files:** `grustic-plan/src/api/`, `public-api.txt`.
- **Unsafe code:** None planned. Unchecked indexing for performance would need per-site approval with measurements.
- **Depends on:** `028-foundation-kernel-and-projection-api`, `072-physical-properties-and-enforcement`, `063-reference-graph-traversal`, `060-resolver-extensions`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
