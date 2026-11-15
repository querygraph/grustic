# Project memory

Durable knowledge about Grustic that is neither a requirement nor obvious from
the code. Together with `openspec/specs/` and the code, these files must be
enough for any agent or person to understand the project from a fresh clone.

Agents maintain these files as part of the change that makes them true. A
pull request that changes the architecture, a convention or a term updates the
matching file in the same pull request.

## What belongs here

- The architecture map: components, dependency direction and where each
  extension point lives.
- A glossary of project terms.
- Conventions that the code follows but cannot state by itself.
- Non-obvious pitfalls, with the reason they matter.

## What does not belong here

- **Requirements.** Link to the capability in `openspec/specs/` instead of
  restating it. If a memory note and a spec disagree, the spec wins and the note
  is fixed.
- **Anything tied to one machine, person or session**: hostnames, paths
  outside the repository, environment setup, harness or model names, chat or
  session references. Those belong in `.local/agents/`, which is gitignored.
- **History**: commit hashes, branch names, pull request numbers, "recently
  changed" notes, or dates of work. Git already records history. Write what is
  true now.
- **Restatements of code** that a reader could get from the code or rustdoc.

## Format

One topic per file, kebab-case names, plain Markdown. Start each file with a
one-paragraph summary. Keep the index below current.

## Index

- [architecture-map.md](architecture-map.md): components, dependency direction
  and extension points.
- [glossary.md](glossary.md): project terms.
- [design-goals.md](design-goals.md): extension scenarios G1–G3 that guide
  hook design; not deliverables.
- [design-pitfalls.md](design-pitfalls.md): tempting choices that would break
  the foundation, with reasons.
- [references.md](references.md): original direction and the external
  architectures Grustic draws on.
