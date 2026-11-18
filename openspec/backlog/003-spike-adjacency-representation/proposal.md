# Proposal: Spike: in-memory adjacency representation

## Why

The in-memory graph layout determines memory use, traversal speed and build cost for the reference backend and the projection contract. CSR is the common default, but other layouts win for sorted access by property, small selective traversals or frequent updates. This should be decided by measurement.

## What Changes

- Candidates: CSR with forward and reverse offsets and edge-id arrays; CSR stored as Arrow arrays; adjacency lists (`Vec<Vec<_>>`); linked adjacency records in the style of Neo4j; per-vertex hash or B-tree adjacency maps; compressed CSR (delta plus varint); and join-based traversal over Arrow edge tables with no projection, as the baseline.
- Workloads: build from an Arrow edge table; 1-hop, 2-hop and k-hop expand; variable-length paths; property-filtered expand; expand ordered by an edge timestamp; full sweeps (BFS, PageRank); a selective traversal on a large graph; batch insert and delete.
- Measurements: build time, peak and resident memory, bytes per edge, traversal throughput and re-sort cost.

## Hypotheses

- Immutable CSR (forward, plus reverse when needed) is close to the best candidate on every traversal workload and uses the least memory per edge. Mutable layouts win only on updates.
- Arrow-backed CSR traverses as fast as `Vec`-backed CSR and saves a copy at the Arrow boundary.
- For traversals that touch a small fraction of a large graph, join-based traversal over indexed edge tables beats building any projection.
- Re-sorting adjacency by an edge property costs less than one build, so it is a reasonable conversion.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `002-prior-art-review`.
- **Decision informed:** Default projection representations, when join-based traversal is preferred, and the shape of the graph-projection contract.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
