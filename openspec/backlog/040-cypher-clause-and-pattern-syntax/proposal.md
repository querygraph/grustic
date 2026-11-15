# Proposal: Cypher clause and pattern syntax

## Why

Cypher is the first textual frontend. The parser must be usable on its own, for linters and tools, and free of the engine and write-dispatch code in the production grust-cypher crate.

## What Changes

- In `grustic-cypher`: lexer, AST and parser for read clauses: `MATCH`, `OPTIONAL MATCH`, `WHERE`, `WITH`, `RETURN`, `ORDER BY`, `SKIP`, `LIMIT`, `UNWIND`, `UNION` and `CALL` subqueries.
- Node and relationship patterns, including variable-length relationships and named paths.
- Every AST node carries a span.
- Write clauses are recognized and refused with a typed unsupported-clause diagnostic.
- The parser corpus is ported with attribution.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `cypher-syntax`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-cypher`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `026-foundation-cypher-api`, `015-crate-dependency-boundaries`, `016-public-api-snapshots`.
- **Ported from:** grust-cypher lexer, AST, parser and corpus, extracted with attribution. The source revision is not yet pinned; this change's design pins it before extraction.
