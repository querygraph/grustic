# Proposal: Cypher built-in function definitions

## Why

LDBC queries and ordinary Cypher need the built-in function library. Registering built-ins through the same public registry as third parties proves that the registry is sufficient.

## What Changes

- Semantic definitions for openCypher built-ins needed by LDBC SNB and common use: entity functions (`id`, `labels`, `type`, `properties`, `keys`, endpoints), path functions, `coalesce`, string, numeric, list and temporal functions, including temporal arithmetic and durations.
- Aggregates: `count`, `sum`, `avg`, `min`, `max` and `collect`, with `DISTINCT`.
- Registered only through the public registry API.

## Capabilities

### New Capabilities

- `cypher-builtin-functions`: Semantics of the built-in Cypher function library.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-functions` (built-in module).
- **Human-scoped files:** `grustic-functions/src/api/` if new public types are needed.
- **Unsafe code:** None expected.
- **Depends on:** `036-function-registry`, `045-resolve-basic-patterns`.
