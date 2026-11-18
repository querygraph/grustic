# Proposal: Cypher expression syntax

## Why

Expressions are the larger half of the Cypher grammar and are needed by every clause. Separating them from clause syntax keeps both changes reviewable.

## What Changes

- Literals, parameters, property access, operators with precedence, `CASE`, list and map literals, list and pattern comprehensions, `reduce`, the list predicates `all`, `any`, `none` and `single`, and slicing.
- Function calls by qualified name, including `DISTINCT` aggregates.
- Subquery expressions: `EXISTS { }`, `COUNT { }` and pattern predicates.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `cypher-syntax`: Adds the expression grammar.

## Impact

- **Crates:** `grustic-cypher`.
- **Human-scoped files:** `grustic-cypher/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `040-cypher-clause-and-pattern-syntax`.
