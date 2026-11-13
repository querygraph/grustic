# Review: can Grust v2 become a Calcite for graph queries?

**Yes, the separation is sound; the current executable MVP needs a selective
redesign before its interfaces become Grustic's public foundation.** The useful
assets are semantic contracts, independently checked answers and replaceable
frontends. The main danger is stabilizing an experimental Sail translator as
the supposedly general graph model.

Grustic should let an application use only schema reasoning, only a parser,
only planning, or an assembled engine. Nutmeg is the Sail assembly and graph
algorithm product. Its performance ambition needs evidence for complete work,
including projection construction and results, on matched execution contracts.

## What the existing work establishes

Sem's [PR 36 clarification](https://github.com/querygraph/grust/pull/36#issuecomment-6000697247)
explicitly separates the toolkit, assembled engine and algorithm layer. Wave 2
and Wave 3 largely follow this dependency direction. Logical types and schema
objects do not require a running engine. A parser-free builder and a function
registry exist. The resolver retains typed slots, graph identity, nullability,
relationship uniqueness and property provenance. Statistics are optional and
unknown estimates differ from zero. Refusals are explicit.

The later executable subset has meaningful evidence. Its complex-SNB gate
records 860 successful returned-result cells and eight expected arithmetic
errors, with 18 capability refusals recorded separately. It includes unchanged
IC2, IC8 and IC9, five short-query families and independent CSV answers.
Synthetic 100-fold replication reaches about one million vertices but preserves
the original disconnected workload. It is not an official LDBC scale factor
or evidence for difficult connected high-degree graphs.

The same report preserves optimized/resolved median ratios up to 1.554 on
those larger controls. That establishes a cost-model calibration problem to
investigate, not its cause. A separate sampled RSS control is useful evidence
for that native process and fixture; it does not establish a general memory
bound. The [source inventory](../review/sources.md) links the exact receipts.

Cosmolang also progressed beyond a proposal: the live slice uses actual
Cosmograph, Sail and Nutmeg. It qualifies a bounded two-level exploration
prototype. Its caps, supplied layout and cooperative deadlines are explicit.
Port the useful semantics; do not inherit its limited deployment assumptions.

## What the Calcite analogy requires

Calcite exposes rules, metadata and execution conventions so adapters can
supply equivalent implementations and account for conversions. Its adapter
contract is broader than SQL string generation. DataFusion similarly permits
custom logical and physical operators. These are useful architectural models,
not a requirement to reproduce their entire frameworks. See the
[Calcite](https://calcite.apache.org/docs/adapter.html) and
[DataFusion](https://datafusion.apache.org/library-user-guide/extending-operators.html)
contracts.

For Grustic, the practical tests are whether a third party can add an operator,
use a different resolver or statistics provider, and execute the same admitted
semantics without Sail. Shipping several crates alone cannot prove that.

## Findings that should shape the port

| Priority | Source observation | Consequence and recommendation |
| --- | --- | --- |
| Before public API | `resolved-plan/src/lib.rs` retains the initial sketch while `src/query.rs` defines a second executable plan. Other crates also retain parallel sketch/executable APIs. | Export one canonical contract for each phase. Keep old sketches in Grust history; do not copy both APIs into Grustic. |
| Before public API | Executable `Expr` has an optional logical type and public fields; executable `Op` is a closed enum. | Use an explicit null-only type and checked constructors for admitted plans. Retain readable enums for built-ins, with a documented extension node and typed construction path. Test malformed plans supplied by third-party code. |
| Before type compatibility | `LogicalType` has lists and structs but no built-in map type or independent list-element nullability. The Cypher adapter returns a single parse diagnostic. | Define map/key/value and nested-null semantics needed by Spark results; implement the requested multi-error frontend diagnostics. A vector in a report type alone does not establish recovery. |
| Before physical planning | The resolver expands fixed patterns into schema paths and joins; bounded ranged paths become unions, with an eight-hop SQL-provider threshold. | Preserve graph operations until implementation selection. Bound planning work and factor schema alternatives. A compiler threshold must not become the graph language's semantics. |
| Before plugin API | `RelationProviders::lower` expands a plugin into existing unresolved relations. It cannot independently add a new physical operator with distribution and cost properties. | Keep expansion providers and add a separate implementation-provider contract. A custom traversal or algorithm must not require editing every core match statement. |
| Before optimization claims | Join DP explores pure inner regions, capped at ten relations, with row/NDV estimates and a simple weighted row cost. It retains no alternative execution convention or distribution requirement. | Keep it as a bounded strategy. Add costed conversions, memory feasibility, statistics provenance and required physical properties before choosing CSR, adjacency or distributed implementations. |
| Before generalized execution | SQL is generated with adapter-specific numeric handling; the Python runtime owns repeated materializations and traversal steps. | Preserve SQL as one adapter and a regression control. Move production execution ownership to Sail. Keep dialect coercion/error policy explicit. |
| Before identity compatibility | The logical schema separates identity from property keys, but the demonstrated Sail graph route uses BIGINT identities. | Keep that fast path. Define adapter-owned mappings for other admitted GraphFrames ID types and preserve output IDs exactly. Dense projection positions must never become durable IDs. |
| Before Spark compatibility | The new POC uses a typed Connect relation while existing Nutmeg uses an extension envelope. Proposed Python names and returns differ from the POC. | Implement independent versioned translators to the same semantic requests. Tests must use the actual pinned client without monkeypatches. |
| Before native migration | Comet work qualifies scalar components. It supplies no relation, staging or iterative algorithm ABI. | Keep the existing relation route. Qualify package loading and workers for the new scalar route separately. |
| Before pipeline composition | The local recovery redesign has a narrower execution/failure model than the pushed SDP product. | Gate an actual combined host. Publish graph snapshots through Sail's authority and record complete computation identities. |
| Before browser scale claims | Cosmolang C1 scans bounded Parquet and uses supplied two-level groups and coordinates. | Add indexed subset access, hierarchy provenance and versioned artifacts before large-graph claims. Results on a quotient graph must be identified as such. |

These are architecture risks, not assertions that every current result is
wrong. Source locations are under `wave-3/sketch` at the complex-query revision
in [the inventory](../review/sources.md). The public constructors and closed
node set are directly observable. Their long-term costs are this review's
inference and should be tested with independent consumers.

## Choices that would pull the project in the wrong direction

**Making Sail types the graph foundation.** It would simplify the first
integration but force schema-only users to inherit engine versions and runtime
concepts. Keep Arrow transport, DataFusion execution and Spark protocol types
in adapters. Logical type conversion should be explicit and tested.

**Rebuilding a complete distributed engine inside Grustic.** Sail already owns
scheduling, shuffles, memory pools and query cancellation. Grustic should supply
graph semantics and implementation alternatives. A separate scheduler or
pipeline checkpoint authority would duplicate the hardest ownership problems.

**Making every graph operation construct CSR.** Compressed sparse row storage
is a compact adjacency representation, useful when the admitted graph fits
and repeated calls amortize construction. Filtering a few neighbors in a huge
indexed graph has a different access pattern. Include scan and indexed routes;
make gather, decoding, ID mapping and reverse-adjacency costs visible.

**Treating all path-like APIs as the same operation.** Cypher TRAIL matching,
GraphFrames motifs, BFS outputs, all-shortest paths and landmark distances have
different multiplicity and termination contracts. Share primitives only after
those contracts are represented. Never silently substitute an easier method.

**Freezing a speculative universal ABI or schema.** Preserve room for features,
timestamps, richer IDs and external formats without claiming frozen LEX/GQL
conformance. Keep C ABI concerns at the host/plugin boundary. Ordinary Rust
users should not pay for a universal plugin runtime.

**Growing abstractions faster than independent users.** A general memo optimizer,
e-graph engine, schema language, full GQL parser and every graph algorithm are
separate undertakings. Demand a second concrete consumer before adding broad
interfaces. Bounded planning and good errors are useful before general search.

## Recommendation and review decisions

Adopt the three-layer direction. Retain semantic tests and reimplement the
contracts that carry prototype assumptions. The first complete use case should
exercise DataFrame composition, native projection ownership and Cosmograph
against one immutable graph snapshot. Require a second lightweight backend
and an external operator example to demonstrate the modularity.

Human review should settle the public phase boundaries, identity model,
extension-node contract, Spark compatibility profiles and Sail execution
ownership before their implementation commits. The next documents propose
those choices and the tests that would distinguish success from failure.
They do not claim those choices are already accepted.
