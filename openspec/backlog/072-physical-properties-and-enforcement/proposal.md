# Proposal: Physical properties and enforcement

## Why

Implementations need properties from their inputs, such as ordering or adjacency sorted by an edge property, and extensions must be able to add new ones.

## What Changes

- An extensible physical property contract with built-in ordering and adjacency layout (orientation and sort key).
- Implementations declare required input properties and the properties they provide.
- The planner inserts registered conversions to satisfy requirements, or refuses when none exists.
- Memory feasibility uses the estimates that implementations and conversions declare, and is checked before cost; an infeasible plan is refused with its budget and estimate. Estimates derived from statistics arrive with the cost model.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `physical-properties`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-plan`, `grustic-optimizer`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `071-optimizer-rule-framework`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
