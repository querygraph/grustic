# Proposal: Spike protocol and shared harness

## Why

Spikes must produce comparable, honest evidence that later decisions can cite. Without a shared protocol and shared data, each spike would invent its own datasets and measurements, and throwaway code could leak into the product.

## What Changes

- A `spikes/` directory outside the product workspace members, one subdirectory per spike change.
- A spike template: hypotheses with acceptance and kill criteria stated before measuring, a time box, candidates, measurements, results, and negative results kept.
- Shared data: LDBC SNB SF0.1 and SF1 loaders, plus synthetic generators for power-law graphs with hubs, long-diameter graphs, many isolates, parallel edges and self-loops.
- A measurement helper: repeated wall-clock timings with dispersion, peak RSS and allocation counts, build time separated from query time.
- Results are stored as data files in the change; findings go in the change's `design.md`.
- Rules: optimized native builds, one heavy job at a time, paired ratios on shared hosts. Spike numbers are directional and never published as performance claims.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike tooling).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** none.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
