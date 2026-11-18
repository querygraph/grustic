# Proposal: Foundation: function definitions

## Why

Function definitions are the most common extension. Their semantic contract and the per-backend implementation contract must be fixed before the registry and the built-in library are implemented.

## What Changes

- Function definition, signature, null behavior, volatility and cost hint types.
- The registry trait and the per-backend implementation registration trait.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `function-registry`: Semantic function definitions, overload resolution and registration rules.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-functions` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `021-foundation-types-and-identity-api`.
