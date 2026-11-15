# Proposal: Function registry

## Why

Functions must be extensible without parser or core changes, and their meaning must be separate from any backend's implementation. Grust v2's registry mixed semantic identity with backend flags.

## What Changes

- In `grustic-functions`: scalar and aggregate definitions with namespaced semantic identity.
- Signatures, overload resolution, null behavior, volatility and cost hints.
- Conflicting registrations are errors at registration time.
- Implementations are registered separately per backend. A resolved call carries only the semantic identity.
- Requirement recorded here and enforced once backends exist: planning refuses with a typed error naming the function and backend when no implementation is registered.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `function-registry`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-functions`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `023-foundation-functions-api`, `030-nested-and-graph-value-types`.
- **Ported from:** Grust v2 Wave 2 function registry, with backend flags replaced by per-backend implementation registration. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, the necessary `wave-2/sketch` contracts under `docs/reviews/grust-v2/`.
