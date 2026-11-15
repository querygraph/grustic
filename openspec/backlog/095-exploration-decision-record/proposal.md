# Proposal: Exploration decision record

## Why

The exploration spikes are only useful once their evidence becomes decisions that humans accept, including where Cosmolang and its Cosmograph adapter live.

## What Changes

- For each exploration spike: the decision, its evidence (the archived spike change), rejected alternatives and remaining risks.
- Where Cosmolang and the Cosmograph adapter live: a Grustic crate, part of Nutmeg, or their own repository.
- Draft proposals for the exploration work that follows, each sized for review, plus any Grustic hooks it needs as separate changes.
- Project memory records the decisions; `spikes/` code from these spikes is removed.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `094-spike-cosmograph`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
