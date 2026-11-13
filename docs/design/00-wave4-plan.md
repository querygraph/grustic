# Wave 4: establish Grustic as a composable graph foundation

## Intended result

Grustic lets Rust applications assemble graph capabilities without adopting a
particular parser, storage system or executor. Nutmeg uses those capabilities
on Sail, a Rust engine serving Spark clients. Cosmolang provides an exploration
protocol that feeds bounded graph views to Cosmograph in a browser.

Wave 4 begins with a clean repository and a reviewable architecture. The prior
Grust v2 work supplies candidate implementations and regression evidence.
Porting is selective: each retained contract must justify its place and pass
new tests after extraction. Historical tests establish historical behavior;
they do not establish that a new port or a new Sail combination works.

The immediate deliverable is the architecture review and document/commit plan.
Dependent implementation follows human review of the contracts it implements.
No production API, compatibility certification or performance result is claimed
by this initial documentation series.

## Proposed reading sequence

| Document | Decision it supports |
| --- | --- |
| [01-assessment.md](01-assessment.md) | Is the Grust v2 direction suitable, and what must change before porting? |
| [02-architecture.md](02-architecture.md) | What are the reusable contracts and dependency boundaries? |
| [03-spark-compatibility.md](03-spark-compatibility.md) | How can Nutmeg track existing and proposed GraphFrames behavior? |
| [04-sail-integration.md](04-sail-integration.md) | How do native relations, Comet scalars and pipelines participate? |
| [05-port-and-qualification.md](05-port-and-qualification.md) | What gets ported, how is it qualified, and what is deferred? |
| [06-commit-sequence.md](06-commit-sequence.md) | Which design comes before each implementation increment? |
| [sources.md](../review/sources.md) | Which exact source revisions and external proposals informed this review? |

The overview and assessment are the short decision path. The other documents
supply distinct contracts for their implementers and reviewers.

## First complete use case

A PySpark Connect client supplies vertex and edge DataFrames to Nutmeg. Sail
resolves a bounded graph selection, stages a reusable native projection when
appropriate, and executes a precisely specified connected-components method.
It returns a composable DataFrame with the chosen compatibility profile's
schema. A Cosmolang client requests a bounded view of the same pinned graph,
follows edges and expands a hierarchy group in the actual Cosmograph SDK.

This use case exercises the seams before a wide algorithm or language port.
A parser-free Rust client and a second small execution adapter must exercise
the same semantic plan. That is the early test of modularity.

## Decisions requiring human review

1. Adopt a graph-aware logical plan with explicit lowering to engine plans.
2. Keep GraphFrames compatibility and its versioned result conventions in adapters.
3. Preserve Sail's relation interface while adding Comet scalar support separately.
4. Keep pipeline recovery under Sail's execution and publication authorities.
5. Port tested semantics selectively; rebuild prototype runtime and transport seams.

The detailed documents state alternatives and distinguishing tests. Acceptance
is recorded against a document revision, with reviewer and remaining limits.
Committing a proposal does not mark it accepted.

## Delivery shape

Initialize the repository with separate documentation commits: this charter,
source inventory and assessment, core architecture, Spark compatibility,
Sail integration, then migration and qualification. Implementation commits
follow in the dependency order proposed in [06-commit-sequence.md](06-commit-sequence.md).

Each code change contains one coherent behavior, tests, attribution and a
manual reproduction. Review cumulative changes against the agreed base.
Avoid large mechanical imports that make a reader reverse-engineer a design.
The source, artifact and verdict identities must remain distinct.

## Completion criteria

The planning goal finishes when all proposed documents exist, their source
claims are checked, local links resolve, and the repository contains the
reviewable documentation history. Wave 4 implementation is a later milestone:
its gates are specified in the qualification document and remain pending.
