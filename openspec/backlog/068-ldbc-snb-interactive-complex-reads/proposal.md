# Proposal: LDBC SNB Interactive complex reads

## Why

The complex reads exercise variable-length paths, shortest paths, correlation, aggregation and ordering. Wave 3 passed IC2, IC8 and IC9 unchanged; Grustic must reach and then extend that coverage.

## What Changes

- Interactive complex reads IC1 to IC14 as unchanged Cypher text.
- Each query passes or has an explicit refusal with its reason; the report lists both.
- IC2, IC8 and IC9 must pass.
- Baseline to reach: the Wave 3 complex-SNB gate (`querygraph/grust` at `b2d5d8b1f3866272bc637fb76e544f35c2c3321c`) ran unchanged IC2, IC8 and IC9 and five short-query families against independent CSV answers, recording 860 successful result cells and 8 expected arithmetic errors, with 18 capability refusals kept separate.
- Query texts come from a pinned LDBC reference implementation, recorded with revision and license. Before work starts, the grammar and functions they use are checked against the expression syntax and built-in function changes; each gap becomes its own change.
- Queries that end as refusals are listed as pending. A refusal does not count as support.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `ldbc-snb-conformance`: Adds the Interactive complex-read workload.

## Impact

- **Crates:** `grustic-ldbc`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `067-ldbc-snb-interactive-short-reads`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
