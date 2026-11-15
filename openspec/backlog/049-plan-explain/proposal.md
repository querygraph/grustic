# Proposal: Plan explain

## Why

Every later change that chooses between implementations or inserts conversions must be able to show that choice. A structured explain is needed as soon as physical plans exist, long before the optimizer records its decisions.

## What Changes

- Structured explain of logical and physical plans in text and machine-readable form, built from each node's explain data, including extension nodes.
- Physical explain shows each node's convention, its implementation and any inserted conversion.
- Explain output is deterministic, so tests can compare it.

## Capabilities

### New Capabilities

- `plan-explain`: Explain output for logical and physical plans.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-plan`, `grustic-session`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `048-physical-plan-contract`.
