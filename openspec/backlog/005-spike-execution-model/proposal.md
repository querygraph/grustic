# Proposal: Spike: reference execution model

## Why

The reference executor's operator interface fixes how data flows between operators. It must be correct first, yet must not rule out factorized representations later. A small prototype on real queries shows what each shape costs before the interface is fixed.

## What Changes

- Candidates: pull-based iterators of Arrow record batches; push-based morsel pipelines; row-at-a-time as a reference point; and a minimal factorized representation for one many-to-many expand.
- Workloads: three or four LDBC Interactive queries of increasing complexity and one BI query, on SF1, as hand-written plans, since no parser or resolver exists yet.

## Hypotheses

- Pull-based batches are simple enough for a correctness-first reference and within a modest factor of push-based pipelines.
- An operator interface parameterized by convention adds no measurable cost.
- Signal only, not a decision: factorized intermediates shrink intermediate results by an order of magnitude on at least some LDBC queries.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `003-spike-adjacency-representation`.
- **Decision informed:** The reference executor interface, and how early factorization measurement should come.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
