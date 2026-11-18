# Proposal: Rust conventions in project memory

## Why

The previous instructions pointed to a Rust guide outside the repository. The project must be fully described by specs, code and project memory, so the conventions contributors follow need to live in the repository.

## What Changes

- Add `.agents/project-memory/rust-conventions.md`, adapted from the QueryGraph Rust guide: domain types, error design, module layout, test layout and naming.
- Link it from the project-memory index and the architecture map's crate layout section.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none.
- **Human-scoped files:** none (project memory is agent-maintained).
- **Unsafe code:** None expected.
- **Depends on:** none.
- **Note:** A maintainer must supply the guide text. Agents must not reconstruct it from memory.
