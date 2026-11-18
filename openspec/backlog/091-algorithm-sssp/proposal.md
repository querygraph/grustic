# Proposal: Single-source shortest paths

## Why

Weighted shortest paths exercise edge features aligned with the projection, and the treatment of unreachable vertices. It is one of the six Graphalytics algorithms with reference outputs.

## What Changes

- Precise semantics of the Graphalytics variant of single-source shortest paths, named as such: inputs, parameters, directedness, duplicate edges, isolated vertices, output schema and determinism.
- A reference implementation over CSR projections.
- Graphalytics validation plus small targeted graphs where the semantic choices change the answer.

## Capabilities

### New Capabilities

- `algorithm-sssp`: Semantics and validation of single-source shortest paths variants, starting with Graphalytics.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-kernels` (algorithm module), `grustic-reference`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None planned. Any unsafe needs per-site approval with measurements.
- **Depends on:** `085-graphalytics-validation-harness`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
