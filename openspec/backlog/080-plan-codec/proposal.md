# Proposal: Plan codec

## Why

Distributed backends must send plans, including extension nodes, to workers. This needs a serialization mechanism, though not a format that is stable across releases.

## What Changes

- A plan codec with an envelope carrying extension name and version.
- An extension codec contract for extension nodes.
- Round-trip tests including extension nodes. An unknown or mismatched extension is refused before execution.
- No cross-release stability is promised.

## Capabilities

### New Capabilities

- `plan-codec`: Serialization of physical plans including extension nodes.

### Modified Capabilities

None.

## Impact

- **Crates:** `grustic-plan`, `grustic-session`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `054-dogfood-plan-review`, `048-physical-plan-contract`, `039-logical-extension-nodes`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
