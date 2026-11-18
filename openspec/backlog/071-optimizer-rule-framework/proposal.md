# Proposal: Optimizer rule framework

## Why

Rewrites must be pluggable so that backends, including Nutmeg's Sail-specific rules, and third-party extensions can add them. Their legality must not depend on registration order.

## What Changes

- In `grustic-optimizer`: a rule contract with a phase (analyze, logical, physical) and the node kinds it understands.
- Extension nodes are barriers to rules that do not claim them.
- Ordering by before and after constraints between named rules; a cycle is a session build error.
- Bounded fixpoint application, with every applied rule recorded.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `optimizer-rules`: Adds the behavior requirements for the contract fixed by the foundation.
- `session`: Sessions register optimizer rules.

## Impact

- **Crates:** `grustic-optimizer`, `grustic-session`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `054-dogfood-plan-review`, `025-foundation-physical-plan-api`, `048-physical-plan-contract`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
