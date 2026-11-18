# Proposal: Cypher parser robustness

## Why

The parser takes untrusted text from tools and users. Input-driven paths must never panic, and diagnostics must stay accurate on arbitrary input, which example-based tests cannot show.

## What Changes

- Property tests on the stable toolchain: for any input the parser returns an AST or diagnostics without panicking, every span lies within the input, and an invalid partial AST is never lowerable.
- Fuzz targets for the lexer and parser, run outside CI on a pinned nightly toolchain; every crash found becomes a regression test.
- Nesting beyond a configured depth is refused with a typed diagnostic instead of exhausting the stack.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `cypher-diagnostics`: Adds robustness guarantees for arbitrary input.

## Impact

- **Crates:** `grustic-cypher`.
- **Human-scoped files:** none expected.
- **Unsafe code:** None expected.
- **Depends on:** `042-cypher-diagnostics-and-recovery`.
