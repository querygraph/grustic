# Proposal: Resolver extensions

## Why

Syntax extensions are useless unless their AST becomes typed plan nodes under the same checks as built-ins.

## What Changes

- A resolver extension contract, keyed by extension AST name, that produces resolved or extension logical nodes through the standard scope and type checks.
- An extension AST node with no registered resolver is refused at its span.
- An end-to-end test with a test-only syntax extension, resolver extension and logical extension node.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `resolver-extensions`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-resolver`, `grustic-session`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `039-logical-extension-nodes`, `059-cypher-syntax-extensions`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
