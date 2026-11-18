# Proposal: Foundation: kernels and projections

## Why

Algorithms and projections carry the resource ownership rules: memory accounting, host execution, cancellation and output lifetime. They follow the adjacency and identity spikes and must be fixed before any algorithm is written.

## What Changes

- Projection identity, representation and conversion types as chosen by the adjacency spike.
- The kernel contract: algorithm semantics identity, requirements, host execution, cancellation and output ownership.
- Resource accounting types that can express staging, identity maps, forward and reverse adjacency, scratch, features and outputs, peak construction overlap, retained-slice admission, and producer completion as a signal separate from a reservation reaching zero.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `graph-projections`: Graph projections, their identity, conversions and resource accounting.
- `algorithm-kernels`: The contract between algorithm semantics, implementations and their host.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-kernels` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `025-foundation-physical-plan-api`.
