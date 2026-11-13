# Grustic contributor instructions

## Design before dependent code

This is an independent repository, not the governing Grust handoff directory.
Start from the plan in `docs/design/00-wave4-plan.md`. Documents describe the
user problem, existing behavior, alternatives, recommendation, evidence and
open decision. Mark proposals and human acceptance separately. Do not infer
acceptance from a commit, passing tests or another agent's review.

Implement each accepted contract in a focused, human-reviewable commit with a
full message explaining the change, reason, verification and limits. Preserve
source attribution when porting. Do not import the old workspace wholesale.
Keep designs here; host changes belong on named branches in querygraph/sail.
Do not publish crates or open upstream Sail PRs as part of a design review.

## Engineering and evidence

Before Rust source or Cargo changes, read `/Users/alexy/src/querygraph/RUST.md`
in full on this host, or the equivalent checked-out QueryGraph guide elsewhere.
Use algebraic domain types, a functional core, typed errors and separate test
files. Keep Python typed and structured; gate with Ruff and mypy.

Gate exact commits in clean detached worktrees with owned external targets and
`CARGO_INCREMENTAL=0`. Preserve HEAD and cleanliness checks. Check free disk
before builds. Make commits conditional on checks using `&&`. Benchmark native
optimized binaries; VMs are for Linux functional/build tests. One heavy job at
a time on shared hardware. Publish shared-host ratios with complete outcomes,
not absolute performance claims. Keep failures, expected errors, refusals and
successes distinct. No evidence implies performance leadership by itself.

Inputs satisfy their declared graph contract. Only free schema/argument checks
run by default. Optional data validation is a separate operation. Never pass
unchecked native pointers merely because graph data is trusted.

Do not change other working checkouts, publish local-only branches, or contact
collaborators without task authorization. Never expose credentials in evidence.
