# Selective ports and evidence required for Grustic

The port should preserve proven semantics while establishing independent
contracts in a clean repository. Historical source and receipts remain in
Grust. Each imported piece gets a source revision, license attribution,
behavioral test and an explanation of what was changed.

This is an implementation and qualification proposal. Nothing in this document
relabels the historical gates as tests of Grustic.

## Port by contract

| Candidate source | Disposition | Required change or proof |
| --- | --- | --- |
| Wave 2 logical types/schema | Port the minimal dependency closure needed by Wave 3 | Separate source identity from projection indices; retain multilabel/inheritance, optional property keys and free schema checks |
| Wave 2 function registry and builder | Port selectively | Keep plugins before Sail integration; replace backend flags with semantic identity plus adapter implementation support where needed |
| Wave 3 executable resolved plan and resolver | Port semantics, revise public representation | One canonical phase contract, total typing, checked construction and preserved graph operations |
| Wave 3 uniqueness, correlation, path values, ordering and numeric controls | Preserve behavior and independent expected answers | Test dialect profiles, hidden ordering, nulls, bag multiplicity, aliased entity provenance and error boundaries |
| Wave 3 join optimizer | Retain as a bounded optional strategy | Supplied-statistics provenance, legality barriers, planning limits, implementation/conversion costs and measured regressions |
| Wave 3 Sail SQL emitter | Extract behind an adapter | Preserve strict parameter handling and dialect tests; do not make SQL the only physical route |
| Production `grust-cypher` parser dependency | Extract only the required lexer/AST/parser surface after review | Remove deep repository path dependency and engine/write-dispatch baggage; preserve corpus, diagnostics and licenses |
| Wave 2 kernel contract | Port and complete the resource boundary | Host executor, feature alignment, adjacency requirements, estimates, cancellation and output lifetime |
| Existing Grust native kernels | Evaluate algorithm by algorithm | Reuse code only after contract/provenance review; no wholesale legacy facade or backend migration |
| Cosmolang schemas and C1 controls | Port the protocol semantics and oracles | Rebuild deployment, catalog and resource ownership around reviewed interfaces |
| Python iterative runner, ad hoc fixtures, old sketch APIs | Keep as historical reference/test harness where useful | Replace production execution in Sail; do not expose prototype orchestration as the new core |
| Historical targets, binaries, giant evidence trees and unrelated backends | Keep out of the new source repository | Link exact evidence and store new artifacts externally with hashes |

“Only relevant Wave 3 work” requires a small number of Wave 2 contracts and
parser components because the executable code depends on them. Record this
closure explicitly; renaming directories without removing those dependencies
would not produce a clean foundation.

## Cosmolang and Cosmograph acceptance

Cosmolang is the versioned control protocol for exploring graph snapshots.
Cosmograph is the browser renderer. HTTP and Model Context Protocol, or MCP,
clients should call the same dispatcher; neither transport owns graph semantics.
Control messages identify operations and revisions. Arrow resources carry
bounded columnar results with schemas, counts and content identities.

First preserve the working C1 operations: request a bounded view, follow edges,
expand/collapse a group and run an admitted WCC method. Use the actual pinned
Cosmograph SDK and test its Arrow field mapping and rendered behavior. Camera
pan/zoom remain local; camera changes can request a new detail level. C1 is 2D;
3D rotation requires an explicitly selected renderer capability.

Extend the protocol through reviewable operations:

| Operation | Meaning that must be explicit |
| --- | --- |
| Degree/property filtering | Source-graph or visible-subgraph degree; directedness, parallel edges, null and boundary predicates |
| Nearest neighbors | Feature snapshot, metric, index identity, exact/approximate mode and result recall evidence |
| WCC and other algorithms | Source graph, induced subset or quotient graph; method and label/result convention |
| Hierarchy expansion | Membership and graph revision, incident edges to still-collapsed groups, loops and multiplicities |
| View installation | Request/session/view revisions and a browser acknowledgement distinct from resource creation |
| Prefetch | A speculative resource with a bounded budget and lifetime; no change to the user's active view |

For multibillion-node sources, generate a hierarchy as versioned data in Sail
pipelines. Each level has disjoint membership, stable group identity, quotient
edges with declared aggregation, and a layout tied to the same snapshot. WCC
alone is not a useful general hierarchy because one component may contain
almost the whole graph. Choose and measure a partitioning/coarsening method
separately. Expansion requires indexed membership and endpoint access; scanning
the whole graph for every click is not the intended design.

The service selects a detail level whose nodes, edges and bytes fit the negotiated
browser envelope, never above its configured million-node ceiling. That ceiling
is a requirement to qualify, not an observed C1 capability. Edge and byte limits
matter even when the vertex count fits. Complete results that exceed limits
must be refused or changed through an explicitly requested approximation.
A sampled answer must never masquerade as the full graph.

Layouts, hierarchy partitions and nearest-neighbor indexes carry snapshot and
method provenance. A graph update invalidates affected artifacts or creates a
new version. A view retains its old snapshot until a complete replacement is
installed. Dropped or delayed responses cannot overwrite a newer view.

Prefetch can prioritize neighboring groups or likely detail levels using recent
navigation. Charge CPU, memory, disk and network; cancel obsolete work and keep
foreground work ahead of speculation. Measure useful-prefetch fraction and wasted
bytes/work before enabling a predictive policy by default. A hierarchy result
must not be presented as exact original-graph algorithm output.

## Qualification matrix

| Boundary | Evidence required |
| --- | --- |
| Independent modules | Small external consumer projects use schema, builder, parser and plan crates separately; dependency checks exclude unintended engines |
| Semantic correctness | Independent reference evaluator and exact fixtures for graph identity, path modes, multiplicity, nulls, correlation, ordering and numeric failures |
| Parser extensibility | Multiple recoverable errors with accurate spans; invalid partial syntax never executes; function calls use the registry |
| Optimizer | Resolved/optimized answers agree; legality barriers and unknown estimates survive; actual physical plans and all slower cases retained |
| SQL/native implementations | Same semantic requests and outputs, including conversion costs and out-of-budget refusal; no silent algorithm substitution |
| GraphFrames clients | Pinned public calls on classic Spark, Spark Connect and Sail Connect; exact schemas, errors, expressions and downstream DataFrame composition |
| Native package boundary | Independent artifacts; loader mismatches across actual separate-process or Kubernetes workers; fields, slices, cancellation and retained-output release; actor-only tests labeled separately |
| Native resource admission | Current driver leases distinguished from new scalar/worker contracts; dependency floors permit the exact host pins outside the workspace |
| Stateful execution | No automatic retry of indeterminate driver mutations; session/attempt isolation, cancellation and explicit producer completion where claimed |
| Pipelines | Batch materialized views qualified separately from proposed continuous execution; no eager analysis in query functions; matching snapshots, accepted writes, takeover, lost replies and restart under the declared failure model |
| Cosmolang | SDK browser tests plus independent HTTP/MCP controls; stale responses, caps, collapse round trips, quotient edges and lossless IDs |
| New combined host | Exact merged-source gate, not the sum of component verdicts; default and supported optional features recorded |

Native boundary checks apply to all Arrow types supported by the relevant
contract, including types Spark does not expose. Test code lives separately
from production code. Explicit invalid-graph validation stays outside algorithm
timers and outside default execution.

## Workloads and measurements

Re-run the existing short and complex SNB subset unchanged, with the pinned
CSV oracle. Add missing graph semantics through discriminating small examples
before expanding coverage. Keep Graphalytics algorithm references separate from
SNB query workloads; matching one does not establish the other.

Larger qualification must include connected skewed graphs, hubs, isolates,
parallel edges, long-diameter graphs, selective/unselective filters and a graph
that exceeds the local CSR admission limit. Disjoint replication remains a
useful control, but cannot stand in for these. Use official datasets and their
licenses and hashes when available; label generated workloads explicitly.

Report complete DataFrame-to-DataFrame or Parquet-in/Parquet-out work as agreed
for each comparison. Retain read, decode/map/projection, algorithm and write
costs where the measurement can actually separate them. Measure one call and
repeated calls on the same snapshot, cold and warm caches, local and distributed
placement, compilation, memory and transfer volume. State the actual worker
topology and algorithm placement: an actor cluster in one process is not
separate-process evidence; a driver-local algorithm is not distributed execution.
Charge precomputation and
index construction, with reuse count, instead of hiding them outside the claim.

Use optimized native builds and matched CPU/memory budgets. VMs serve Linux
functional/build testing. Dedicated-host timings support absolute results;
shared-host reports use paired ratios with dispersion and all outcomes.
No automatic speed-leadership claim follows from a selected fast cell.

For browser exploration, measure end-to-end request-to-installed-frame latency,
payload bytes, browser/GPU memory and concurrent-session behavior. Software
WebGL proves functional integration only. Million-node rendering and large
hierarchy traversal need their own hardware/browser receipts.

## Gates and release boundary

Run formatting, strict Clippy, tests and documentation checks on the selected
Rust workspace; Ruff/mypy on Python and type/build checks on the SDK adapter.
Gate exact commits in detached checkouts with owned targets and unchanged HEAD.
Record toolchain, features, binary hashes, dataset identities, parameters and
unresolved outcomes. A source verdict and an artifact/runtime verdict are
separate evidence.

A minimum useful slice may release as experimental with its supported inventory.
The intended product requires closing the agreed GraphFrames inventory,
qualifying the new host integration and meeting the reviewed resource and
performance envelopes. A capability refusal is an honest outcome; it does not
complete a required feature. Stable APIs and broader claims wait for those gates.
