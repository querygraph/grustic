# Proposal: Spike: extension mechanism end to end

## Why

Extension points only matter if they work together. A throwaway end-to-end slice shows whether the session, registries and traits compose before the foundation contracts are fixed.

## What Changes

- A minimal pipeline built from the winning plan, parser, error and session candidates: session with registries, syntax hook, resolver extension, logical extension node, physical property requirement with a conversion, and a reference implementation.
- Exercises the three design goals: a user function in a query, a temporal path with re-sorted adjacency, and a table entry kind joined with graph results.
- Checks object safety, `Send` and `Sync`, generics versus trait objects at each hook, registration conflicts and error propagation across extension boundaries.
- Settles open decisions: extension naming and versioning (`vendor.name@version` proposed); whether syntax extensions may add reserved keywords or only contextual ones; rule ordering by before and after constraints or by fixed named stages.
- Starts from the candidate trait shapes and rejected alternatives in this change's `design.md`.

## Hypotheses

- Every hook works as a trait object registered in session state, without generic parameters leaking into the session type.
- None of the three design goals needs an edit to core enums.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `008-spike-error-and-diagnostic-model`, `009-spike-session-async-and-hosting`, `003-spike-adjacency-representation`.
- **Decision informed:** Extension trait shapes and session state structure.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
