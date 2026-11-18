# Proposal: Resolved logical plan contract

## Why

There must be exactly one canonical, fully typed resolved plan. Grust v2 had parallel sketch and executable plans with public fields. Backends and optimizers depend on this contract, so it must reject malformed plans however they are constructed.

## What Changes

- Resolved nodes keep graph operations: node scan, expand, path match and selection, optional and correlated apply, alongside relational project, filter, join, aggregate, sort, limit, union and unwind.
- Every expression and output column is fully typed with explicit nullability.
- Checked constructors establish scope, slot uniqueness, output schema and function binding. No public fields.
- Deserialized and third-party-constructed plans pass the same checks.
- Every node provides explain data.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `resolved-plan`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-plan`.
- **Human-scoped files:** `grustic-plan/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `037-unresolved-plan-and-builder`.
- **Ported from:** Grust v2 Wave 3 executable resolved-plan semantics, with a revised representation. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, `wave-3/sketch` under `docs/reviews/grust-v2/`.
