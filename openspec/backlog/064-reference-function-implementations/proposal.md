# Proposal: Reference backend: function implementations

## Why

Built-in functions need implementations for the reference backend. Registering them through the per-backend implementation API proves that the API serves third parties too.

## What Changes

- Implementations of the built-in function library on arrow-rs kernels, registered through the public API.
- Design goal G1 as a test-only extension: a user function such as `ai_score` registered with a reference implementation runs inside `MATCH ... WHERE` with no parser or core change. Without an implementation, planning refuses with the typed error naming the function and backend.

## Capabilities

### New Capabilities

- `reference-functions`: Function implementations provided by the reference backend.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-reference`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `046-cypher-builtin-functions`, `061-reference-relational-execution`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
