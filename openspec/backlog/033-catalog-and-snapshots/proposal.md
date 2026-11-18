# Proposal: Catalog and snapshots

## Why

Plans must be resolved against a fixed view of schema and data. A catalog hierarchy with extension-defined entry kinds lets applications combine graphs with other sources, such as relational tables or documents, without core changes.

## What Changes

- A catalog, schema and entry naming hierarchy with name resolution rules.
- A catalog provider contract. Entry kinds are graphs plus extension-defined kinds registered by name.
- A catalog snapshot fixes schema revision, data revision and identity mapping. Plans hold immutable snapshot identifiers, never live handles.
- Snapshot collection runs before resolution: referenced names are gathered, possibly asynchronously, then resolution is synchronous against the snapshot.
- Typed refusals for unknown, ambiguous or incompatible entries.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `catalog`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-schema`.
- **Human-scoped files:** `grustic-schema/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `032-lpg-schema-groups`.
