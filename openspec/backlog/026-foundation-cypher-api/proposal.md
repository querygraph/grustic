# Proposal: Foundation: Cypher syntax tree and parser hooks

## Why

The parser is a standalone product for tools, and its AST is public. The AST types and the syntax extension hooks must be fixed before the grammar is implemented.

## What Changes

- AST node types with spans, the parser entry point and its diagnostic output.
- The syntax extension trait and its hook points, as chosen by the parser spike.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `cypher-syntax`: The accepted Cypher read-query syntax and its AST.
- `cypher-diagnostics`: Diagnostic reporting and recovery for Cypher input.
- `cypher-syntax-extensions`: How extensions add syntax to the Cypher parser.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-cypher` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `020-foundation-errors-and-diagnostics-api`.
