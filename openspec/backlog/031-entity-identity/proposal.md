# Proposal: Entity identity

## Why

Graph sources identify entities in many ways, while kernels use dense positions. The assessment named confusing the two as a key risk. Identity must be defined before schema, plans or projections refer to entities.

## What Changes

- An entity reference is graph identity, entity kind and an opaque identity value in a declared namespace.
- Namespaces may be global or group-scoped; aliases across groups resolve to one canonical reference when the catalog declares them equal.
- Identity value domains: an Int64 fast path, strings, binary and composite keys, each with declared equality and lossless round trip.
- Projection positions are a separate type that cannot become an identity without the projection's mapping.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `entity-identity`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-types`.
- **Human-scoped files:** `grustic-types/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `021-foundation-types-and-identity-api`, `029-logical-scalar-types`.
