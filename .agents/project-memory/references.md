# References

External sources that shaped Grustic's direction. They explain why the
requirements look the way they do; they are not requirements themselves.
Exact source revisions for ported code are recorded in the proposal of the
change that ports it.

## Original direction from Sem

- [Architecture clarification on Grust PR 36](https://github.com/querygraph/grust/pull/36#issuecomment-6000697247):
  independently usable graph and query components, a separate assembled
  engine, and separate local and distributed algorithms. Avoid assumptions
  that prevent future graph-schema or storage-format support, and do not
  declare conformance to an evolving LEX proposal.
- [Earlier answers on the same PR](https://github.com/querygraph/grust/pull/36#issuecomment-5992818702):
  one owning schema group per population, feature alignment, working-set
  estimates, reverse adjacency, host-provided execution, multiple parse
  diagnostics, and a function registry before Sail integration.

## Architectural models

- [Calcite adapter architecture](https://calcite.apache.org/docs/adapter.html):
  replaceable rules, metadata and execution conventions, with costed
  conversions between conventions.
- [DataFusion extension operators](https://datafusion.apache.org/library-user-guide/extending-operators.html):
  integration points for custom logical and physical operators and optimizer
  rules. Nutmeg hosts Grustic plans through them.
- [GraphAr format](https://graphar.apache.org/docs/specification/format/):
  logical entities, physical property bundles, chunked storage and adjacency
  layouts must stay distinguishable in the schema model.
