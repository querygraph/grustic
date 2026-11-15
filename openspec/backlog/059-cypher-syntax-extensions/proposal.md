# Proposal: Cypher syntax extensions

## Why

Third parties must be able to add syntax, such as a temporal path form, without forking the parser.

## What Changes

- Fixed hook points: clause start, pattern element and expression prefix.
- Extensions declare contextual keywords; conflicting claims at one hook point are a session build error.
- Extension AST nodes carry name, span and children, and report through the standard diagnostics.
- Verified with test-only extensions.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `cypher-syntax-extensions`: Adds the behavior requirements for the contract fixed by the foundation.

## Impact

- **Crates:** `grustic-cypher`, `grustic-session`.
- **Human-scoped files:** `src/api/` and `public-api.txt` of both crates.
- **Unsafe code:** None expected.
- **Depends on:** `054-dogfood-plan-review`, `042-cypher-diagnostics-and-recovery`, `047-session-state-and-planning-entry`.
- **Note:** Provisional: written before the foundation and Nutmeg dogfood, and revised by the plan review before work starts.
