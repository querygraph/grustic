# Proposal: Spike: Cosmolang exploration protocol

## Why

Cosmolang is the versioned control protocol for exploring graph snapshots from browsers and agents. The earlier C1 slice qualified a bounded two-level exploration on Sail and Nutmeg with supplied groups and layouts. Before any contract is written, this spike tests whether its operations can be served from Grustic's public crates, and where hierarchy, indexing and caps belong.

## What Changes

- Candidates: operations expressed as Grustic queries and algorithm calls; dedicated view operators as extension nodes; a separate service layer over the session.
- Operations from C1: request a bounded view, follow edges, expand and collapse a group, and run WCC on a stated graph (source graph, induced subset or quotient graph).
- One dispatcher behind HTTP and Model Context Protocol transports; neither transport owns graph semantics.
- Hierarchy as versioned data: disjoint membership per level, stable group identity, quotient edges with declared aggregation, and a layout tied to the same snapshot.
- View revisions per request, session and view; stale or delayed responses never overwrite a newer view; identities stay lossless across JSON, including integers beyond JavaScript's exact range.
- Node, edge and byte caps: results over a cap are refused or changed only through an explicitly requested approximation, never silently sampled.
- Workloads: a pinned LDBC SNB graph and a synthetic graph with hubs, with a precomputed multi-level hierarchy.

## Hypotheses

- Every C1 operation can be expressed as a Grustic plan plus a small number of extension nodes, with no core change.
- With indexed membership and endpoint access, expanding a group costs time proportional to that group and its incident edges, not to the graph.
- Protocol semantics belong outside the core crates, in a Cosmolang crate or repository that depends only on public Grustic crates.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `092-experimental-release-readiness`.
- **Ported from:** Cosmolang schemas and C1 controls from `querygraph/grust` at `d56a4993f3378fa4775ad261a7bc4926ef23c449` (`work/cosmolang-live`), with attribution.
- **Decision informed:** Where Cosmolang lives, its operation contract, and which Grustic hooks it needs.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the exploration decision record removes it. Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
