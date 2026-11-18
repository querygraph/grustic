# Proposal: Prior-art review

## Why

Several systems have solved parts of this problem. GOpt is a Calcite-based graph optimizer, DuckPGQ adds SQL/PGQ to an embeddable engine, Kùzu uses factorized execution and worst-case-optimal joins, and Morpheus brought Cypher to Spark before being abandoned. Their lessons should shape the spike hypotheses rather than be rediscovered.

## What Changes

- A review of GraphScope GOpt, DuckPGQ, Kùzu, Morpheus/CAPS, the Neo4j planner, Calcite traits, conventions and metadata, DataFusion's TreeNode, extension and SessionState APIs, sqlparser-rs dialects, and GraphAr.
- For each: what to adopt and what to avoid, with sources.
- Refined hypotheses and candidate lists for the following spikes.
- No code. Findings go in the change's `design.md`.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `001-spike-protocol-and-harness`.
- **Decision informed:** Hypotheses and candidates for every later spike.
