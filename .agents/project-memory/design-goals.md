# Design goals for extensions

Three scenarios state what a third party must be able to build with Grustic's
extension points. They guide the design of every hook. They are **not
deliverables**: Grustic does not ship these extensions, and their syntax and
semantics belong to whoever writes them. Proposals cite them as G1, G2 and G3.
Each is exercised by a small test-only extension in the change that
implements the hooks it needs, with independently computed answers.

## G1: a user-defined function in a graph query

Given an application registers `ai_score(Int64, Utf8) -> Float64` with an
implementation for its backend,
when it runs
`MATCH (n)-[e]-(p:P) WHERE ai_score(p.age, p.region) > 0.3 RETURN id(n)`,
then the query resolves, plans and executes with no change to the parser or
core crates. On a backend without an implementation, planning refuses with a
typed error naming the function and the backend.

Hooks: function definitions with volatility and cost hints; per-backend
implementation registration.

## G2: temporal path expansion with a reordered CSR

Given an extension adds time-respecting path syntax and semantics, and needs
adjacency sorted by an edge timestamp,
when a query uses that syntax,
then the extension parses, resolves and plans it, and the planner inserts its
costed CSR re-sort only when no available projection is already sorted that
way. None of this edits core match statements.

Hooks: syntax extension, resolver extension, logical extension node, optimizer
rule (for example pushing time-window predicates into the path), physical
property requirement, and a conversion whose cost includes peak memory during
construction.

## G3: relational tables in the catalog

Given an extension adds relational tables as a catalog entry kind,
when a query joins graph results with such a table,
then the join uses the core relational operators, and the table is not
converted into a graph. A document collection entry kind is the same scenario
with nested values.

Hooks: extension catalog entry kinds with their own scan node and value types;
the core join, projection and aggregation operators.
