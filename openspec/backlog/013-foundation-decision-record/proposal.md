# Proposal: Foundation decision record

## Why

Spike evidence is only useful once it becomes explicit decisions that humans accept. This change collects them in one reviewable place before any product code exists.

## What Changes

- For each spike: the decision, its evidence (the archived spike change), rejected alternatives and remaining risks.
- Project memory states the current decisions and their rationale, and the architecture map is updated with the chosen crate list.
- Policy and discipline decisions go to `AGENTS.md`, which a maintainer writes from text the agent proposes in the pull request. Coding conventions go to project memory through the Rust conventions change.
- `spikes/` code is removed, including the shared harness. Results remain in the archived spike changes, and the harness design is the starting point for the LDBC validation and benchmark harness changes.
- Later proposals whose assumptions changed are updated.
- The port disposition of Grust v2 sources is confirmed: semantics, tests and corpora are ported by contract with attribution; the Python iterative runner, old sketch APIs and ad hoc fixtures stay in Grust as references; historical build targets, binaries and evidence trees are never imported; existing Grust native kernels are reviewed one algorithm at a time, with no wholesale facade.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none.
- **Human-scoped files:** `AGENTS.md` (maintainer-authored).
- **Unsafe code:** None expected.
- **Depends on:** `012-spike-sail-hosting-probe`.
