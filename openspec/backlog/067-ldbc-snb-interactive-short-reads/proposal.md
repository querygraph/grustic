# Proposal: LDBC SNB Interactive short reads

## Why

The short reads exercise point lookups and one- and two-hop patterns. They are the first complete workload to pass end to end.

## What Changes

- Interactive short reads IS1 to IS7 as Cypher text, run against the validation dataset on the reference backend.
- Every query passes, or the change is not complete.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `ldbc-snb-conformance`: Adds the Interactive short-read workload.

## Impact

- **Crates:** `grustic-ldbc`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `066-ldbc-snb-validation-harness`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
