# Proposal: Foundation: logical plans and extension nodes

## Why

Every frontend produces unresolved plans and every backend consumes resolved plans. The node enums, the extension node contract and checked construction must be fixed before the resolver is written.

## What Changes

- Unresolved and resolved plan node enums in the representation chosen by the plan spike.
- The extension node trait and the rule-facing transform API.
- Checked constructors with private fields.
- Only types, enums, constants and trait definitions, plus constructors and validation of those types.
- Requirements added here cover only what this change can test: construction and validation invariants, error and refusal kinds, and compile-fail checks. Behavior requirements are added by the changes that implement them, so no archived requirement lacks a test.
- Contract tests, including compile-fail tests for invariants enforced by the type system.

## Capabilities

### New Capabilities

- `unresolved-plan`: The frontend-independent unresolved query plan and its builder.
- `resolved-plan`: The canonical typed logical plan and its construction invariants.
- `logical-extension-nodes`: How third-party logical operators participate in resolved plans.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-plan` (new).
- **Human-scoped files:** the whole change. Maintainers own these files; an agent may draft them only with explicit approval of each file.
- **Unsafe code:** None expected.
- **Depends on:** `022-foundation-schema-and-catalog-api`, `023-foundation-functions-api`.
