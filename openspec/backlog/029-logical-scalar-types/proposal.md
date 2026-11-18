# Proposal: Logical scalar types

## Why

Every phase needs one shared vocabulary of value types. Grust v2 expressions carried an optional logical type and had no explicit null-only type, so typing was partial. A total, explicit type system is the base for checked plans.

## What Changes

- In `grustic-types`: scalar logical types needed by openCypher and LDBC SNB: boolean, integers, floats, string, bytes, date, local and zoned time and datetime, duration. The exact set is decided in design.
- An explicit null-only type and per-type nullability.
- Logical values for constants and parameters.
- Equality, comparability and orderability classes per type, including null and NaN behavior.
- The implicit coercion lattice (for example integer to float) expressed as data.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `logical-types`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-types`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `021-foundation-types-and-identity-api`, `015-crate-dependency-boundaries`, `016-public-api-snapshots`.
- **Ported from:** Grust v2 Wave 2 logical types, minimal closure, with attribution. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, the necessary `wave-2/sketch` contracts under `docs/reviews/grust-v2/`.
- **Out of scope:** Nested and graph-valued types; Arrow mapping.
