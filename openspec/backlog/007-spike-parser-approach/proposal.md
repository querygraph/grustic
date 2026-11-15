# Proposal: Spike: parser approach

## Why

Syntax extension hook points and multi-error recovery are requirements that some parser approaches make hard. The parser is also a standalone product for tools, so speed and error quality matter.

## What Changes

- Candidates: a port of grust-cypher's parser; a new hand-written recursive-descent parser with Pratt expressions; a combinator library such as chumsky or winnow; a generator such as lalrpop or pest.
- Each implements the same slice: `MATCH`, `WHERE`, `RETURN`, expressions and one extension hook point.
- Measurements: recovery quality on a corpus of broken queries, span accuracy, parse speed on LDBC queries, compile time and licensing.

## Hypotheses

- Hand-written recursive descent with Pratt expressions gives the clearest hook points and the best recovery.
- Parser generators make runtime extension hooks impractical.

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
- **Decision informed:** Parser technology, and whether grust-cypher code is ported or rewritten.
- **Note:** Spike code lives in `spikes/<change-id>/`, outside the product workspace members. Product crates never import it, and the decision record removes it.
