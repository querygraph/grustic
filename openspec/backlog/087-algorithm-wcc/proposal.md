# Proposal: Weakly connected components

## Why

Connected components are part of the first complete use case. Label conventions such as minimum-ID labels must be exact rather than any equivalent partition. It is one of the six Graphalytics algorithms with reference outputs.

## What Changes

- Precise semantics of the Graphalytics variant of weakly connected components, named as such: inputs, parameters, directedness, duplicate edges, isolated vertices, output schema and determinism.
- A reference implementation over CSR projections.
- Graphalytics validation plus small targeted graphs where the semantic choices change the answer.

## Capabilities

### New Capabilities

- `algorithm-wcc`: Semantics and validation of weakly connected components variants, starting with Graphalytics.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-kernels` (algorithm module), `grustic-reference`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None planned. Any unsafe needs per-site approval with measurements.
- **Depends on:** `085-graphalytics-validation-harness`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
