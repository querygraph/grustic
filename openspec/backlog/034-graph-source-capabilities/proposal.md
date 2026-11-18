# Proposal: Graph source capabilities

## Why

The planner must know what a graph source can do without touching data. This is the graph counterpart of Calcite adapters and DataFusion's TableProvider. Without declared capabilities the planner must assume full scans or hard-code one storage.

## What Changes

- A graph source contract: the schema groups it serves and their identity namespaces.
- Declared capabilities: label scans, neighbor lookup by identity, available adjacency layouts with orientation and sort keys, indexes and partitioning.
- Optional statistics with provenance and confidence; unknown is distinct from zero.
- Capabilities are data that the planner reads. Nothing executes in this change.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `graph-source`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-schema`.
- **Human-scoped files:** `grustic-schema/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `033-catalog-and-snapshots`.
