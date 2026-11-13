# Grustic Wave 4: architecture review for Sem

Grustic will be a modular graph foundation for Rust. Developers can compose
schema, query, optimization and kernel capabilities independently. Nutmeg will
assemble them on Sail, with GraphFrames-compatible PySpark access and Cosmolang
feeding Cosmograph.

**Recommendation:** retain Grust v2's three-layer direction and selectively
port its tested semantics. Redesign the contracts that currently expose early
Sail lowering, duplicated prototype APIs or incomplete runtime ownership.
The new repository is initialized with reviewable documentation commits.
Implementation and new runtime qualification have not begun.

## Read this first

1. [Assessment and risks](docs/design/01-assessment.md): what is sound, what
   is demonstrated, and which choices could prevent the intended modularity.
2. [Document and commit sequence](docs/design/06-commit-sequence.md): the
   proposed path from design decisions to the full intended product.

The short [overall plan](docs/design/00-wave4-plan.md) defines the first complete
use case. Detailed contracts are references for their respective decisions:
[architecture](docs/design/02-architecture.md),
[Spark compatibility](docs/design/03-spark-compatibility.md),
[Sail integration](docs/design/04-sail-integration.md), and
[ports/qualification/Cosmolang](docs/design/05-port-and-qualification.md).
[Sources](docs/review/sources.md) identify what was inspected.

## Decisions proposed for review

| Decision | Recommendation | Reason |
| --- | --- | --- |
| Core plan | Preserve graph operations before engine lowering; one canonical contract per phase | A reusable planner must still be able to choose joins, indexes or native projections |
| Modularity | Keep logical schema, language, plans, optimizer, kernels and adapters independently consumable | Match Sem's toolkit/engine/algorithm separation and verify it with independent consumers |
| Spark compatibility | Versioned profiles and actual unchanged PySpark Connect clients | The SPIP is unsettled and its POC uses a new typed relation |
| Native boundary | Preserve DataFusion-FFI relations; add Comet scalars explicitly | The scalar ABI does not supply relation planning, staging or retry semantics |
| Runtime ownership | Sail owns execution and publication; retain indeterminate native outcomes | Task retries and memory admission do not establish safe replay or producer completion |
| Delivery | Design notes followed by focused commits and exact combined gates | Keep human intervention possible before implementation locks in decisions |

All rows are **proposed for review**. The owner has directed the clean-slate
project, modularity, use of the new Sail capabilities and human-reviewable
construction. No maintainer acceptance is inferred from that direction.

## Evidence and material limits

The review includes executable Wave 3 code, complex-SNB evidence, the real
Cosmograph C1 slice, the GraphFrames SPIP and POC, pushed SDP product sources,
Capitola's local C scalar/recovery references, and Claude's source handoff.
The full review distinguishes observed source behavior, historical test results
and recommendations. No old gate is presented as a Grustic verdict.

Claude's packet review sharpened the host boundary: current memory leases cover
driver regions, product worker evidence is in-process, and continuous data/control
isolation belongs to the separate redesign chain. The plan now names the required
new resource contracts and process gates, and separates the generic host port
from Nutmeg integration. The future continuous host still awaits design decisions.

The largest architectural risks are losing graph structure before optimization,
confusing source identity with kernel indices, forcing local CSR on every
workload, treating scalar ABI work as general extension compatibility, and
assuming retry or cleanup guarantees the host does not provide.

The performance objective is retained as an engineering ambition. Qualification
must include projection construction, transfer, repeated-call reuse, algorithms
and output under matched contracts, with all slower outcomes retained. The old
optimizer sometimes produced slower plans; a new project name does not fix that.

## What comes after this review

Settle the public model and phase boundaries first, then implement the workspace,
logical types, schema and function registry in the proposed sequence. Host-facing
code waits for its own accepted integration design. Small working slices provide
early evidence; completion still requires the agreed GraphFrames inventory,
combined Sail host, hierarchical exploration and scale/resource qualification.
