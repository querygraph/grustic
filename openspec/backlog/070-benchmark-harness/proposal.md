# Proposal: Benchmark harness

## Why

Cost-model and implementation choices need measured evidence, and the spike harness was throwaway code. Product benchmarks need one harness that follows the repository's measurement rules.

## What Changes

- Benchmarks over LDBC SNB workloads and synthetic graph shapes: hubs, long diameters, isolates, parallel edges and self-loops.
- Optimized native builds, one heavy job at a time, repeated timings with dispersion and peak memory; projection and build time reported separately from query time.
- Shared-host results are paired ratios with every outcome kept, including slower ones and refusals.
- Workload shapes also include a graph that exceeds the projection admission limit, selective and unselective filters, one call versus repeated calls on the same snapshot, and cold versus warm caches.
- Measured work is complete: read, decode, identity mapping, projection, algorithm and output, separated where the measurement can separate them. Explicit validation stays outside the timers.
- Result data is stored outside the repository and referenced by hash. Benchmarks never gate CI.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** `grustic-ldbc` (benchmark targets).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `066-ldbc-snb-validation-harness`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
