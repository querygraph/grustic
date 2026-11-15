# Proposal: Reference backend: relational execution

## Why

The reference backend gives every user a runnable engine and every backend author an oracle. It starts with the relational operators every query needs.

## What Changes

- `grustic-reference` crate: in-memory tables of Arrow record batches, served as graphs through table-backed mappings.
- Expression evaluation on arrow-rs kernels with Cypher null semantics; scan, project, filter, sort, skip and limit, distinct, unwind and union.
- Correctness first: single-node and in-memory. An asynchronous executing session wraps the planning state, runs through the host executor contract with cooperative cancellation, and refuses with a typed error beyond the configured memory budget.
- Scans of extension catalog entry kinds run through implementations that the extension registers.
- Operator interfaces are defined per convention rather than fixed to flat record batches, so a factorized convention can be added later.

## Capabilities

### New Capabilities

- `reference-execution`: Behavior of the arrow-rs reference backend.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-reference` (new, adapter).
- **Human-scoped files:** new crate manifest, `src/lib.rs`, `src/api/` and `public-api.txt`; `boundaries.toml` entry.
- **Unsafe code:** None expected.
- **Depends on:** `054-dogfood-plan-review`, `048-physical-plan-contract`, `051-arrow-type-mapping`, `035-table-backed-graph-mapping`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
