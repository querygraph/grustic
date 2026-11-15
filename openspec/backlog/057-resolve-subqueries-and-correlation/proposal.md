# Proposal: Subqueries and correlation

## Why

Correlated subqueries appear throughout LDBC complex reads, and Wave 3 tested their semantics. They must resolve to explicit correlated operations so that optimizers can decorrelate only where legal.

## What Changes

- `EXISTS`, `COUNT` and pattern-predicate subqueries, `CALL { }` subqueries, list and pattern comprehensions, `reduce` and list predicates.
- Correlated apply nodes with explicit outer references.
- Scope rules for variables imported into and exported from subqueries.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `query-resolution`: Adds subquery and correlation semantics.

## Impact

- **Crates:** `grustic-resolver`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `055-resolve-optional-match-and-uniqueness`.
- **Ported from:** Grust v2 Wave 3 correlation controls. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, `wave-3/sketch` under `docs/reviews/grust-v2/`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
