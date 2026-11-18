# Proposal: Foundation: session and backend

## Why

The session is how applications assemble Grustic, and the backend contract is how engines plug in. Both were checked against Sail in the hosting spike and must be fixed before Nutmeg and the reference backend are written.

## What Changes

- The session state builder, configuration types and the planning entry point.
- The backend trait, the host executor and cancellation contracts, and the resolver extension trait.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `session`: Session state, registration rules and the planning entry point.
- `resolver-extensions`: How extensions resolve their syntax into typed plan nodes.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-session`, `grustic-resolver` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `025-foundation-physical-plan-api`, `026-foundation-cypher-api`.
