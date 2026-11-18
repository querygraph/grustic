# Proposal: Table-backed graph mapping

## Why

Most users arrive with vertex and edge tables from GraphFrames, Spark or Parquet. A PGQ-style mapping from tables to schema groups is the canonical way to define a graph over tables, and the route both the reference backend and Nutmeg use.

## What Changes

- A declarative mapping: a vertex table becomes a group with key columns and labels; an edge table becomes a group with source and target key columns that reference vertex groups.
- Free checks of the mapping against the schema: types and key presence. Data validation stays a separate, explicit operation.
- A mapping yields a graph source whose capabilities are scan-based unless the provider declares more.

## Capabilities

### New Capabilities

- `table-graph-mapping`: Defining graphs over vertex and edge tables.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-schema`.
- **Human-scoped files:** `grustic-schema/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `034-graph-source-capabilities`.
