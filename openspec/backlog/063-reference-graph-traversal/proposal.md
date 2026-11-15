# Proposal: Reference backend: graph traversal

## Why

The reference backend must execute graph operations from the resolved plan, starting with the simplest faithful implementation over scans and hash lookups.

## What Changes

- Expand over edge scans with hash lookup of endpoints, relationship uniqueness and optional expand.
- Variable-length path evaluation with path modes and bounds, and shortest-path selections.
- Path values materialized according to the Arrow mapping.
- Differential tests against the minimal second backend on the vertical-slice queries.

## Capabilities

### New Capabilities

- `reference-graph-operators`: How the reference backend executes graph operations.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-reference`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `062-reference-joins-and-aggregation`, `056-resolve-variable-length-paths`, `050-minimal-second-backend`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
