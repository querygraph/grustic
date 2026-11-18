# Proposal: Bounded join ordering

## Why

Wave 3's join enumeration is useful but must stay a bounded, optional strategy that respects barriers and reports when it gives up.

## What Changes

- Join enumeration over inner regions with a configurable planning limit.
- Deterministic fallback when the limit is reached, recorded in the plan.
- Never reorders across barriers, optional or correlated operations.

## Capabilities

### New Capabilities

- `join-ordering`: Bounded join-order search and its limits.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-optimizer`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `075-statistics-and-cost-model`, `076-builtin-logical-rewrites`.
- **Ported from:** Grust v2 Wave 3 join optimizer. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, `wave-3/sketch` under `docs/reviews/grust-v2/`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
