# Proposal: Logical extension nodes

## Why

Third parties must be able to add logical operators, such as a temporal path, without editing core enums. Grust v2's closed operator enum only allowed expanding a plugin into existing nodes.

## What Changes

- One extension variant in the resolved plan that holds an extension node.
- The extension node contract: name and version, children, expressions, output schema, effects, logical properties, equality and hashing, and explain data.
- Rebuilding a node with new children re-runs its checks.
- Expansion providers that lower a convenience node into existing nodes remain available.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `logical-extension-nodes`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-plan`.
- **Human-scoped files:** `grustic-plan/src/api/`, `public-api.txt`.
- **Unsafe code:** None expected.
- **Depends on:** `038-resolved-logical-plan-contract`.
