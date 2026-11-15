# Proposal: Vertical slice on a minimal second backend

## Why

The backend contract must not take the shape of whichever backend is written first. A deliberately different, minimal backend runs the first queries end to end before the reference backend exists, so the session, resolver and physical contracts meet a real consumer early, as the first-use-case plan asks.

## What Changes

- A row-at-a-time backend under `examples/`, not published, that implements node scan, fixed-length expand, filter, projection and limit through the public backend contract only.
- A small in-memory graph source served through the public catalog contract.
- The same queries built with the Rust builder and written as Cypher text resolve to equal plans and return equal results, compared with hand-computed answers.
- A node without an implementation in this backend is refused with the typed missing-implementation error.
- Any contract change the slice needs is made through a MODIFIED delta with explicit approval, never worked around.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `physical-plan`: Adds scenarios for a backend registered only through the public contract.

## Impact

- **Crates:** none (example crate under `examples/`).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `049-plan-explain`.
