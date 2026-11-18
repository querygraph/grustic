# Design pitfalls

Choices that look simpler but would pull Grustic away from being a reusable,
engine-free graph foundation. Each entry states the pitfall and why it
matters. Requirements in `openspec/specs/` win over anything here.

## Architecture

- **Making Sail or DataFusion types the graph foundation.** It simplifies the
  first integration but forces schema-only and parser-only users to inherit
  engine versions and runtime concepts. Engine types live in adapters.
- **Using an engine's logical plan as the core plan.** DataFusion's
  `LogicalPlan` with extension nodes would couple every consumer to DataFusion
  releases. A modest graph-aware plan plus adapters costs one extra
  translation, which the minimal second backend and Nutmeg must justify.
- **Lowering graph queries to SQL as the canonical route.** SQL erases graph
  alternatives (joins, indexed adjacency, projections, kernels) before the
  planner can choose among them. Grustic has no SQL-generation adapter: Sail
  is reached through Nutmeg's public extensions, and the minimal second
  backend keeps the backend contract honest.
- **Rebuilding a distributed engine inside Grustic.** Hosts own scheduling,
  shuffles, memory pools and cancellation. Grustic describes partitioning and
  serializes plans; it never schedules or keeps a second checkpoint authority.
- **Growing abstractions faster than independent users.** A general memo
  optimizer, e-graphs, a schema language, full GQL and every algorithm are
  separate undertakings. Demand a second concrete consumer before adding a
  broad interface.
- **Freezing a universal ABI or format.** Do not promise a stable serialized
  enum or a stable Rust trait ABI. In-process extensions are compiled together;
  dynamic native libraries use the host's own, separately versioned boundary.
  Claim no conformance to evolving formats such as LEX or GraphAr because a
  type name looks similar.

## Plans and semantics

- **Treating all path-like operations as one.** Cypher trail matching,
  GraphFrames motifs, BFS outputs, all shortest paths and landmark distances
  have different multiplicity and termination contracts. Reachability may
  track only visited endpoints; returning trails must keep edge sequences.
  Share primitives only after each contract is represented.
- **Making a compiler limit part of the language.** Grust v2 expanded
  variable-length paths into unions with an eight-hop threshold. Resource limits
  are planning or execution refusals, never semantics.
- **Inferring physical ordering from a logical sort.** A later
  materialization may not preserve it. Carry order as a physical property
  where correctness needs it.
- **Silently substituting an easier method.** A missing implementation is a
  typed refusal; nothing falls back to another backend or a cheaper algorithm.
- **Assuming the host keeps a proposed join tree.** A host may rewrite it.
  Record both Grustic's plan and the host's actual plan.

## Identity and caching

- **Confusing source identity with projection positions.** Dense positions
  are per projection and never become durable identities. 32-bit positions are
  admitted only when bounds permit.
- **Losing identity precision at JSON boundaries.** Browser and protocol
  clients receive identities as lossless strings or binary values, including
  integers beyond JavaScript's exact range.
- **Under-keyed caches.** A cache key includes snapshot identities,
  parameters, semantics profile, implementation version and authorization
  scope. A fresh schema alone does not make old data or an old view current.

## Execution and resources

- **Building CSR for every operation.** CSR pays off when the graph fits and
  repeated calls amortize construction. A selective traversal on a huge
  indexed graph has a different access pattern. Keep scan and indexed routes,
  and make gather, decoding, identity mapping and reverse-adjacency costs
  visible.
- **Treating a batch algorithm as incremental.** Streaming updates,
  retractions and incremental algorithms need their own contracts; calling a
  batch kernel in a loop does not provide them.
- **Hidden validation.** Inputs satisfy their declared contract. Validation is
  an explicit operation, kept outside default execution and outside algorithm
  timers.
