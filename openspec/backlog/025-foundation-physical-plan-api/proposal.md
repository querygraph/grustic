# Proposal: Foundation: physical plans, properties and rules

## Why

Backends, conversions and optimizer rules all meet in the physical layer. Its contracts decide whether extensions can add properties and implementations without editing core code.

## What Changes

- Physical plan nodes, convention identifiers, the physical property trait, and implementation and conversion traits.
- The rule trait with phases, the node kinds a rule understands, and ordering constraints.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `physical-plan`: Physical plans, conventions and the backend registration contract.
- `physical-properties`: Physical properties, requirements and their enforcement during planning.
- `optimizer-rules`: Registration, ordering and application of optimizer rules.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-plan`, `grustic-optimizer` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `024-foundation-logical-plan-api`.
