# Proposal: Optimizer decision trace

## Why

Users and backend authors need to see why a plan was chosen. Rejected alternatives and their reasons are what make optimizer regressions diagnosable.

## What Changes

- A decision trace with chosen and rejected alternatives, reasons, statistics provenance and inserted conversions.
- The trace is attached to explain output in text and machine-readable form.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `plan-explain`: Adds the optimizer decision trace.

## Impact

- **Crates:** `grustic-optimizer`, `grustic-plan`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `077-bounded-join-ordering`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
