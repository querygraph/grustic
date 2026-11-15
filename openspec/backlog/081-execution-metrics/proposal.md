# Proposal: Execution metrics

## Why

Diagnosing plans, comparing implementations and measuring factorization potential all need per-operator measurements from real execution.

## What Changes

- Per-operator rows, batches, bytes, elapsed time and peak memory in the reference backend.
- Explain with measured results attached to the plan.

## Capabilities

### New Capabilities

- `execution-metrics`: Per-operator execution measurements and their reporting.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-reference`, `grustic-plan`.
- **Human-scoped files:** `grustic-plan/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `078-optimizer-decision-trace`, `061-reference-relational-execution`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
