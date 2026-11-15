# Proposal: Spike: crate layout and dependencies

## Why

The crate split determines what consumers can take independently, how long builds take, and how the human-scoped and agent-scoped code split works in practice. Too many crates slow development; too few force consumers to take more than they need.

## What Changes

- Candidates: fine-grained (about twelve crates, as in the architecture map), coarse (about five crates with features) and a hybrid.
- Built from the extension spike's code arranged in each layout.
- Measurements: clean and incremental build times; dependency closure for schema-only, parser-only and planner-only consumers; behavior of public API snapshot and boundary tooling; friction of the `src/api/` convention; an Arrow version range that includes Sail's current pin.

## Hypotheses

- A hybrid of seven or eight crates keeps every consumer's dependency closure minimal with acceptable build times.
- The `src/api/` convention works with public API snapshots without macro workarounds.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `010-spike-extension-mechanism-end-to-end`.
- **Decision informed:** Crate list, dependency boundaries, workspace layout and the Arrow version range.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
