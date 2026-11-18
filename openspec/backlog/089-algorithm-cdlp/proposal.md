# Proposal: Community detection by label propagation

## Why

Label propagation is sensitive to update order and tie-breaking. Graphalytics fixes a deterministic variant that must be matched exactly. It is one of the six Graphalytics algorithms with reference outputs.

## What Changes

- Precise semantics of the Graphalytics variant of community detection by label propagation, named as such: inputs, parameters, directedness, duplicate edges, isolated vertices, output schema and determinism.
- A reference implementation over CSR projections.
- Graphalytics validation plus small targeted graphs where the semantic choices change the answer.

## Capabilities

### New Capabilities

- `algorithm-cdlp`: Semantics and validation of community detection by label propagation variants, starting with Graphalytics.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-kernels` (algorithm module), `grustic-reference`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None planned. Any unsafe needs per-site approval with measurements.
- **Depends on:** `085-graphalytics-validation-harness`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
