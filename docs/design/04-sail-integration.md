# Nutmeg execution through Sail's extension and pipeline contracts

Nutmeg should execute graph operations inside Sail while returning ordinary
composable DataFrames. The proposed ownership puts task execution, memory
admission, cancellation and publication in Sail. Grustic supplies semantic plans
and graph implementations. Current native interfaces only partly provide this.

**Proposal:** preserve the working native relation route, add the Comet scalar
route explicitly, and integrate both with the chosen Sail runtime in separate
reviewable increments. Neither scalar interoperability nor a passing standalone
pipeline proves their combined behavior.

## The actual starting points

The [source inventory](../review/sources.md) pins three distinct references.
`99dc47788` is the pushed SDP product with the working extension host and a
bounded microbatch runtime for streaming tables and AutoCDC. Maintainers declined
that runtime; it remains the batch compatibility control, not the continuous
foundation. The repaired scalar component is local-only `bfdecbcf9`. The local
append-recovery reference is `37837d20a`, based on upstream `b52144955`. These
are not one qualified combined host. P5 `ef7286ca2` is an earlier prototype.

On the product reference, Python entry points in `pysail.extensions` discover
packages. Manifests are checked before binding native objects; host identity
comes from `Cargo.lock` at build time. Nutmeg pins DataFusion 55.2.0 and Arrow
59.2.0, and its vendored graph adapter uses Grust 0.24.1. A Grustic adapter that
crosses the same DataFusion FFI must inherit the selected host's exact pins.
Its published dependency floors must also allow those versions: a floor above
Arrow 59.2.0 or DataFusion 55.2.0 would prevent this integration even if a workspace
lockfile passed. Test the published adapter dependency closure outside the
workspace. Do not import these engine dependencies into the logical core.

The existing native relation is a session-owned region placed on the driver
in cluster mode, or the local session process in local mode. Host inputs and
frozen plan identity are explicit. Retry is disabled for that region; an
unacknowledged mutation can be indeterminate. The new design must retain that
outcome until a separate replay protocol is implemented and verified.

Product extension tests called `process-cluster` actually used an in-process
actor cluster. This tree lacks the separate-process worker launcher from the
earlier extensions branch. Distributed sessions refuse non-driver extensions
that declare relations. A worker loader in source does not establish that code,
configuration and ownership cross a process boundary correctly.

Capitola's reply confirms the owner directions: continuous StreamSource
execution, the additional C scalar boundary independent of streaming,
commit-time rejection of obsolete writers for every admitted durable target,
and strict typed configuration. The detailed host mechanisms remain proposals
for maintainers. Design agreement precedes dependent host changes.
The future continuous host is not selected: memo decisions 1 and 4 on execution
ownership and native interfaces remain unanswered, as does the operator-isolation
choice. Keep that host an explicit prerequisite, not an implied existing target.

## Keep three native boundaries distinct

| Boundary | Work it carries | Migration requirement |
| --- | --- | --- |
| Existing DataFusion FFI | Native relation planning/execution and existing scalar packages | Preserve exact build admission, package identity, driver placement, input binding, cleanup and errors |
| Comet Arrow C scalar interface | Trusted immutable scalar kernels over Arrow fields and arrays | Pin producer/layout; add explicit per-export selection, package loading and worker identity |
| Native memory lease | Current driver-native quota retained while allocations/results need it | Preserve ownership; scalar/worker admission and per-job producer completion require new contracts |

Comet's scalar interface does not define table providers, async streams,
projection handles or algorithms. A C relation boundary remains a separate
proposal. Inspect the actual Sedona provider/execution interfaces and dependency
closure before choosing attributed host-owned adapters or a pinned dependency.
Do not construct a new general C ABI solely to make the crate names uniform.

For mixed packages, relaxing version checks for the scalar export must not
relax checks for its DataFusion-FFI relation export. The selected boundary,
content digest, normalized configuration and semantic function identity travel
with a serialized plan. Workers resolve that identity or fail before execution.
A duplicate export name cannot silently choose a different implementation.

## Preserve Arrow ownership and kernel semantics

The pinned Comet contract admits trusted pure immutable functions. Its
constant-argument initialization pointer must be null. Preserve complete fields,
including nested metadata and nullability, instead of checking only data types.
Qualify empty and sliced arrays, dictionaries, unions, run-end encoding, nested
values, nulls, errors and returned-array lifetime where applicable.

Code containing release callbacks stays loaded for the initial process lifetime.
Function instances and session allocations still release when their owners end.
The current process-wide scalar registry retains the package owner, so separating
code retention from session state is required work. Bound repeated library loads
and identify actual content. A library retained after a discovery refusal still
consumes resources. A manifest cannot make untrusted native code safe.

The proposed kernel contract uses host execution and admitted memory. Today
`memory_bytes` is accepted only for driver placement, and driver extensions
cannot export scalars. Scalars run with placement `any` and no native memory
lease. Scalar and worker admission are therefore new work under N4/C15b, not
guarantees inherited from the Comet ABI or the product host.

The new contract must charge staging, ID maps, forward/reverse adjacency,
scratch, features and outputs, including peak construction overlap. Retained
Arrow slices must retain the relevant admission. Native allocation accounting
needs an explicit cooperation or reservation contract; a host cannot infer it
from an arbitrary callback. Bound projection caches by graph snapshot, selected
features, orientation and implementation contract.

The current lease tracks admissions and their final owners, not a count of
running producer clones. A session reservation reaching zero is not automatically
a per-job completion protocol. Define owned producer completion separately before
claiming cancellation has drained native work. Test retained results, failed
construction and noncooperative kernels. Accounted bytes and process RSS are
different observations.

## Use declarative pipelines for graph artifacts

A pipeline declaration describes desired tables and their dependency graph.
Registration and dry-run analysis should construct and validate plans without
staging a graph, running an algorithm or writing files. The existing A9 product
increment admits extension relations as batch sources for materialized views;
its bounded microbatch runtime refuses a streaming flow over such a relation.
This is not a continuous StreamSource verdict. The A9 graph-table test constructs
`GraphTables` outside the query function because its schema inspection is
forbidden during pipeline query construction. Preserve this distinction between
effect-free plan construction and eager remote analysis.

A useful first graph pipeline reads pinned vertex and edge tables, produces
selected graph data and hierarchy/layout artifacts, and publishes a manifest
naming their exact versions. A consumer sees a complete graph snapshot only
after all required artifacts have accepted writes. An atomic manifest switch
can select that completed version without claiming a transaction spanning all
underlying tables. Its publisher must reject stale ownership and reconcile lost
commit replies through the chosen Sail publication contract.

Reusable native projections are caches of a snapshot, not the durable source
of truth. Restart may rebuild a pure projection from retained inputs. It must
not retry an indeterminate mutation against a live native registry. Separate
read-only computation, snapshot-bound state and externally visible effects in
the request contract.

## Join the continuous runtime without inventing graph recovery

A StreamSource describes a live reader owned by the executing task. Its data
and control messages include progress and completion boundaries. Data-only
expression evaluation is implemented on the local redesign chain, including
projection at `62e180afe` and the filter isolation change recorded as `c442efdb7`.
It is not a property established by `99dc47788`, whose kernels see plain batches
in bounded microbatch flows. Continuous scalar execution requires agreement on
and porting of isolation, followed by controls for ordering and retractions.

Comet integration on the product control is batch-only. Qualify package identity
and library lifetime in an actual separate worker process or Kubernetes worker;
an in-process actor run is a separate control. After a continuous host is chosen,
gate that bridge with data and control messages through the actual worker path.

A finite graph relation can become a batch input with explicit completion in
an admitted plan. A live graph source additionally needs stable source identity,
recoverable positions, retained history and controls reaching its actual task.
No such graph source is delivered by the scalar ABI or by the old Nutmeg staging
handle. Incremental WCC, PageRank and hierarchy maintenance likewise need
algorithm-specific update and deletion semantics.

Use the selected Sail recovery mechanism for saved computation and publication.
Do not create a second Nutmeg checkpoint coordinator. Record source snapshots,
parameters, code identity, algorithm semantics, partition assumptions and any
random seed. Save intended output when deterministic recomputation cannot be
established. Every admitted durable target must enforce ownership at commit;
a cooperative single-writer convention is insufficient.

The local append reference covers process crashes with the filesystem and OS
surviving. It does not qualify distributed graph state, multiple coordinated
inputs, machine loss, arbitrary catalogs or broader SDP API integration. The
new graph manifest and snapshot protocol needs its own failure controls.

## Implementation order and distinguishing tests

1. Preserve Nutmeg's existing relation behavior on the selected product host,
   replacing only its graph foundation after the core contract is reviewed.
2. After the continuous host is agreed, port generic package/identity/placement/
   resource contracts independently of Grustic. Then gate Nutmeg on that host;
   only the Nutmeg-specific step needs the new graph core.
3. Independently add explicit Comet scalar export admission on the selected
   batch host, then actual worker-process execution and the new scalar/worker
   admission contract. This can precede step 2. Keep the old scalar route as a
   control; fail mismatched code and fields. Qualify continuous composition only
   after the isolation and combined-host prerequisites are met.
4. Admit batch graph materialized views, checking analysis has no effects,
   snapshot consistency, cancellation, work-directory isolation and result leases.
5. Add durable graph artifact publication only through the accepted target
   authority. Inject failures before/after acceptance, lost replies, takeover
   and cleanup. Delay live graph sources until their separate contract passes.

These increments are integration work, not a claim that unpushed branches are
accepted upstream. The final host composition needs its own exact-source
qualification and independent human review.
