# Proposal: Public API snapshots

## Why

AGENTS.md makes the public surface human-scoped, but Rust visibility is not tied to file paths: an implementation module can expose an item. A committed snapshot per crate makes every public API change visible in review and routes it to code owners.

## What Changes

- Generate a textual public API listing per publishable crate into `crates/<name>/public-api.txt` with a pinned tool such as `cargo-public-api`.
- CI fails when the generated listing differs from the committed snapshot.
- A script regenerates snapshots locally.
- `cargo-public-api` reads nightly rustdoc JSON, so this check alone uses a second, pinned nightly toolchain. Builds and tests stay on the pinned stable toolchain.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** tooling only.
- **Human-scoped files:** `.github/workflows/` (maintainer-authored: agents may not write `.github/`, so the agent proposes the workflow text in the pull request); tool pin in the workspace manifest.
- **Unsafe code:** None expected.
- **Depends on:** `014-workspace-and-ci-gates`.
