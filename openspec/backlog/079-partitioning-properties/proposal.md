# Proposal: Partitioning properties

## Why

Grustic does not schedule, but its plans must be able to describe distributed execution so that a distributed backend can implement them.

## What Changes

- A partitioning property: single, hash by keys, or vertex-partitioned by a named scheme.
- Exchange is a conversion provided by the backend; the planner never assumes local execution.
- The reference backend declares single partitioning only.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `physical-properties`: Adds partitioning and exchange conversions.

## Impact

- **Crates:** `grustic-plan`, `grustic-optimizer`.
- **Human-scoped files:** `grustic-plan/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `072-physical-properties-and-enforcement`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
