# Proposal: Spike: plan representation and rewrites

## Why

Plans are rewritten many times by rules and extended by third parties. The representation decides how easy rules are to write, how expensive rewrites are, and whether invariants can be enforced by construction.

## What Changes

- Candidates: enums with one extension variant in an `Arc` tree with a TreeNode-style transform API; an arena with index-based nodes; trait objects for every node; and, as a stretch, an e-graph.
- The same small node set in each, with three rules (predicate pushdown, projection pruning, inserting a conversion for a required property), one extension node, and checked construction.
- Measurements: rule code size and clarity, rewrite cost on large plans, whether invariants can stay private, clone cost and ease of serialization.

## Hypotheses

- Enums plus an extension variant in an `Arc` tree with checked constructors gives the shortest rules and allows invariants to be enforced.
- An arena adds complexity with no measurable benefit at expected plan sizes.
- E-graphs are premature for Grustic's first optimizer.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None. This change alters no spec-level behavior; its `.openspec.yaml` sets `skip_specs: true`.

## Impact

- **Crates:** none (spike).
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `002-prior-art-review`.
- **Decision informed:** Plan representation and the rule API shape.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
