# Proposal: Session state and planning entry point

## Why

Applications need one object that holds catalogs, registries, extensions and configuration, and turns a query into a plan. Grust v2 had no session; a Python runner wired the pieces together.

## What Changes

- In `grustic-session`: a state builder registering catalogs, functions and configuration, with slots for extensions, rules and a backend added by later changes.
- Build-time validation of conflicting registrations with typed errors.
- Planning entry point: query text or builder plan, then snapshot collection, then a resolved plan.
- Strict typed configuration: unknown keys are errors.
- The Cypher frontend is an optional feature. With it disabled, the session accepts builder plans only and does not depend on the parser.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `session`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-session`.
- **Human-scoped files:** `grustic-session/Cargo.toml` for the optional Cypher feature. Otherwise the public surface comes from the foundation changes.
- **Unsafe code:** None expected.
- **Depends on:** `027-foundation-session-and-backend-api`, `045-resolve-basic-patterns`, `044-cypher-to-unresolved-plan`, `046-cypher-builtin-functions`.
