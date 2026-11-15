# Proposal: Physical plan and backend contract

## Why

Backends need a plan that says how operations execute, not only what they mean. A minimal physical contract lets the reference backend and Nutmeg start before the full optimizer exists.

## What Changes

- Physical plan nodes for every resolved node, each tagged with a convention.
- A backend contract: register conventions and implementations for logical node kinds, including extension nodes, and function implementations.
- Direct lowering: each logical node maps to a registered implementation. A missing implementation is a typed refusal naming the node or function and the backend. Nothing falls back silently.
- Session planning now produces physical plans.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `physical-plan`: Adds the behavior requirements for the contract fixed by the foundation.
- `session`: Planning produces physical plans for a registered backend.

## Impact

- **Crates:** `grustic-plan`, `grustic-session`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `025-foundation-physical-plan-api`, `047-session-state-and-planning-entry`, `039-logical-extension-nodes`.
