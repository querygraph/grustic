# Proposal: Experimental release readiness

## Why

Before any release, each crate must work as a downstream user sees it, and the supported surface must be stated. A refusal is honest, but it does not count as support.

## What Changes

- External consumer fixtures: schema only, parser only, resolver with a custom catalog, and planner with the minimal second backend.
- Each crate is packaged and tested from outside the workspace.
- Every release declares, per capability, what is supported, refused or pending. Each supported entry links the tests that are its evidence. The inventory is a generated release artifact; the requirement is that it exists and is complete.
- No publishing. That needs a separate, explicit maintainer decision.

## Capabilities

### New Capabilities

- `release-inventory`: What every release must declare about its supported, refused and pending surface, and the evidence for it.

### Modified Capabilities

None.

## Impact

- **Crates:** tooling and fixtures.
- **Human-scoped files:** `.github/workflows/` (maintainer-authored: agents may not write `.github/`, so the agent proposes the workflow text in the pull request).
- **Unsafe code:** None expected.
- **Depends on:** `080-plan-codec`, `082-factorization-measurement`, `091-algorithm-sssp`, `090-algorithm-lcc`, `089-algorithm-cdlp`, `088-algorithm-pagerank`, `087-algorithm-wcc`, `086-algorithm-bfs`, `052-arrow-dependency-floors`, `017-unsafe-approval-ledger-check`, `079-partitioning-properties`, `065-graph-data-validation`, `018-scenario-test-coverage-check`, `043-cypher-parser-robustness`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
