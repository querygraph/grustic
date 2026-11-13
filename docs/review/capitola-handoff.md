# Capitola handoff incorporated into the review

Claude on Capitola replied to the Grustic questions as CF-0807, mirrored in
`/Users/alexy/src/grust-sdp/codex-to-codex.md` at
`2026-11-13T22:04:05.295432+00:00`. Alexy designated that file as the live
channel, with append-only writes protected by `codex-to-codex.md.lock`.

## Confirmed by the peer

- The pushed SDP product reference is `99dc47788`. The repaired standalone
  scalar reference is `bfdecbcf9`; the local append reference is `37837d20a`.
  P5 is superseded. The local redesign chain has not incorporated later upstream
  main and is not maintainer-accepted.
- Owner directions include continuous StreamSource execution, Comet scalars
  independent of streaming, strict typed configuration, optional Dhall,
  optional ORC/Iggy crates, and commit-time fencing for admitted durable targets.
  Detailed operator, authority/failure-model and package-boundary decisions
  remain open for maintainers.
- The working native relation path uses Python package discovery, DataFusion FFI,
  driver placement and host-funded C memory leases. No general C relation contract
  or C scalar package/worker bridge is established by the local prototype.
- Stateful driver regions disable replay. Lost mutation acknowledgements can be
  indeterminate. Admissions and their final ownership do not provide a per-job
  signal that every native producer finished.
- Maintainers expect design discussion before dependent code and can review a
  few coherent reference PRs. The implementation commit count should not dictate
  a large number of upstream PRs.

These are confirmations from a collaborator and the cited session record.
This report does not turn them into evidence of an integrated runtime gate or
an independent maintainer decision.

## Independently checked in source

At product revision `99dc47788941daf28eec847527949a4cadafdbd2`:

| File | Observation |
| --- | --- |
| `examples/extensions/nutmeg/Cargo.toml` | DataFusion 55.2.0 and Arrow 59.2.0 exact pins |
| `examples/extensions/vendor/nutmeg-graph/Cargo.toml` | Grust crates at 0.24.1 |
| `crates/sail-session/src/extensions/manifest.rs` | Host DataFusion/Arrow versions are build-derived and mismatches refuse before binding |
| `crates/sail-session/src/extensions/mod.rs` | `pysail.extensions` discovery, retained package owner and worker loading path |
| `crates/sail-common-datafusion/src/driver_extension.rs` | Driver-scoped bound plans, weak registry entries, worker decode refusal and disabled-retry declaration |
| `crates/sail-common-datafusion/src/native_resource.rs` | Session admission counter and final-owner release; accounted quota is distinct from process RSS |
| `crates/sail-spark-connect/src/pipelines/streaming/admission.rs` | Unqualified extension nodes are refused for streaming admission |

The scalar component's public source and ABI records were read through SSH.
Its independent-producer and lifetime results come from committed historical
receipts. No new binary test was performed during this architecture review.

## Resulting plan corrections

Comet scalar integration can proceed against a chosen supported host independently
of the continuous-runtime port. The combined runtime still needs a later gate.
Native driver retry and job drainage are explicit contracts to preserve or
extend deliberately. Current batch graph materializations and proposed continuous
composition have separate acceptance tests. Source revisions, owner directions
and maintainer acceptance remain distinct throughout the packet.
