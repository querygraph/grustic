# Proposal: Spike: hosting on Sail

## Why

Nutmeg on Sail is the first consumer and must use only public extension points. Checking early whether a Grustic-shaped physical plan can run inside Sail catches contract misfits before the foundation is fixed.

## What Changes

- A spike outside the product workspace that depends on Sail's pinned DataFusion and Arrow.
- Wraps a toy physical plan with one graph operator as a DataFusion extension node, passes Arrow data both ways, and registers one function implementation.

## Hypotheses

- The contracts chosen by the earlier spikes can be hosted as DataFusion extension nodes without Grustic core types depending on DataFusion.
- No data copies are needed at the Arrow boundary.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `011-spike-crate-layout-and-dependencies`.
- **Decision informed:** Confirms or amends the backend contract before the foundation is written.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
