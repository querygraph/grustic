# Proposal: Graphalytics validation harness

## Why

Algorithm correctness comes from LDBC Graphalytics reference outputs. Each algorithm needs its own comparison rule, such as partition equivalence or a numeric tolerance.

## What Changes

- A loader for Graphalytics datasets and reference outputs.
- Comparison rules per algorithm, defined in each algorithm's spec.
- A report that keeps pass, wrong answer, error and refusal distinct, with dataset identity, license and hash.

## Capabilities

### New Capabilities

- `graphalytics-conformance`: How Graphalytics datasets are loaded, run and judged.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-ldbc`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `084-algorithm-calls`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
