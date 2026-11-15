# Proposal: Foundation: logical types and identity

## Why

Every phase refers to logical types, values, entity identities and projection positions. Their public types must exist before anything else can be written against them.

## What Changes

- Logical type and value enums with nullability, nested and graph-valued types.
- Entity reference, identity namespace and identity value types; a separate projection position type with the width policy from the identity spike.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `logical-types`: Logical value types, nullability, comparison classes and coercion.
- `entity-identity`: Entity references, identity namespaces and value domains, kept distinct from projection positions.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-types` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `020-foundation-errors-and-diagnostics-api`.
