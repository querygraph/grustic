# Proposal: Statistics and cost model

## Why

Choosing between implementations needs estimates. Grust v2's simple weighted cost produced plans up to about 1.55 times slower on larger controls, so costs must be explicit, include conversions and keep unknowns unknown.

## What Changes

- A statistics provider contract: row and byte counts, degree distribution and skew, distinct counts, layout and partitioning, each with provenance and confidence.
- Unknown estimates stay unknown and never default to zero.
- Costs include conversions and memory. Feasibility is decided before ranking.
- A regression set that records slower plans rather than hiding them.
- Wave 3 evidence is a starting point, not a verdict: its optimized plans had median ratios up to 1.554 against resolved plans on larger controls, but its 100-fold synthetic replicas kept the original disconnected workload. They are not an official scale factor and cannot stand in for connected, skewed graphs, and its sampled memory control does not establish a general bound.

## Capabilities

### New Capabilities

- `cost-model`: Statistics, provenance and cost estimation for physical choices.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-optimizer`, `grustic-schema`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `074-reference-adjacency-implementations`, `070-benchmark-harness`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
