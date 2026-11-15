# Proposal: Nested and graph-valued types

## Why

Cypher and Spark results contain lists, maps, structs and graph values. Grust v2 had no map type and no independent list-element nullability. These must exist before resolved plans can be fully typed.

## What Changes

- Lists with independent element nullability; structs with named fields; maps with key and value types, a null-key policy and equality.
- Graph-valued types: node reference, relationship reference and path.
- Type equality and common-supertype rules used by `CASE`, `coalesce`, `UNION` and list literals.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `logical-types`: Adds nested and graph-valued types and supertype rules.

## Impact

- **Crates:** `grustic-types`.
- **Human-scoped files:** `grustic-types/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `029-logical-scalar-types`.
