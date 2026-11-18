# Proposal: Resolution of fixed-length patterns

## Why

Resolution turns names into a typed plan bound to a catalog snapshot. This change starts the resolver with fixed-length patterns, the base for every later resolution feature.

## What Changes

- In `grustic-resolver`: scope and slot resolution, label and property binding against the snapshot, property provenance, expression and parameter typing, and function binding through the registry.
- Fixed-length node and relationship patterns resolve to scans and expands, not to joins.
- Typed errors with locations for unknown names, type mismatches and ambiguous references.
- Tests build unresolved plans with the Rust builder, so the resolver does not depend on the Cypher frontend.

## Capabilities

### New Capabilities

- `query-resolution`: How unresolved queries become typed resolved plans.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-resolver`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `027-foundation-session-and-backend-api`, `038-resolved-logical-plan-contract`.
- **Ported from:** Grust v2 Wave 3 resolver semantics for basic patterns, with independent expected answers. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, `wave-3/sketch` under `docs/reviews/grust-v2/`.
