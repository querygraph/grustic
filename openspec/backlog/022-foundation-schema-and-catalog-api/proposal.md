# Proposal: Foundation: schema, catalog and graph sources

## Why

The schema, catalog and graph source contracts are what storage adapters implement. They are the graph counterpart of Calcite adapters and DataFusion's TableProvider, and must be stable before adapters are written.

## What Changes

- Schema group, label and property key types.
- Catalog provider trait, entry kinds with extension-defined kinds, and catalog snapshot identity.
- Graph source trait, capability declarations and statistics types with provenance.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `lpg-schema`: Schema groups, labels, inheritance, property keys and free schema checks.
- `catalog`: Catalog hierarchy, providers, extension entry kinds and snapshots.
- `graph-source`: What a graph source declares about its data access capabilities and statistics.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-schema` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `021-foundation-types-and-identity-api`.
