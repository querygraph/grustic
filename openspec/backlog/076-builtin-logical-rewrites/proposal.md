# Proposal: Built-in logical rewrites

## Why

Standard rewrites make plans efficient without changing answers, and being registered like third-party rules proves the rule framework is sufficient.

## What Changes

- Predicate pushdown into scans, expands and paths where legal.
- Projection pruning.
- Constant folding that respects volatility.
- Decorrelation of correlated apply where legal.

## Capabilities

### New Capabilities

- `logical-rewrites`: Semantics-preserving built-in rewrites and their legality conditions.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-optimizer`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `071-optimizer-rule-framework`, `057-resolve-subqueries-and-correlation`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
