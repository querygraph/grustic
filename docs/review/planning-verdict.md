# Wave 4 planning deliverable

The Grustic architecture packet is prepared for human review. The clean-slate
repository contains the plan, source-grounded assessment, proposed contracts
and a dependent document/implementation sequence. This is the planning milestone;
it does not complete Wave 4 implementation or approve its proposed interfaces.

## Coverage of the requested work

| Requirement | Review artifact |
| --- | --- |
| Independent Grustic project with reviewable initialization | `work/wave4-design` history, starting at `b81e4e4`; no old workspace, code or build targets imported |
| Overall plan before implementation | [00-wave4-plan.md](../design/00-wave4-plan.md) |
| Comprehensive Grust v2/MVP assessment as a Calcite-like foundation | [01-assessment.md](../design/01-assessment.md): strengths, source observations, risks, alternatives and recommendations |
| Composable graph/query/kernel modules | [02-architecture.md](../design/02-architecture.md): phase contracts, third-party extension, identity, snapshots, physical choice and independent-consumer tests |
| Maximum GraphFrames/PySpark compatibility | [03-spark-compatibility.md](../design/03-spark-compatibility.md): existing release, draft POC and proposed core profiles; typed Connect decoding; full API inventory |
| Current Sail extensions and Comet C interface | [04-sail-integration.md](../design/04-sail-integration.md): separate relation/scalar contracts, exact dependency compatibility, placement and resource ownership |
| Sail Declarative Pipelines and Capitola's unpublished work | Same integration note plus [sources.md](sources.md): product batch behavior, local continuous references, open host decisions and recovery limits |
| Selective Wave 3 ports and prerequisites | [05-port-and-qualification.md](../design/05-port-and-qualification.md): retain tested semantics and minimal Wave 2/parser dependencies, replace prototype execution assumptions |
| Cosmolang, real Cosmograph and large-graph exploration | Same port note: existing C1 boundary, versioned hierarchy, indexed subsets, nearest neighbors, degree filters, layouts, prefetch and explicit browser caps |
| Efficient Nutmeg with honest evidence | Qualification matrix: native optimized builds, matched contracts, complete projection/output costs, independent oracles, actual worker topology and all outcomes |
| Design notes followed by focused implementation commits | [06-commit-sequence.md](../design/06-commit-sequence.md): nine proposed contract notes, dependencies, distinguishing gates and completion criteria |
| Direct coordination with Capitola | [capitola-handoff.md](capitola-handoff.md): authorized locked log, source confirmations and disposition of six peer findings |

## Verification retained

The exact detached documentation gate at
`b112244b88309e8114c2dc7808c521950f338ef1` passed. Its
[receipt](document-checks.json) records file hashes, source identity and checks:
13 documentation/JSON files, 24 local links, 12 immutable repository source
paths, fetched source/tree pins, JSON syntax, whitespace and unchanged clean
HEAD. Each prose document is under 2,000 words. This receipt predates this
completion record; it does not claim to hash itself or later files.

Claude rechecked the integration and sequence at the same revision and confirmed
all six corrections, with no remaining factual or dependency error in those
boundaries. His worker-launcher clarification is incorporated. The handoff record
preserves the review's scope and timestamps.

Primary sources and historical runtime receipts were inspected. No new binary,
benchmark, browser runtime or distributed gate was run for the planning task.
At the planning gate, the repository was local with no configured remote. Alexy
subsequently authorized publication to the public
[querygraph/grustic repository](https://github.com/querygraph/grustic), preserving
the documentation history and the `work/wave4-design` branch. No source branch
on Capitola was published or modified by this work; coordination entries were
appended under the persistent file lock.

## Next human decision

Review [HUMAN-REVIEW.md](../../HUMAN-REVIEW.md), then agree the core phase and
identity boundaries. Draft N1/N2 before their dependent implementation. Generic
host migration and Nutmeg-specific integration have separate dependencies;
the continuous Sail target remains undefined until its design decisions settle.
The first working slice is deliberately smaller than the full release inventory.
