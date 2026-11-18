# Proposal: Arrow dependency floors

## Why

Nutmeg links `grustic-arrow` alongside Sail's exact Arrow and DataFusion pins. A published version floor above the host's pin makes integration impossible even when the workspace lockfile builds.

## What Changes

- `grustic-arrow` declares an Arrow version range that includes the current Sail pin.
- CI builds an external fixture with the minimum and latest supported Arrow versions.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `crate-dependency-boundaries`: Adds supported version ranges for adapter crates.

## Impact

- **Crates:** `grustic-arrow`.
- **Human-scoped files:** `grustic-arrow/Cargo.toml`; `.github/workflows/` (maintainer-authored: agents may not write `.github/`, so the agent proposes the workflow text in the pull request).
- **Unsafe code:** None expected.
- **Depends on:** `051-arrow-type-mapping`.
