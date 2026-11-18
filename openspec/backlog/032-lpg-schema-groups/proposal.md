# Proposal: LPG schema groups

## Why

An LPG schema must describe vertex and edge populations, labels, inheritance and properties without any storage or engine. It is the first independently useful piece: a schema-only consumer.

## What Changes

- In `grustic-schema`: schema groups with one owning population each, labels and label inheritance, multi-label entities, typed and optional property keys, key constraints and edge endpoint constraints.
- Free schema checks with no data access: feasibility of label and property combinations, and edge endpoint compatibility.
- Schema validation with typed errors.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `lpg-schema`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-schema`.
- **Human-scoped files:** none expected. The public surface comes from the foundation changes. If implementation shows it is wrong, change it here through a MODIFIED delta, with explicit maintainer approval.
- **Unsafe code:** None expected.
- **Depends on:** `022-foundation-schema-and-catalog-api`, `030-nested-and-graph-value-types`, `031-entity-identity`.
- **Ported from:** Grust v2 Wave 2 schema: multi-label, inheritance and optional property keys. Source: `querygraph/grust` at `60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7`, the necessary `wave-2/sketch` contracts under `docs/reviews/grust-v2/`.
