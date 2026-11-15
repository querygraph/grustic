# Proposal: LDBC SNB validation harness

## Why

Correctness evidence comes primarily from LDBC workloads with reference results. A harness that loads validation datasets, runs queries and compares answers must exist before any workload is claimed.

## What Changes

- A loader for LDBC SNB validation datasets into reference tables, with a table-backed mapping of the LDBC schema.
- A runner that executes parameterized queries and compares results to expected answers, using ordered comparison only where the query defines an order.
- A report that keeps pass, wrong answer, error and refusal distinct, and records dataset identity, license and hash.
- Datasets are downloaded on demand and never stored in the repository.

## Capabilities

### New Capabilities

- `ldbc-snb-conformance`: How LDBC SNB workloads are loaded, run and judged.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-ldbc` (new, test tooling).
- **Human-scoped files:** new crate manifest, `src/lib.rs`, `src/api/` and `public-api.txt`
- **Unsafe code:** None expected.
- **Depends on:** `063-reference-graph-traversal`, `064-reference-function-implementations`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
