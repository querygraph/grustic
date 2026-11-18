# Design: extension mechanism end to end

## Starting candidates

These shapes are the starting point for the spike, carried over from the
earlier design review. They are sketches, not frozen signatures. The
spike replaces or confirms each one and records its findings below.

### Session

A session owns everything an application registers. Building it checks for
conflicting registrations and returns a typed error, so a misconfigured
session never reaches planning. Planning ends at a physical plan; each backend
wraps the planning state in its own executing session.

```rust
let state = SessionStateBuilder::new()
    .with_catalog("main", crm_and_social_catalog)
    .with_function(ai_score_def())
    .with_syntax(TemporalPathSyntax)
    .with_resolver(TemporalPathResolver)
    .with_rule(TemporalWindowPushdown)
    .with_backend(ReferenceBackend::default())
    .build()?; // Err(SessionError::DuplicateKeyword { .. }), etc.

let physical = state.plan(Query::cypher(text))?; // engine-free
let batches = reference.execute(&physical, &exec_ctx).await?; // backend crate
```

Resolution is a pure function of the query and a catalog snapshot. Remote
catalogs are read by an earlier asynchronous step that collects referenced
names and fixes the snapshot.

### Syntax

Fixed hook points where an extension may claim the next tokens, following
sqlparser-rs dialects. Keywords are contextual by default. Two extensions
claiming the same keyword at the same hook point is a build error, never
first-registered-wins. An extension AST node with no registered resolver is
refused with a diagnostic at its span.

```rust
pub trait SyntaxExtension: Send + Sync {
    fn name(&self) -> &ExtensionName;            // e.g. "acme.temporal@1"
    fn keywords(&self) -> &[ContextualKeyword];
    fn parse_clause(&self, p: &mut Parser<'_>) -> Option<ParseResult<ExtClause>> { None }
    fn parse_pattern(&self, p: &mut Parser<'_>) -> Option<ParseResult<ExtPattern>> { None }
    fn parse_expr_prefix(&self, p: &mut Parser<'_>) -> Option<ParseResult<ExtExpr>> { None }
}
```

### Functions

One semantic definition; each backend registers its own implementation.
Volatility limits which rules may move or deduplicate a call; the cost hint
orders it after cheaper filters.

```rust
FunctionDef::scalar("ai_score")
    .signature([LogicalType::Int64, LogicalType::Utf8], LogicalType::Float64)
    .nulls(NullBehavior::NullOnNullInput)
    .volatility(Volatility::Stable)
    .cost_hint(CostHint::Expensive)
```

### Catalog

```rust
pub enum CatalogEntry {
    Graph(Arc<dyn GraphSource>),
    Extension(Arc<dyn ExtensionEntry>),  // e.g. relational tables, documents
}
```

A `GraphSource` describes schema groups, identity namespaces and
capabilities: label scans, neighbor lookup, adjacency layouts and sort keys,
indexes, partitioning, and optional statistics with provenance. An extension
entry kind supplies its own scan node and value types.

### Logical nodes and rules

Built-in operators stay readable enums with one `Extension` variant holding a
trait object. Rebuilding a node with new children re-runs its checks. An
extension node is a barrier to every rule that does not claim it.

```rust
pub trait Rule: Send + Sync {
    fn name(&self) -> &RuleName;
    fn phase(&self) -> Phase;                       // Analyze | Logical | Physical
    fn understands(&self, kind: &NodeKind) -> bool;
    fn apply(&self, plan: &Plan, cx: &RuleContext<'_>) -> Result<Transformed<Plan>, RuleError>;
}
```

### Physical properties, implementations and conversions

```rust
pub trait PhysicalProperty: Send + Sync {
    fn id(&self) -> PropertyId;
    fn satisfies(&self, required: &dyn PhysicalProperty) -> bool;
}

pub trait Implementation: Send + Sync {
    fn implements(&self) -> &NodeKind;
    fn convention(&self) -> &ConventionId;
    fn required_inputs(&self, node: &Plan) -> Vec<RequiredProperties>;
    fn provides(&self, node: &Plan, inputs: &[ProvidedProperties]) -> ProvidedProperties;
    fn cost(&self, node: &Plan, stats: &Statistics) -> Cost;   // Unknown stays unknown
}

pub trait Conversion: Send + Sync {
    fn from(&self) -> &ConventionId;
    fn establishes(&self) -> &dyn PhysicalProperty;
    fn cost(&self, input: &Statistics) -> Cost;
}
```

When an implementation requires a property its input lacks, the planner
inserts a registered, costed conversion, or refuses if none exists or it
would exceed the memory budget.

### Distribution

```rust
pub trait ExtensionCodec: Send + Sync {
    fn encode(&self, node: &dyn ExtensionNode, out: &mut Vec<u8>) -> Result<(), CodecError>;
    fn decode(&self, name: &ExtensionName, version: u32, bytes: &[u8],
              children: Vec<Plan>) -> Result<Plan, CodecError>;
}
```

The envelope carries extension name and version; an unknown or mismatched
extension is refused before execution. No cross-release stability is
promised.

## Open decisions for this spike

1. Extension naming and versioning: `vendor.name@version` is proposed.
2. Whether syntax extensions may add reserved keywords, or only contextual
   ones.
3. Rule ordering: before and after constraints between named rules, or fixed
   named stages.

## Alternatives already rejected

| Alternative | Why not |
| --- | --- |
| Closed enums; extend by forking | Fails the extensibility requirement directly |
| Trait objects for every node, including built-ins | Loses exhaustive matching and readable rules for little gain over one extension variant |
| DataFusion's `LogicalPlan` with extension nodes as the core | Couples every consumer to DataFusion releases |
| Composable parser grammar | Harder to diagnose and to make conflict-safe than fixed hook points |
| Calcite itself through the JVM | Not usable as a Rust library; excludes engine-free embedding |

## Findings

To be written by the spike.
