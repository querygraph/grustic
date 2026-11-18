# Proposal: Crate dependency boundaries

## Why

Consumers must be able to use the schema, parser, resolver or planner without pulling in Arrow, DataFusion or Sail. Without a mechanical check, an engine dependency can enter a core crate unnoticed and silently break that promise.

## What Changes

- A human-scoped `boundaries.toml` classifying crates as core or adapter.
- A check that the normal dependency closure of every core crate excludes `arrow*`, `datafusion*`, `sail*`, async runtimes and network crates.
- An external consumer fixture, built outside the workspace, that depends on one core crate and verifies the closure as a downstream user sees it.
- The check runs in CI and reports the offending dependency path.

## Capabilities

### New Capabilities

- `crate-dependency-boundaries`: Which crates may depend on engine libraries, and how violations are detected and reported.

### Modified Capabilities

None.

## Impact

- **Crates:** tooling only.
- **Human-scoped files:** `boundaries.toml` (not yet in CODEOWNERS; a maintainer adds it); `.github/workflows/` (maintainer-authored: agents may not write `.github/`, so the agent proposes the workflow text in the pull request).
- **Unsafe code:** None expected.
- **Depends on:** `014-workspace-and-ci-gates`.
- **Out of scope:** Version floors for adapter crates (see `arrow-dependency-floors`).
