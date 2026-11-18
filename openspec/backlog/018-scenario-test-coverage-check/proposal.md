# Proposal: Scenario test coverage check

## Why

AGENTS.md requires every scenario to be covered by at least one test whose name or doc comment names its requirement and scenario. Without a mechanical check, an uncovered scenario is only found by reading every test.

## What Changes

- A typed Python checker (Ruff, mypy) that reads every requirement and scenario in `openspec/specs/` and in the deltas of active changes.
- It fails when a scenario has no test naming its requirement and scenario, unless the scenario is marked as not automatable with its manual check.
- It reports uncovered scenarios by capability and requirement.
- Checker tests live in separate files with small fixtures.
- The check runs in CI.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** tooling only.
- **Human-scoped files:** `.github/workflows/` (maintainer-authored: agents may not write `.github/`, so the agent proposes the workflow text in the pull request).
- **Unsafe code:** None expected.
- **Depends on:** `014-workspace-and-ci-gates`.
