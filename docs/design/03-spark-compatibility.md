# Nutmeg compatibility with GraphFrames in Spark

Nutmeg should accept graph operations from ordinary PySpark clients and return
DataFrames that compose with later Spark operations. The proposed Spark
integration changes both the public API and the Connect protocol. Grustic's
semantic core should survive those changes without being renamed or tied to
a particular generated protobuf package.

**Proposal:** maintain versioned GraphFrames adapters over explicit algorithm
contracts. Make unchanged-client execution an acceptance test. A similarly
named Python method is insufficient evidence of compatibility.

## What is settled and what is provisional

The [SPIP](https://docs.google.com/document/d/1v8OEV5Qqk5LE1XID4ZiPF5rLkAk-Zz6fp0tns9Z_TA8/edit)
remains in discussion. It targets Spark 4.5 subject to acceptance and readiness.
It proposes migration in batches and places new Cypher/GQL support outside
that migration's required scope. Namespace, initial scope and API stability
remain community decisions.

The [POC at 9ba1b8e5](https://github.com/apache/spark/pull/58302) is open and
draft. Its client namespace is `pyspark.graphframes`; its protocol contains
`Relation.graph_frames`. The SPIP also proposes `pyspark.graph`. The POC still
illustrates PageRank returning a GraphFrame, while the proposal discusses a
DataFrame-only return. Pin these as distinct profiles.

The SPIP's property-retention item contains conflicting wording: its explanation
argues for omitting properties, followed by “Do carry additional properties”.
Treat that as unresolved until a chosen reference implements an agreed contract.
Do not quietly infer which sentence is authoritative.

## Compatibility profiles

| Profile | Authority | Client and host obligation |
| --- | --- | --- |
| Existing external GraphFrames | A selected release, source commit and test inventory | Preserve its imports, defaults, result shape, algorithms and Connect encoding within declared scope |
| Spark POC | Exact Spark commit and generated client | Decode its typed graph relation in Sail and preserve observed behavior |
| Proposed core API | The SPIP plus later accepted Spark implementation | Track naming and semantic changes in a new profile; no released-API claim until there is one |
| Nutmeg extensions | Explicit Nutmeg version | Expose additional capability without silently changing a standard operation |

Do not install a package that shadows `pyspark`, mutate `sys.modules`, or patch
a client at runtime to claim the unchanged client works. A temporary Nutmeg
facade can be useful, but its results qualify that facade only.

## Translate requests at the protocol boundary

Sail must recognize the POC's typed relation and translate it to Nutmeg's semantic
request. Existing Nutmeg extension messages take a separate decoder. Both reach
the same engine contracts and diagnostics. Preserve versioned request fields,
input relation identities, expression trees, output schemas and unknown-field
policy. Keep generated Spark types out of Grustic's schema and plan crates.

A server with only an extension-envelope handler cannot automatically run the
new built-in client. The required Sail Connect decoder and planner changes must
be small, independently reviewed and tested against the chosen Spark commit.
Do not infer support merely from protobuf's ability to carry unknown fields.

The initial Sail client route is **PySpark Connect**. Classic PySpark against
Spark's JVM supplies a reference oracle. Directly running a classic JVM client
on Sail would require a distinct bridge and is not established by Connect parity.
Document this distinction for users.

## Preserve the whole DataFrame contract

Graph construction should remain lazy. Carry the input DataFrame plans and
session identity; do not collect all graph rows into Python. Use Spark Column
expressions and their typed plans for predicates, messages and aggregation.
Turning arbitrary expressions into strings loses names, types and semantics.

Results must support later select, join, filter, aggregate, persist/unpersist,
collect and write operations where the reference supports them. Compare field
names, types, nullability, nested metadata and row multiplicity, not just values.
An explicit ordering contract requires ordered comparison; other outputs use
multisets. Preserve intended errors and report deliberate differences.

Keep the Int64 route efficient. Support other admitted ID domains through
adapter mappings when an algorithm needs dense IDs. Preserve original IDs in
results, including empty graphs, isolated vertices, parallel edges and self
loops. A caller requesting minimum-ID component labels must not receive random
representatives merely because they encode the same partition.

## Track each proposed breaking change

| SPIP proposal | Design response and distinguishing check |
| --- | --- |
| PageRank and SVD++ return DataFrames | Result adaptation belongs to the selected profile. Assert actual return type, schema and metadata; preserve any legacy diagnostics through its documented API. |
| Property retention and explicit `join_vertices` | Keep algorithm state separate from source properties internally. The adapter applies the chosen result convention and tests collision, null and join behavior. Resolve the draft's conflicting sentence before claiming parity. |
| Python naming becomes snake_case | Generate or maintain explicit versioned method maps. Test imports and signatures with real clients rather than aliases that mask missing methods. |
| DataFrame algorithms become the default | Record requested algorithm/default in the semantic request. Change implementation only where results and method contract permit it; retain explain and an explicit override. |
| Separate working storage from checkpoint directories | Scope temporary state by session, operation and attempt. Map the public work-directory setting to host-owned resources and test cleanup without deleting unrelated files. |
| Split Pregel and AggregateMessages dispatch | Treat message expression construction and runtime dispatch as separate pieces. Test the public imports and both client modes against their references. |
| Move Hash2Vec to ML | Keep sequence embedding helpers outside the graph core. Track the public move in the profile rather than retaining an accidental graph dependency. |

## Algorithm semantics before implementation substitution

Record a contract for every public algorithm: directedness, duplicate-edge
handling, isolated vertices, initialization, convergence, maximum iterations,
normalization, weights, tie rules, nondeterminism and output schema. BFS path
rows and landmark shortest-path maps are separate contracts. PageRank tolerance
and fixed-step runs are separate, including dangling-mass handling.

Pregel is a reusable primitive only after its active-set, message-combination,
update order, no-message and stopping semantics are fixed. Cypher edge uniqueness
must not be imposed on GraphFrames motif finding without reference evidence.
Tests must include graphs where these differences change the answer.

The complete parity inventory includes construction and validation APIs,
degrees, subgraphs, reversal, triplets/motifs, BFS/all paths/cycles, message
aggregation/Pregel, connected and strongly connected components, shortest paths,
PageRank variants, community algorithms, triangle count, clustering, SVD++,
independent sets, k-core, HyperANF and embeddings. Include PropertyGraphFrame
separately if it survives the agreed Spark scope. Classify every entry as
supported, deliberately different, pending, unsupported or outside that profile.
A three-algorithm milestone must not be labelled full GraphFrames compatibility.

## Qualification and intentional differences

Pin a release/source client, server and dataset for each row. Run the same
public calls on fresh classic Spark and Spark Connect reference sessions, then
on Sail. Keep returned types, complete small-graph answers, public expression
behavior and resource-lifetime tests. Add large tests after correctness.

Sail's engineering requirements still apply. Do not reproduce a known Spark
bug, unsafe retry or weak ownership behavior merely to match an observation.
A deliberate difference needs a source/runtime counterexample, a documented
contract and a test. Missing functionality is not a deliberate improvement.

Valid graph inputs are assumed by the algorithm contract. An explicit validation
API may perform a separate scan; do not add hidden validation jobs to every
algorithm. If a selected profile promises different invalid-input behavior,
record that difference rather than claiming parity.

## Decision before implementation

Approve the compatibility-profile mechanism and unchanged-client requirement.
Select the first external release and POC commit for executable tests. Then
review the Connect translation and result conventions before porting algorithms.
Future namespace changes remain adapter updates; the core retains its own
stable semantic vocabulary.
