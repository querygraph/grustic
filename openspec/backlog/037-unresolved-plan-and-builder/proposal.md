# Proposal: Unresolved plan and Rust builder

## Why

A parser-free client must be able to build the same queries as Cypher. The unresolved plan is the common target for every frontend and the first contract that downstream tools rely on.

## What Changes

- In `grustic-plan`: unresolved plan nodes for patterns, projection, filtering, aggregation, ordering, paging, unwind, subqueries and parameters.
- Structured source locations: text spans from parsers, or caller-supplied locations from programmatic clients.
- A Rust builder API with immediate structural checks, such as empty patterns or duplicate aliases in one scope.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `unresolved-plan`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-plan`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `024-foundation-logical-plan-api`, `033-catalog-and-snapshots`, `036-function-registry`.
- **Ported from:** Grust v2 Wave 2 parser-free builder. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, the necessary `wave-2/sketch` contracts under `docs/reviews/grust-v2/`.
