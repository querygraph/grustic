# Proposal: Arrow type mapping

## Why

The reference backend and Nutmeg both exchange data as Arrow. Defining the mapping once avoids two diverging conversions and keeps Arrow out of the core crates.

## What Changes

- `grustic-arrow` crate: mapping between logical types and Arrow fields, including nullability, nested types, maps and metadata for graph values.
- Identity encoding: the Int64 fast path, other identity domains and lossless round trip.
- Typed refusal for logical types without a mapping.

## Capabilities

### New Capabilities

- `arrow-mapping`: Mapping logical types and identities to Arrow.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-arrow` (new, adapter).
- **Human-scoped files:** new crate manifest, `src/lib.rs`, `src/api/` and `public-api.txt`; `boundaries.toml` entry as an adapter crate.
- **Unsafe code:** None expected.
- **Depends on:** `030-nested-and-graph-value-types`, `031-entity-identity`.
