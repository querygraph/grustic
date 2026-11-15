# Proposal: Spike: identity to position mapping

## Why

Mapping external identities to dense positions often dominates projection build time, and must be lossless for Int64, string and composite identities. The result also decides whether 32-bit positions are worth supporting.

## What Changes

- Candidates: hash maps (std, hashbrown with different hashers), sorted arrays with binary search, Arrow dictionary encoding, and radix or perfect hashing for Int64.
- 32-bit versus 64-bit positions on the traversal workloads from the adjacency spike.
- Measurements: build time, lookup throughput, memory, and reverse mapping cost.

## Hypotheses

- For Int64 identities, a sorted array is competitive with hashing for build plus lookup and uses less memory.
- 32-bit positions give a measurable traversal speedup, which justifies supporting both widths.
- Dictionary-encoded string identities cost a bounded, small multiple of Int64 identities.

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
- **Decision informed:** Identity mapping strategy and position-width policy.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
