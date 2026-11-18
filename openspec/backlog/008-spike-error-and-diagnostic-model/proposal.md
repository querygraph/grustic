# Proposal: Spike: error and diagnostic model

## Why

Typed errors, refusals and diagnostics cross every crate and every extension boundary. Getting their shape wrong forces breaking changes everywhere later.

## What Changes

- Candidates: per-crate error enums with shared refusal and diagnostic types; one shared error crate; ways to carry extension errors (namespaced codes with boxed sources, or generic parameters).
- A diagnostic type with spans, stable codes and structured locations for programmatic callers.
- Tested on the plan and parser spikes' code.

## Hypotheses

- Per-crate enums with shared refusal and diagnostic types keep errors typed without a cross-crate catch-all enum.
- Namespaced codes with boxed sources are enough for extension errors.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `006-spike-plan-representation`, `007-spike-parser-approach`.
- **Decision informed:** Error, refusal and diagnostic contracts.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
