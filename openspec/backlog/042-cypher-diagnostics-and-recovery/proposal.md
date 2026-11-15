# Proposal: Cypher diagnostics and error recovery

## Why

Tools and users need all syntax errors at once, with accurate locations. Grust v2's adapter returned a single diagnostic.

## What Changes

- Error recovery at statement and clause boundaries; multiple diagnostics per input, each with span, stable code and message.
- Partial ASTs are marked invalid and can never be lowered or executed.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `cypher-diagnostics`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-cypher`.
- **Human-scoped files:** `grustic-cypher/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `041-cypher-expression-syntax`.
