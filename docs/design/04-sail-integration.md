# Nutmeg execution through Sail's extension and pipeline contracts

Nutmeg should execute graph operations inside Sail while returning ordinary
composable DataFrames. Sail owns task execution, memory admission, cancellation
and publication. Grustic supplies semantic plans and graph implementations.

**Proposal:** preserve the working native relation route, add the Comet scalar
route explicitly, and integrate both with the chosen Sail runtime in separate
reviewable increments. Neither scalar interoperability nor a passing standalone
pipeline proves their combined behavior.

## The actual starting points

The [source inventory](../review/sources.md) pins three distinct references.
`99dc47788` is the pushed SDP product with the working extension host. The
repaired scalar component is local-only `bfdecbcf9`. The local append-recovery
reference is `37837d20a`, based on upstream `b52144955`. These are not one
qualified combined host. P5 `ef7286ca2` is an earlier prototype.

On the product reference, Python entry points in `pysail.extensions` discover
packages. Manifests are checked before binding native objects; host identity
comes from `Cargo.lock` at build time. Nutmeg pins DataFusion 55.2.0 and Arrow
59.2.0, and its vendored graph adapter uses Grust 0.24.1. A Grustic adapter that
crosses the same DataFusion FFI must inherit the selected host's exact pins.
Do not import those pins into the engine-independent core.

The existing native relation is a session-owned region placed on the driver
in cluster mode, or the local session process in local mode. Host inputs and
frozen plan identity are explicit. Retry is disabled for that region; an
unacknowledged mutation can be indeterminate. The new design must retain that
outcome until a separate replay protocol is implemented and verified.

Capitola's reply confirms the owner directions: continuous StreamSource
execution, the additional C scalar boundary independent of streaming,
commit-time rejection of obsolete writers for every admitted durable target,
and strict typed configuration. The detailed host mechanisms remain proposals
for maintainers. Design agreement precedes dependent host changes.

## Keep three native boundaries distinct

| Boundary | Work it carries | Migration requirement |
| --- | --- | --- |
| Existing DataFusion FFI | Native relation planning/execution and existing scalar packages | Preserve exact build admission, package identity, driver placement, input binding, cleanup and errors |
| Comet Arrow C scalar interface | Trusted immutable scalar kernels over Arrow fields and arrays | Pin producer/layout; add explicit per-export selection, package loading and worker identity |
| Native memory lease | Host-funded quota retained while allocations/results need it | Preserve ownership; add a separate job completion contract if per-job drainage is required |

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

Native kernels use a host executor and admitted memory. Charge staging, ID maps,
forward/reverse adjacency, scratch, features and outputs, including peak overlap
during construction. Retained Arrow slices keep the relevant memory admission
alive. A projection cache is bounded and keyed by graph snapshot, selected
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
it refuses a streaming flow over such a relation. Preserve that tested boundary
while adding any broader composition deliberately.

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
and control messages include progress and completion boundaries. A scalar
kernel receives only data; Sail preserves control ordering and retractions.
The new Comet bridge must pass this through package and worker execution tests,
not just standalone array calls.

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
2. Review and port the package/identity/placement/resource contracts onto the
   selected continuous-runtime foundation. Gate the actual combined host.
3. Independently add explicit Comet scalar export admission on the selected
   supported host, then package and worker execution. This can proceed before
   the continuous-runtime port in step 2. Keep the old scalar route as a control;
   fail mismatched code and fields. Gate their combination when both are ready.
4. Admit batch graph materialized views, checking analysis has no effects,
   snapshot consistency, cancellation, work-directory isolation and result leases.
5. Add durable graph artifact publication only through the accepted target
   authority. Inject failures before/after acceptance, lost replies, takeover
   and cleanup. Delay live graph sources until their separate contract passes.

These increments are integration work, not a claim that unpushed branches are
accepted upstream. The final host composition needs its own exact-source
qualification and independent human review.
