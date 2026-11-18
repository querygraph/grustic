# Proposal: Cypher lowering to the unresolved plan

## Why

The parser and the Rust builder must produce equivalent unresolved plans. That equivalence is the evidence that the frontend is replaceable.

## What Changes

- Lowering from AST to unresolved plan, preserving spans.
- Equivalence tests: Cypher text and builder calls produce structurally equal plans for the same query.
- Constructs the plan does not yet support produce typed refusals at their spans.

## Capabilities

### New Capabilities

- `cypher-lowering`: Translation of Cypher syntax into unresolved plans.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-cypher`.
- **Human-scoped files:** `grustic-cypher/Cargo.toml` (new dependency on `grustic-plan`).
- **Unsafe code:** None expected.
- **Depends on:** `042-cypher-diagnostics-and-recovery`, `037-unresolved-plan-and-builder`.
