# Proposal: Factorization and worst-case-optimal join measurement

## Why

Kùzu-style factorization and worst-case-optimal joins were postponed until numbers support a decision. This produces those numbers without committing to an implementation.

## What Changes

- A tool that runs LDBC Interactive and BI queries with execution metrics and records flat intermediate sizes per operator.
- Estimated factorized sizes for many-to-many expansions and worst-case-optimal bounds for cyclic patterns.
- Results for every query, including those where neither helps, as data for a human decision.
- No factorized or worst-case-optimal implementation.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** `grustic-ldbc` (tooling).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `081-execution-metrics`, `069-ldbc-snb-bi-reads`.
- **Note:** Any implementation follows as separate changes after a human decision. Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
