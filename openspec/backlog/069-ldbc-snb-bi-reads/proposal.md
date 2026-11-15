# Proposal: LDBC SNB BI reads

## Why

The BI workload stresses aggregation, large intermediate results and analytical patterns that Interactive does not. It is also the main input to the factorization measurements.

## What Changes

- BI read queries as Cypher text from a pinned LDBC reference implementation, on the BI validation dataset.
- Each query passes or has an explicit refusal with its reason.
- Queries whose reference implementation calls graph-algorithm procedures, such as weighted shortest paths, are recorded as pending until algorithm calls exist, and reported as refusals rather than wrong answers.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `ldbc-snb-conformance`: Adds the BI read workload.

## Impact

- **Crates:** `grustic-ldbc`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `068-ldbc-snb-interactive-complex-reads`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
