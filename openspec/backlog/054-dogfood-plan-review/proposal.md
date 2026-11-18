# Proposal: Plan review after the foundation and dogfood

## Why

The rest of the plan was written before any foundation code existed or Nutmeg consumed it. The foundation and its first real consumer are the evidence for adjusting it, so the remaining proposals are reconsidered before any of them starts.

## What Changes

- Findings from the vertical slice and the Nutmeg checkpoint: which contracts held, which needed MODIFIED deltas, and which hooks were missing.
- A revised list of the remaining proposals, each kept, changed, merged, reordered or dropped, with reasons, for human approval.
- Missing hooks become their own changes, each with an example crate.
- Project memory records the decisions; the remaining proposals are rewritten to match.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `053-nutmeg-dogfood-checkpoint`.
