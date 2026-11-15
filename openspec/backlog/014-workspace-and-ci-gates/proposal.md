# Proposal: Rust workspace and CI gates

## Why

There is no Rust workspace yet. Every later change needs one place where the toolchain, lints and gates are defined, so that contributions from any person, agent or machine are checked identically before review.

## What Changes

- Cargo workspace with a `crates/` layout, pinned `rust-toolchain.toml`, edition and minimum supported Rust version.
- Workspace lints: `unsafe_code = "deny"`; Clippy `undocumented_unsafe_blocks` and `multiple_unsafe_ops_per_block` denied; `missing_docs` on public items; an agreed pedantic subset.
- CI on a clean checkout of the exact commit with `CARGO_INCREMENTAL=0`: `cargo fmt --check`, `cargo clippy --workspace --all-targets -D warnings`, `cargo test --workspace`, `cargo doc --no-deps` with warnings denied.
- CI runs `openspec validate --all --strict`.
- `cargo-deny` for licenses, advisories and duplicate versions.
- `pyproject.toml` with Ruff and mypy for repository scripts.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (workspace only).
- **Human-scoped files:** `Cargo.toml`, `rust-toolchain.toml`, `deny.toml`, `.cargo/` and `pyproject.toml`, each with explicit approval; `pyproject.toml` is not yet in CODEOWNERS, so a maintainer adds it. `.github/workflows/` (maintainer-authored: agents may not write `.github/`, so the agent proposes the workflow text in the pull request).
- **Unsafe code:** None expected.
- **Depends on:** `013-foundation-decision-record`.
- **Out of scope:** Crates themselves; dependency-boundary, public API and unsafe-ledger checks (separate changes).
