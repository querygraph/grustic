# Architecture map

Grustic is a set of independently usable crates: an engine-free LPG model,
Cypher frontend, resolver and planner, a reference backend on arrow-rs, and
adapters. Consumers can take any subset and extend each phase. This map gives
the intended components and their dependency direction.

**Current state:** no crates exist yet. Every component below is planned. When
a change creates or alters a component, it updates its row and links the
capabilities in `openspec/specs/` that govern it.

## Pipeline

```text
Cypher text ──> grustic-cypher ──┐
Rust builder ─> grustic-plan ────┼─> unresolved plan
                                 │
          grustic-schema ────────┤   (catalog snapshot)
          grustic-functions ─────┤   (function registry)
                                 v
                       grustic-resolver ──> resolved, typed logical plan
                                 │
                       grustic-optimizer ──> physical plan
                                 │            (conventions, properties)
                    ┌────────────┴────────────┐
           grustic-reference             Nutmeg adapter
           (arrow-rs executor)           (Sail, separate repository)
```

`grustic-session` holds catalogs, registries, rules, configuration and the
selected backend, and drives the pipeline. Planning ends at a physical plan;
backends execute.

## Components

| Component | Responsibility | Depends on | Must not depend on |
| --- | --- | --- | --- |
| `grustic-types` | Logical values and types, entity identities | — | Arrow, DataFusion, engines |
| `grustic-schema` | LPG schema groups, labels, catalog provider and entry kinds | types | Arrow, DataFusion, engines |
| `grustic-functions` | Semantic function signatures, null behavior, volatility | types | Backend implementations |
| `grustic-plan` | Unresolved, resolved and physical plan contracts; extension node contract; physical properties | types, schema, functions | Parser, resolver, optimizer |
| `grustic-cypher` | Lexer, parser, AST, diagnostics, syntax extension hooks | plan | Engines |
| `grustic-resolver` | Typed resolution, including extension AST resolvers | plan, schema, functions | Engines, data scans |
| `grustic-optimizer` | Rule phases, property enforcement, bounded search | plan | Engines, statistics collection |
| `grustic-session` | Session state builder, registries, plan codec; Cypher text input behind an optional feature | core crates (`grustic-cypher` only with that feature) | Arrow, DataFusion, engines |
| `grustic-kernels` | Projection contract, algorithm semantics variants, host executor and output ownership | types, schema, plan | Arrow, DataFusion, engines, global thread pools |
| `grustic-arrow` | Logical type to Arrow mapping, identity encoding | types, arrow-rs | DataFusion, Sail |
| `grustic-reference` | Reference backend: correctness-first arrow-rs executor | core crates, grustic-arrow | DataFusion, Sail |
| `grustic-ldbc` | LDBC SNB and Graphalytics loaders, runners, reports and benchmarks (test tooling, not a library for consumers) | reference backend, core crates | — |
| Minimal second backend (`examples/`) | Row-at-a-time backend that keeps the backend contract honest; unpublished | public Grustic crates only | Private Grustic items, Arrow |
| Nutmeg (external) | Sail backend, Sail-specific rules and conventions | public Grustic crates only | Private Grustic items |

Dependencies point toward contracts. Backends depend on plan types, never on
the concrete resolver or optimizer.

## Extension points

| Phase | Hook | Lives in |
| --- | --- | --- |
| Syntax | Parser hook points producing extension AST nodes | `grustic-cypher` |
| Resolution | Resolver for extension AST nodes | `grustic-resolver` |
| Functions | Semantic definition; implementation registered per backend | `grustic-functions`, backends |
| Catalog | Graph sources and extension entry kinds | `grustic-schema` |
| Logical plan | Extension node with validated rebuild | `grustic-plan` |
| Rules | Analyzer, logical and physical rules with ordering constraints | `grustic-optimizer` |
| Physical | Extensible properties, conventions, conversions, implementations | `grustic-plan` |
| Distribution | Partitioning properties, extension codec | `grustic-plan`, `grustic-session` |
| Backend | Capability registration and execution | backend crates |

## Crate layout

Each crate separates its human-scoped public surface from agent-scoped
implementation:

```text
crates/<name>/
  Cargo.toml         human-scoped
  public-api.txt     human-scoped snapshot of the public surface
  src/lib.rs         human-scoped: crate docs, lints, modules, re-exports
  src/api/           human-scoped: public traits, types, errors, constants
  src/<modules>      agent-scoped implementation, private or pub(crate)
  tests/             agent-scoped tests, named after requirements and scenarios
```
