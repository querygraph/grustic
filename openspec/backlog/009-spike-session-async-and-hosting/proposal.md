# Proposal: Spike: session, async boundary and execution hosting

## Why

Grustic's core is synchronous and engine-free, while hosts such as Sail use async runtimes and own thread pools, memory and cancellation. The boundary must be decided before the session and backend contracts are fixed.

## What Changes

- Candidates: a synchronous core with one asynchronous snapshot-collection step; an asynchronous resolver; a host executor contract versus a global pool in the reference backend.
- Exercised with a Tokio-hosted toy backend and the synchronous reference prototype.

## Hypotheses

- Synchronous resolution plus one asynchronous snapshot step is enough for remote catalogs.
- A host executor contract with cooperative cancellation serves both a Tokio host and the synchronous reference without a global pool.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `005-spike-execution-model`.
- **Decision informed:** Async boundary, executor contract and cancellation model.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
