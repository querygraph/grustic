# A reviewable document and commit sequence

The repository starts with the documentation series below. Human decisions
then authorize their dependent implementation. Source review, a prototype or
a passing test is not a substitute for that decision.

Keep individual commits coherent enough to explain and reproduce. The count
below is a dependency map, not a demand for one upstream pull request per row.
Capitola's Sail practice permits a few well-explained reference PRs followed by
agreed smaller integrations. Grustic implementation can proceed independently
where its own accepted contract does not depend on an unsettled Sail change.

## Initial repository history

| Commit unit | Artifact | Review purpose |
| --- | --- | --- |
| D0 | Charter, contributor instructions and `00-wave4-plan.md` | Establish scope before source imports |
| D1 | Source inventory and `01-assessment.md` | Challenge the MVP against Sem's architectural requirements |
| D2 | `02-architecture.md` | Propose phase, identity, extension and execution boundaries |
| D3 | `03-spark-compatibility.md` | Define version profiles and unchanged-client obligations |
| D4 | `04-sail-integration.md` | Explain current host, Comet and pipeline integration duties |
| D5 | `05-port-and-qualification.md` | Select ports and define full-product evidence |
| D6 | This sequence and the human review index | Make dependencies and acceptance decisions explicit |

Each has a full commit message stating purpose, checks and limits. These
commits initialize the independent repository without carrying Grust history,
unrelated backends or old build artifacts.

## Design notes before code

The overview documents provide the architecture recommendation. The following
short notes must settle concrete contracts before the listed code units.
Each note explains one problem, viable alternatives, a recommendation and a
small test that distinguishes them. Record acceptance against its revision.

| Note | Concrete decision | Code units unlocked |
| --- | --- | --- |
| N1: values and identity | Entity identity namespace, schema ownership, explicit nulls and portable type mapping | C1–C2 |
| N2: query contracts | Plan construction/validation, graph operators, plugin nodes, diagnostics and dialect profiles | C3–C6 |
| N3: physical choice | Rule legality, required properties, statistics, conversions and bounded search | C7–C8 |
| N4: kernel ownership | Projection lifecycle, host executor, features, quotas, cancellation and producer completion; scalar/worker admission is new work beyond today's driver lease | C9–C10, C15b |
| N5: Spark profiles | Pinned baseline, complete API inventory, typed Connect translation and result conventions | C11–C13 |
| N6: native host migration | Future continuous host remains undefined pending Sail decisions; choose source, relation route, Comet exports, dependency floors, process identity and retry limits | C14a–C15b |
| N7: pipeline graph publication | Effect-free analysis, snapshot manifest, commit authority and failure model | C16–C17 |
| N8: exploration protocol | View installation, hierarchy membership, indexing, caps and speculative work | C18–C20 |
| N9: release evidence | Workload inventory, resource envelopes, regression policy and supported compatibility claims | C21–C22 |

Notes can be reviewed together where their decisions are coupled. They need
not become nine separate approval ceremonies. The purpose is to expose an
interface before substantial code makes it expensive to change.

## Proposed implementation commits

Each row includes tests with its behavior. Splitting an oversized row must
preserve its stated contract and dependency. Core implementation belongs in
Grustic; changes to Sail's host/protocol belong in `querygraph/sail` with a
cross-repository source manifest and a final combined gate.

| Unit | Focused change and proposed message | Depends on | Acceptance evidence |
| --- | --- | --- | --- |
| C0 | `build: establish the independent Grustic workspace and gates` | Architecture direction | Default dependency closure, toolchain, provenance and gate parity; published adapter floors allow host pins (current Arrow 59.2.0/DataFusion 55.2.0) |
| C1 | `feat(types): define logical values and source identities` | C0, N1 | Null/list/field types, source-ID round trips, dense-index separation |
| C2 | `feat(schema): add graph groups and snapshot catalog contracts` | C1 | Schema-only external consumer, inheritance and missing-property cases, bounded feasibility reasoning |
| C3 | `feat(functions): bind extensible semantic signatures` | C1, N2 | Scalar/aggregate distinction, overloads, null rules, effects and unavailable implementation errors |
| C4 | `feat(plan): add canonical unresolved plans and Rust construction` | C2–C3 | Parser-free consumer; invalid scope and parameter diagnostics |
| C5 | `feat(cypher): extract the reviewed frontend and diagnostics` | C4 | Attributed parser extraction, corpus preservation, multiple spans/recovery and refusal to execute malformed syntax |
| C6 | `feat(resolve): preserve typed graph semantics through analysis` | C4–C5 | Stage as separate commits for basic patterns, uniqueness, paths, correlation and ordering/numerics; independent answers for each |
| C6r | `feat(reference): execute the admitted query subset independently` | C6 | Small reference evaluator with independently derived fixtures, no Sail dependency |
| C6s | `feat(sail): lower admitted queries through the SQL adapter` | C6 | Same admitted subset as C6r, strict parameters, correct unsupported outcomes and real Sail execution |
| C7 | `feat(plan): expose physical capabilities and structured explain` | C6, N3 | Third-party operator/provider, required order/placement and explicit conversions |
| C8 | `feat(optimizer): add bounded costed alternatives` | C7 | Semantic equivalence, unknown statistics, memory feasibility, planning caps and complete decision trace |
| C9 | `feat(kernels): define admitted projection and execution ownership` | C2, N4 | Resource/feature alignment, host executor, bounds, cancellation and retained-output ownership |
| C10 | `feat(nutmeg): execute one graph contract through Sail` | C6, C9 | Release native WCC with a reference contract, pure DataFrame composition, projection reuse and out-of-budget outcome |
| C11 | `feat(spark): translate versioned graph requests` | C7, C10, N5 | Sail Connect host decoder plus unchanged pinned PySpark client; existing extension route retained as control |
| C12 | `feat(graphframes): add matched traversal and PageRank contracts` | C11 | BFS/shortest-path/PageRank discriminating oracles, schema/default/result checks and downstream DataFrame operations |
| C13 | `feat(graphframes): close the agreed public API inventory` | C12 | Several algorithm-family commits, each with classic/Connect references; every inventory row has evidence or an explicitly incomplete status |
| C14a | `feat(extensions): port generic relation ownership to the agreed host` | N6 continuous-host decision; independent of Grustic core | Chosen continuous host with generic package identity, driver placement, session state, indeterminate outcomes and cleanup |
| C14b | `feat(nutmeg): qualify Grustic on the migrated relation host` | C10, C14a | Nutmeg-specific combined gate, projection reuse, results and ownership; no inferred automatic replay |
| C15a | `feat(extensions): integrate pinned Comet scalar exports in batch plans` | N6 batch-host contract; independent of C10/C14 | Explicit admission, independent SDK binary, fields/errors/lifetimes; actual separate-process or Kubernetes worker identity, not actor-only evidence |
| C15b | `feat(extensions): admit scalar and worker native resources` | C15a, N4 | New accounting and ownership contract, cancellation/retained outputs; current driver-only lease cannot establish this |
| C16 | `feat(pipelines): admit batch graph materializations` | C11, N7; existing product host | Dry run has no effects; actual SDP declaration and graph snapshot consistency on the combined host |
| C17 | `feat(pipelines): publish recoverable graph snapshot artifacts` | C14b, C16, N7 | Accepted authority, immutable manifests, lost replies/takeover/restart and failures at every publication boundary |
| C18 | `feat(cosmolang): implement bounded revisioned exploration` | C10, N8 | One dispatcher for HTTP/MCP, lossless IDs, resource caps, retries, stale-view refusal and install acknowledgements |
| C19 | `feat(cosmograph): connect the SDK to graph snapshot views` | C18 | Actual pinned SDK, Arrow loading, follow/expand/collapse and browser controls; software renderer distinguished from GPU measurements |
| C20 | `feat(exploration): add indexed hierarchy and bounded prefetch` | C17, C19 | Multi-level membership and quotient invariants, degree/nearest-neighbor contracts, snapshot invalidation, budgeted speculation and no silent truncation |
| C21 | `test(qualification): measure representative end-to-end graph workloads` | Relevant C8–C20, N9 | SNB oracles, connected/skewed inputs, local plus actual separate-process workers, placement, all outcomes and complete conversion/output costs |
| C22 | `docs(release): publish supported contracts and reproducible evidence` | C21 and complete release inventory | Package tests from outside workspace, compatibility matrix, migration guide, artifact hashes and reviewed release scope |

C6 and C13 are explicitly multi-commit families; their eventual subjects should
name the concrete behavior. Do not hide a large import beneath either heading.
A prerequisite that is still a proposal prevents its dependent code from landing,
but does not prevent independent core work after its own design is accepted.

C14a's destination is currently undefined; memo decisions 1 and 4 must settle
execution ownership and native interfaces first. C15a can target the working
`99dc47788` batch host without waiting for that continuous host. Its batch gate
does not cover control messages. Actual workers on that product host require
Kubernetes or a prior port of the missing separate-process launcher.
A later continuous integration commit needs
accepted and ported projection/filter isolation plus its own combined gate.
C16 concerns batch materialized views, not the declined bounded microbatch
streaming runtime. Its tests must also preserve the prohibition on eager schema
analysis inside pipeline query functions.

## Milestones and stop conditions

**Core foundation:** C0–C9, including C6r and C6s. The reference backend and Sail
SQL adapter must execute the same admitted subset; an explain-only mock does not
demonstrate portability.
Do not declare this milestone complete with only compiled trait signatures.

**First complete product slice:** C10–C12 and C18–C19 on the selected working
host. This delivers the bounded DataFrame-to-Cosmograph use case. It remains
experimental and does not claim full GraphFrames compatibility or large-scale
hierarchical deployment.

**Wave 4 product qualification:** C13–C17 and C20–C22 close the agreed inventory,
new extension integration, pipeline artifacts and scale evidence. Live graph
sources, general incremental graph algorithms, full ISO GQL, transactions and
a general C relation ABI require separate scope decisions. They are not silently
inferred from these milestones or hidden behind a passing small slice.

A code unit is ready for human review only with its design revision, source
origin, cumulative diff, exact checks and unresolved limits. A confirmed introduced
regression blocks readiness. A performance claim additionally needs the matched
boundary and full results; failed/unsupported cells remain visible.
