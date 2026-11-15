# Glossary

Terms used across Grustic specs, code and discussion. When a term has a
normative definition in `openspec/specs/`, the spec's definition wins.

- **LPG**: labeled property graph. Vertices and edges carry labels and
  properties.
- **Schema group**: one owning vertex or edge population. Labels and
  inheritance describe meaning; a group is where entities live.
- **Entity identity**: a reference made of graph identity, entity kind and an
  opaque value in a declared namespace. It is not the same thing as a
  projection position.
- **Projection position**: a dense index (for example into CSR arrays) that a
  backend assigns for one projection. It never becomes a durable identity.
- **Catalog snapshot**: a fixed schema revision, data revision and identity
  mapping that a plan is resolved against.
- **Unresolved, resolved, physical plan**: the plan phases. Resolved plans are
  fully typed; physical plans add conventions and physical properties.
- **Extension node**: a plan node supplied by a third party through the
  extension contract. Rules that do not declare they understand it treat it as
  a barrier.
- **Convention**: an execution representation and location, such as
  relational batches, indexed adjacency or a local CSR projection.
- **Physical property**: something an operator requires from or provides to
  its inputs, such as ordering, partitioning or adjacency sort order.
- **Conversion**: a costed operation that establishes a convention or
  property, such as building or re-sorting a CSR projection.
- **CSR**: compressed sparse row adjacency.
- **Refusal**: a typed, intentional "not supported" outcome. It is distinct
  from an error and does not complete a feature.
- **Reference backend**: `grustic-reference`, the arrow-rs executor that puts
  correctness over speed.
- **Nutmeg**: the Sail backend and first consumer, in a separate repository.
- **Sail**: the Rust engine, built on DataFusion, that serves Spark clients.
- **Human-scoped / agent-scoped**: who owns a file. See `AGENTS.md`.
- **Backlog**: the numbered draft proposals in `openspec/backlog/`. A draft
  becomes an active change when its pull request moves it to
  `openspec/changes/`. Drafts after the plan review are provisional.
- **Design goals G1–G3**: extension scenarios that guide hook design; see
  [design-goals.md](design-goals.md).
