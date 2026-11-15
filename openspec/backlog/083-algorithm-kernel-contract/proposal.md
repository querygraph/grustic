# Proposal: Algorithm kernel contract

## Why

Graph algorithms need semantics separate from implementation, and explicit resource ownership: feature alignment, reverse adjacency, memory estimates, host-provided execution, cancellation and output lifetime.

## What Changes

- In `grustic-kernels`: the kernel contract.
- Algorithm identity is its semantics: directedness, duplicate edges, initialization, convergence, ties, nondeterminism and output schema.
- Requirements: projection properties, features and memory estimate.
- A host executor contract with no global thread pool, cooperative cancellation, and output ownership or leases.
- Resource accounting covers staging, identity maps, forward and reverse adjacency, scratch space, features and outputs, including peak overlap during construction. Retained Arrow slices keep their admission, and producer completion is reported separately from a reservation reaching zero.
- Each run records its computation identity: source snapshots, parameters, code identity, semantics variant, partition assumptions and any random seed.
- An algorithm is identified by name plus a named semantics variant, for example the Graphalytics PageRank. Other variants, such as those GraphFrames clients expect, can be added later without changing an existing variant's meaning.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `algorithm-kernels`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-kernels`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `028-foundation-kernel-and-projection-api`, `073-graph-projections-and-conversions`.
- **Ported from:** Grust v2 Wave 2 kernel contract, completed at the resource boundary. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, the necessary `wave-2/sketch` contracts under `docs/reviews/grust-v2/`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
