# Proposal: Nutmeg dogfood checkpoint

## Why

Nutmeg on Sail is the first consumer and must use only public extension points. Dogfooding right after the foundation, before most of the plan is built on it, turns every misfit into a contract change while that is still cheap.

## What Changes

- Nutmeg, at a pinned revision in its own repository, builds against this commit's public crates with no patches, private items or forks.
- It registers Sail conventions, implementations for the vertical-slice nodes and one function implementation through the public backend contract, plans the vertical-slice queries through the session, and executes them in Sail over Arrow data mapped by `grustic-arrow`.
- Its answers equal the minimal second backend's answers on the same queries.
- It implements one Sail rule, one physical property and one conversion against the foundation traits, to check that they fit, even though the planner does not apply them yet.
- Each missing hook or misfit is recorded for the plan review. This change adds no hooks.
- Project memory lists the extension points Nutmeg uses.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (cross-repository check).
- **Human-scoped files:** `.github/workflows/` (maintainer-authored: agents may not write `.github/`, so the agent proposes the workflow text in the pull request), for the cross-repository build job.
- **Unsafe code:** None expected.
- **Depends on:** `050-minimal-second-backend`, `052-arrow-dependency-floors`.
- **Note:** Nutmeg code changes belong to the Nutmeg repository.
