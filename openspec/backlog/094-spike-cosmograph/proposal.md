# Proposal: Spike: Cosmograph rendering envelope

## Why

Cosmograph is the browser renderer for Cosmolang views. The C1 slice used the actual Cosmograph SDK with software WebGL and a supplied layout, which proves functional integration only. Large-graph exploration needs measured browser limits before any envelope is promised.

## What Changes

- The actual pinned Cosmograph SDK in a real browser, fed by the Cosmolang spike's views.
- Arrow field mapping from view results to the SDK's inputs, including identities, group membership and layout coordinates.
- Follow, expand and collapse round trips, with view installation acknowledged by the browser separately from resource creation.
- Measurements at increasing node and edge counts, up to the configured million-node ceiling: request-to-installed-frame latency, payload bytes, and browser and GPU memory, on GPU hardware. Software-renderer runs are labeled as functional only.
- Prefetch of neighboring groups as a budgeted speculative resource: useful-prefetch fraction and wasted bytes and work, with foreground requests always ahead of speculation.
- 2D only. 3D needs an explicitly selected renderer capability.

## Hypotheses

- Arrow IPC results reach the SDK with no per-row conversion.
- Edge and byte limits bind before the vertex limit on dense views.
- A million-node view fits the envelope on the target hardware. This is a ceiling to qualify, not an observed capability.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `093-spike-cosmolang`.
- **Decision informed:** The browser envelope, the view result format, and whether prefetch is worth a policy.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the exploration decision record removes it. TypeScript spike code gets type and build checks. Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
