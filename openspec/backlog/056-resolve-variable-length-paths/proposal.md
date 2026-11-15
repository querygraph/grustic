# Proposal: Variable-length paths

## Why

Grust v2 expanded variable-length paths into unions with an eight-hop threshold, making a compiler limit part of the language. Paths must remain graph operations with explicit modes.

## What Changes

- Variable-length patterns resolve to a path operation with bounds, direction, path mode, multiplicity and returned path values.
- An unbounded upper bound is valid semantics. Resource limits are planning or execution refusals, never semantics.
- `shortestPath` and `allShortestPaths` resolve to distinct path selections.
- Path values expose nodes, relationships and length.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `query-resolution`: Adds variable-length path and shortest-path semantics.

## Impact

- **Crates:** `grustic-resolver`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `055-resolve-optional-match-and-uniqueness`.
- **Ported from:** Grust v2 Wave 3 path-value controls. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, `wave-3/sketch` under `docs/reviews/grust-v2/`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
