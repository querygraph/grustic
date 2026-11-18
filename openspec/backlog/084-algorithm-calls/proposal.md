# Proposal: Algorithm calls in queries

## Why

Algorithms must compose with queries: take a graph or subgraph as input and return a relation that later clauses can use.

## What Changes

- `CALL namespace.algorithm(args) YIELD ...` in Cypher, and the builder equivalent.
- Resolution to an algorithm-call node whose graph input is a catalog graph or a query-defined subgraph.
- Typed refusal for unknown algorithms or backends without an implementation.
- The `CALL ... YIELD` syntax and its resolution are built through the public syntax and resolver extension contracts, which proves they are sufficient for a built-in feature.

## Capabilities

### New Capabilities

- `algorithm-calls`: Invoking algorithms from queries and composing their results.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-cypher`, `grustic-resolver`, `grustic-plan`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of the affected crates.
- **Unsafe code:** None expected.
- **Depends on:** `083-algorithm-kernel-contract`, `060-resolver-extensions`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
