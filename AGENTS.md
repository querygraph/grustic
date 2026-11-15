# Grustic agent instructions

These instructions apply to every coding agent, whatever harness or machine it
runs on. They are written and changed by humans only.

Grustic is a modular property-graph foundation for Rust: an engine-free LPG
model, parser, resolver and planner with extension points at every phase, plus
an arrow-rs reference backend. Nutmeg on Sail is the first consumer and uses
only public extension points.

## Sources of truth

The project is fully described by three things:

1. **OpenSpec requirements** in `openspec/specs/`: what the system must do.
   They are the only requirements.
2. **Code**: how it does it.
3. **Project memory** in `.agents/project-memory/`: durable knowledge that is
   neither a requirement nor obvious from the code, such as the architecture
   map, glossary and conventions.

`README.md` and `docs/` are written by and for people. They explain
motivation and record design discussion. Read them for context, but do
not treat them as requirements. When they disagree with a spec, the spec wins,
and you tell the operator about the disagreement.

## Every change is an OpenSpec change in its own pull request

One pull request contains exactly one OpenSpec change, **archived**:

- `openspec/changes/archive/<date>-<change-id>/` with `proposal.md`,
  `tasks.md`, the delta specs and, when needed, `design.md`;
- the deltas applied to `openspec/specs/` by `openspec archive`;
- the code, tests and project-memory updates that implement it.

The planned changes wait as draft proposals in `openspec/backlog/`, numbered
in dependency order. `openspec validate` does not read the backlog, so drafts
without delta specs never break the gates. A draft is a starting point, not an
approved design.

Workflow:

1. Propose: move the change's directory from `openspec/backlog/` to
   `openspec/changes/`, or create a new change there. Then write or revise the
   proposal and write the delta specs, design if needed, and tasks. Run
   `openspec validate <change-id> --strict`.
2. Stop for human review of the proposal and the delta specs before writing
   dependent code. The operator approves in the session or on the pull request.
3. Apply: implement the tasks in order, ticking each one when its
   verification passes.
4. Archive: run `openspec archive <change-id>`, then
   `openspec validate --all --strict`. The archived change and synced specs are
   part of the same pull request as the code.

**Trivial changes** need no OpenSpec change: typos, formatting, comments,
lockfile or tool bumps with no behavior change, and CI fixes. If you are unsure
whether a change is trivial, it is not. Say "trivial: <reason>" in the pull
request description.

A change that alters no requirement (a pure refactor or tooling change) is
still an OpenSpec change, with `skip_specs: true` in its `.openspec.yaml`. Do
not invent a requirement to satisfy validation.

## Specs override code, and only humans override specs

Archived requirements, plus the deltas of the change you are applying, define
correct behavior.

- Code that contradicts a requirement is a bug, even if its tests pass.
- If implementation shows that a requirement is wrong, ambiguous, impossible
  or contradicts another requirement, **stop**. Explain the conflict to the
  operator with the requirement name, the evidence and the options. Do not
  resolve it yourself.
- With the operator's explicit approval, change the requirement through a
  `MODIFIED`, `REMOVED` or `RENAMED` delta in the current change. Record the
  approval in the pull request under "Requirement changes approved".
- Never weaken, delete or reinterpret a scenario to make code pass. Never edit
  `openspec/specs/` directly; it changes only through archived deltas.

Every scenario must be covered by at least one test whose name or doc comment
names the requirement and scenario. A scenario that cannot be tested
automatically says so in the delta, with the manual check that replaces it.

## Where agents may write

| Path | Agents | Notes |
| --- | --- | --- |
| `openspec/changes/` | Yes | Through the workflow above |
| `openspec/backlog/` | Yes | Draft proposals; revise them in the pull request that changes the plan |
| `openspec/specs/` | Only via `openspec archive` | Never hand-edited |
| Agent-scoped code (below) | Yes | Implements the specs |
| Human-scoped code (below) | Only with explicit operator approval | Explain the change first |
| `.agents/project-memory/` | Yes | Rules in its `README.md` |
| `.local/agents/` | Yes | Gitignored, machine-local |
| `README.md`, `docs/` | **No** | Suggest text in the pull request instead |
| `AGENTS.md`, `openspec/config.yaml`, `.github/`, `unsafe-approvals.toml` | **No** | Human-maintained policy |

OpenSpec's generic task guidance sometimes suggests documentation tasks. In
this repository, user-facing documentation is rustdoc on human-scoped items, and
agent knowledge goes to project memory. Never plan tasks that edit `docs/`.

## Local notes

`.local/agents/` is gitignored and belongs to one machine and operator. Keep
local setup there: container or sandbox details, toolchain paths, benchmark
host addresses, credentials locations (never the credentials themselves) and
personal workflow notes. Read it at the start of a session if it exists.

Nothing in tracked files may depend on `.local/agents/`. If a local note turns
out to be true for everyone, move it to project memory without the local
details.

## Human-scoped and agent-scoped code

Humans own the small, important surface. Agents own the implementation.

**Human-scoped** (listed in `.github/CODEOWNERS`):

- workspace and crate manifests: `Cargo.toml`, `rust-toolchain.toml`,
  `deny.toml`, `.cargo/`;
- tooling policy: `pyproject.toml` and `boundaries.toml`;
- each crate's `src/lib.rs`: crate docs, lint attributes, module declarations
  and `pub use` of the public surface;
- each crate's `src/api/`: public traits, types, error enums and constants;
- each crate's public API snapshot, `public-api.txt`.

**Agent-scoped:** every other module under `src/`, `tests/`, `benches/`,
`examples/` and test fixtures.

Rules:

- Implementation modules are private or `pub(crate)`. The public surface is
  only what `lib.rs` re-exports from `api/`.
- Changing the public surface changes `public-api.txt`, which requires human
  review. Exposing an item through an agent-scoped path is still a public API
  change.
- Before editing a human-scoped file, explain what you want to change and why,
  and wait for explicit approval. List every human-scoped file you changed in
  the pull request.
- Agent-scoped code is still written for humans to read: functions sized as
  described in "Code structure", descriptive names, comments explaining why,
  and no clever tricks without a stated reason.

## Unsafe code requires human approval at every site

The workspace denies `unsafe_code`. Clippy's `undocumented_unsafe_blocks` and
`multiple_unsafe_ops_per_block` are denied as well.

To introduce or change any unsafe code:

1. Stop and explain to the operator: what the unsafe operation is, why no safe
   alternative works (including measured cost when performance is the reason),
   which invariants make it sound, and how a test or Miri exercises them.
2. After explicit approval, the operator or a human reviewer adds an entry
   `UNSAFE-NNNN` to `unsafe-approvals.toml`. Agents do not add or edit entries.
3. Mark the item with
   `#[expect(unsafe_code, reason = "UNSAFE-NNNN: <one line>")]` and give each
   unsafe block a `// SAFETY:` comment stating the invariants.

The approval covers that site and its stated invariants only. Moving the code,
widening the block or changing its invariants needs a new approval. FFI to
native graph kernels or Arrow C data is not exempt.

## Engineering rules

- Rust: algebraic domain types, a functional core with effects at the edges,
  typed errors per crate (no `anyhow` in libraries), no panics on input-driven
  paths, and tests in separate files.
- Core crates (types, schema, functions, plan, resolver, optimizer, session)
  depend on neither Arrow nor DataFusion. Engine types live in adapters.
- Python tooling is typed and gated with Ruff and mypy.
- Inputs satisfy their declared graph contract. Only free schema and argument
  checks run by default; data validation is a separate, explicit operation.
  Never pass unchecked native pointers merely because graph data is trusted.
- Correctness evidence comes from independently derived answers: LDBC
  reference results where they apply, otherwise oracles computed separately
  from the implementation.
- Keep failures, expected errors, refusals and successes distinct in tests and
  reports. A refusal does not complete a feature.
- Performance: benchmark optimized native builds, one heavy job at a time on
  shared hardware. Report shared-host results as paired ratios with all
  outcomes, including slower ones. Never claim performance leadership from a
  selected result.
- Preserve source attribution and licenses when porting code. Port by contract,
  never by wholesale import.

## Code structure

Optimize for a reader who goes through the code top to bottom once. Avoid both
extremes: a 500-line function body, and logic scattered across dozens of
one-line helpers and constants that force the reader to jump between places.

- A function does one meaningful step that its name can describe. Splitting
  is right when the piece has a name worth knowing, is reused, or is tested on
  its own. Splitting only to make functions shorter is not.
- Do not wrap a single expression or call in a function unless the name adds
  meaning the expression lacks, or it hides a detail that changes together.
- Keep a sequence of steps that reads as one story in one function. A
  function of 30 to 60 lines with clear sections is fine; past about 100 lines,
  look for real sub-steps.
- Name a constant when the value has meaning beyond its literal, is shared, or
  is a tuning knob. A literal used once, whose meaning is obvious at the use
  site, stays inline.
- Keep code next to the code it serves: a private helper used by one function
  sits right after it, in the same module. Do not create a module, trait or
  generic parameter for a single use.
- Indirection must pay for itself. Each layer, trait, builder or wrapper needs
  a second user or a stated reason; "might be reused later" is not one.

The project is a prototype with no released API and no users to migrate.

- No migration notes, deprecation notes or backward-compatibility remarks in
  code, comments or docstrings. Nothing is kept "for compatibility": change
  the code and its callers in the same change.
- No history in comments: no "previously", "now uses", "changed from", "new
  implementation" or references to changes and pull requests. Git records
  history.
- Keep docstrings and comments short and factual. A docstring states what the
  item is or does, its contract (inputs, outputs, errors, invariants) and,
  where it is not obvious, why. A comment explains why, never what the next
  line plainly does.
- No filler: no restating the item's name, no marketing adjectives, no
  narrative, no lists of everything the item does not do. If a sentence can be
  deleted without losing information, delete it.

## Gates

Before asking for review, the change must pass, from a clean checkout of the
exact commit:

- `cargo fmt --check`, `cargo clippy --workspace --all-targets -- -D warnings`,
  `cargo test --workspace` and `cargo doc --no-deps` with
  `RUSTDOCFLAGS="-D warnings"`;
- dependency checks: core crates must not pull in Arrow, DataFusion or Sail;
- the public API snapshot check;
- `openspec validate --all --strict`;
- Ruff and mypy for Python.

Report exactly what ran and what did not. Never describe an unrun gate as
passing.

## Branches live in forks; upstream stays clean

The upstream repository, `querygraph/grustic`, holds only `main` and the
branches and tags its maintainers create. It never receives feature branches.

If you make commits yourself:

- Commit and push only to the operator's fork. Create feature branches there,
  one per OpenSpec change, and open the pull request from the fork to
  upstream `main`.
- Never push to upstream, and never create branches or tags there, even when
  your credentials allow it.
- Before the first push, check which remote points where (`git remote -v`). If
  no fork remote exists or it is unclear which remote is the fork, stop and ask
  the operator. Do not guess, and do not create a fork yourself. Record the
  answer in `.local/agents/`.

## Boundaries

- Do not publish crates, open pull requests on other repositories, or contact
  collaborators unless the operator asks for that specific action.
- Do not change other working checkouts or push branches you were not asked to
  push.
- Never put credentials, hostnames, local paths or personal setup in tracked
  files. They belong in `.local/agents/`.
