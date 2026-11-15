# Proposal: Foundation: errors, refusals and diagnostics

## Why

Every crate and extension reports errors, refusals and diagnostics. Their shapes must be fixed first, as decided in the error spike, because every other contract refers to them.

## What Changes

- Shared refusal and diagnostic types, span and location types, and stable code conventions.
- The pattern for per-crate error enums and for carrying extension errors.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `error-model`: Errors, refusals and diagnostics shared by every crate and extension.

### Modified Capabilities

None.

## Impact

- **Crates:** the crate chosen by the decision record.
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `017-unsafe-approval-ledger-check`, `016-public-api-snapshots`, `015-crate-dependency-boundaries`, `019-rust-conventions-in-project-memory`.
