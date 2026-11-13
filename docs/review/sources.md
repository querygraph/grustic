# Sources and limits of this review

This is a source and architecture review. It includes historical qualification
receipts, but runs no new graph benchmark or runtime gate. Exact observed
commits, tree identities, the Spark proposal export digest and observation time
are recorded in [source-pins.json](source-pins.json). Branch names below locate
work; immutable commits identify what was reviewed.

## Grust and Sem's requirements

- [Sem's architecture clarification on PR 36](https://github.com/querygraph/grust/pull/36#issuecomment-6000697247): independently usable graph/query components, a separate assembled engine, and separate local/distributed algorithms. Avoid assumptions that prevent future graph-schema or storage-format support; do not declare conformance to an evolving LEX proposal.
- [Sem's earlier answers](https://github.com/querygraph/grust/pull/36#issuecomment-5992818702): one owning schema group, feature alignment, working-set estimates, reverse adjacency, host-provided execution, multiple parse diagnostics and a function registry before Sail integration.
- [Grust main at 58c01ed2](https://github.com/querygraph/grust/tree/58c01ed2db7e46e8637d247c9b62d0f4f1b3e086/docs/reviews/grust-v2): earlier integrated Wave 2/3 design, executable subset and evidence. This tree does not contain the later complex-SNB report.
- [Complex-query branch at 60e4e90d](https://github.com/querygraph/grust/tree/60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7/docs/reviews/grust-v2): later executable candidate. The `wave-3/sketch` sources and necessary `wave-2/sketch` contracts are the proposed extraction source.
- [Complex-SNB qualification](https://github.com/querygraph/grust/blob/60e4e90d24533d8c4678b6eb1bfbcb195e1fe5e7/docs/reviews/grust-v2/wave-3/COMPLEX-SNB.md): exact historical gate at `b2d5d8b1f3866272bc637fb76e544f35c2c3321c`. Short queries and IC2/IC8/IC9, independent CSV oracles, synthetic disjoint replicas, estimates, slower outcomes and sampled memory limits.
- [Cosmolang live slice at d56a4993](https://github.com/querygraph/grust/blob/d56a4993f3378fa4775ad261a7bc4926ef23c449/docs/reviews/sem-research-2026-10-03/visualization/cosmolang/live/README.md): historical runtime qualification at `4d222349b452ddc75e76cf3267acec2f78c73ba3`, actual Cosmograph 2.5.1, native Sail/Nutmeg, bounded two-level hierarchy. Source and artifact versions differ and are stated in that report.

## Sail product and local redesign

`grust-sdp` is a Capitola checkout of **querygraph/grust**, on
`work/sail-declarative-pipelines`. It is not a separate GitHub repository.

- [SDP review memo at e873f06b](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/REDESIGN-REVIEW-PACKET.md) and [human review](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/HUMAN-REVIEW.md): execution ownership, data/control isolation, destination authority and explicitly selected native interfaces.
- [Delivery plan](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/REDESIGN-PLAN.md), [clarity rules](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/CLARITY.md) and [commit map](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/COMMIT-MAP.md) inform this repository's document-first sequence. The map explains amended hashes; old receipts retain their original source identities.
- [Pushed SDP product at 99dc4778](https://github.com/querygraph/sail/tree/99dc47788941daf28eec847527949a4cadafdbd2): bounded pipeline product and existing extension integration. The book's differences chapter documents its Spark 4.2.1 reference behavior. It does not establish the local redesign is integrated.
- [Earlier extension host at a20d660d](https://github.com/querygraph/sail/tree/a20d660d9f3439adcbeee3836edde94ecaaa6fc9): manifests, relation dispatch, DataFusion FFI and native memory leases. This is a historical control, not the newest product host.
- [Arrow C extension proposal](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/EXTENSIONS-ON-ARROW-C-ABI.md), [integration design](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/EXTENSIONS-STREAMSOURCE-INTEGRATION.md) and [component results](https://github.com/querygraph/grust/blob/e873f06b44110e74c1a0cc638283eecf11f45e20/docs/sail-declarative-pipelines/COMET-SCALAR-RESULTS.md): pinned scalar interface, field fidelity, retained callback code and actual Comet SDK producer probes. Package and worker integration remain unqualified there.

The following committed heads were inspected through SSH in Capitola's
`/Users/alexy/src/sail`. They are local-only references, so this report does
not invent working GitHub links to them or push them.

| Local branch | Head | Reviewed significance |
| --- | --- | --- |
| `work/streamsource-foundation-20261105` | `049017850` | Streaming data/control fidelity and failure propagation |
| `work/streamsource-comet-scalar-20261105` | `bfdecbcf9` | Repaired standalone C scalar component; original evidence names `06dd7e237` |
| `work/streamsource-projection-20261105` | `62e180afe` | Expressions isolated from control records |
| `work/streamsource-participation-20261105` | `cb2d58ab0` | Source participation survives logical pruning |
| `work/projection-composition-20261106` | `c40e30efc` | Computations and declared schemas survive composition |
| `work/delta-append-recovery-20261106` | `37837d20a` | Local process-crash append reference; old evidence names `bddb46709` |
| `proto/p5-c-udf` | `ef7286ca2` | Earlier scalar prototype, superseded as the preferred component reference |

The branch map and selected implementation files were inspected. This is not a
line-by-line review of the entire streaming redesign. Its published failure
model is local process crashes while the OS/filesystem survive, with one
source partition. Distributed recovery and machine-loss durability do not
follow from those tests.

## External contracts

- [GraphFrames SPIP](https://docs.google.com/document/d/1v8OEV5Qqk5LE1XID4ZiPF5rLkAk-Zz6fp0tns9Z_TA8/edit): exported text was read and hashed. It says Discussion and targets Spark 4.5 conditionally. Public naming and result conventions remain proposals.
- [Spark POC PR 58302](https://github.com/apache/spark/pull/58302), observed open/draft at `9ba1b8e5a840478bedcac1e3a2090735d92b7739`. Its [relation protobuf](https://github.com/apache/spark/blob/9ba1b8e5a840478bedcac1e3a2090735d92b7739/sql/connect/common/src/main/protobuf/spark/connect/relations.proto) has `graph_frames = 49`. That is a typed relation field, distinct from the existing extension envelope. The POC's API and the SPIP's proposed API are not interchangeable.
- [Current GraphFrames Python API](https://graphframes.io/api/python/graphframes.html): DataFrame inputs/results, expression arguments and result-lifetime obligations. Pin a release before differential execution; this moving documentation page is not a version lock.
- [Calcite adapter architecture](https://calcite.apache.org/docs/adapter.html): replaceable rules, metadata and execution conventions. [DataFusion extension operators](https://datafusion.apache.org/library-user-guide/extending-operators.html): integration points for logical/physical operators and optimizer rules.
- [GraphAr format](https://graphar.apache.org/docs/specification/format/): logical entities, physical property bundles, chunked storage and adjacency layouts must remain distinguishable.
- [Pinned Comet SDK](https://github.com/apache/datafusion-comet/blob/0f7f8e45399847715093b3d3affdfa97614b39dc/native/comet-udf-sdk/src/lib.rs): an attributed, pinned interface, not an unrestricted ABI stability promise.

## Review communication

Alexy authorized questions to Capitola's Claude session. They were appended
as CF-0806 in `/Users/alexy/src/grust-sdp/codex-to-fable.md`, the SDP protocol's
canonical live log. An entry in `codex-to-codex.md` points to that message.
Posting is not acknowledgement or human acceptance. Any subsequent reply
will be recorded separately with its scope.
