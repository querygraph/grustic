# Proposal: Unsafe approval ledger check

## Why

Every unsafe site needs explicit human approval recorded in `unsafe-approvals.toml`. Nothing yet connects code to ledger entries, so an unapproved or stale site would only be caught by reading every diff.

## What Changes

- A typed Python checker (Ruff, mypy) that finds every `#[expect(unsafe_code, reason = "UNSAFE-NNNN: ...")]` and every unsafe block, function, trait and impl.
- It fails when an unsafe item has no `UNSAFE-NNNN` id, an id has no ledger entry, a ledger entry has no site, `#[allow(unsafe_code)]` is used instead of `expect`, or an unsafe block lacks a `// SAFETY:` comment.
- Checker tests live in separate files with small fixture crates.
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
