# Proposal: Reference backend: adjacency-based traversal

## Why

Modularity requires a real choice between two faithful implementations with a visible conversion cost. This adds CSR-based traversal next to the scan-based one.

## What Changes

- CSR-based expand and variable-length path implementations in the reference backend.
- Identical answers to the scan-based implementations on LDBC workloads and targeted cases.
- Explain shows the chosen implementation and any inserted conversion.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `reference-graph-operators`: Adds adjacency-based implementations.

## Impact

- **Crates:** `grustic-reference`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None planned. Any unsafe needs per-site approval with measurements.
- **Depends on:** `073-graph-projections-and-conversions`, `068-ldbc-snb-interactive-complex-reads`, `049-plan-explain`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
