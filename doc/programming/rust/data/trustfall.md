---
title: Trustfall — query engine over arbitrary data sources
main_link: https://github.com/obi1kenobi/trustfall
keywords: [trustfall, query-engine, graph, graphql, adapter]
status: reviewed
review_date: 2026/05/03
---

# Trustfall — query engine over arbitrary data sources

**Main link:** <https://github.com/obi1kenobi/trustfall>

## Summary

Trustfall is a query engine for **any kind of data source** — APIs, databases, files on disk, GitHub repos, RSS feeds, even AI models — created by Predrag Gruevski (also the author of `cargo-semver-checks`, which is itself built on Trustfall). You write GraphQL-like queries with extra constructs (filters, recursion, type coercions), implement an `Adapter` trait that yields an iterator of vertices and edges from your data source, and Trustfall handles the rest. The model is closer to a **GraphQL-over-anything** layer than to a SQL or graph DB.

## Insight

Reach for Trustfall when you have **structured data scattered across heterogeneous sources** and want a single query language that joins across them — the canonical demo is `cargo-semver-checks`, which queries Rust crate ASTs to find breaking-change patterns; other adapters in the wild query HackerNews APIs, Wikipedia, RSS feeds, file-system trees, and crates.io metadata. Compared to GraphQL: Trustfall queries can recurse, filter on edges, and traverse arbitrary depth. Compared to graph databases (Neo4j / SurrealDB): Trustfall has no storage layer of its own — it's a *query engine*, you bring the data adapter. Compared to SQL: it's column-traversal-style rather than relational-algebra-style. Gotchas: adapter performance is entirely your responsibility (no automatic pushdown to the source); the query language has a learning curve even for SQL/GraphQL veterans; the project is small and one-maintainer. Try it in the browser at <https://play.predr.ag/>.

## Similar / related topics

- GraphQL — closest mainstream cousin; Trustfall is GraphQL-shaped but more powerful.
- Cypher / Gremlin — graph query languages; Trustfall is similarly traversal-based but adapter-driven.
- `jq` — for JSON-on-disk; Trustfall wins on cross-source joins and typed schemas.
- `cargo-semver-checks` — flagship Trustfall application worth studying.

## Internal links
- [[meilisearch]]
- [[gluesql]]
- [[sqlparser]]

## Keywords

`#trustfall` `#query-engine` `#graphql` `#graph` `#adapter`

## References / raw notes

- Repo: <https://github.com/obi1kenobi/trustfall>
- Crates.io: <https://crates.io/crates/trustfall>
- Playground: <https://play.predr.ag/>
- 10-min talk + demo: linked from the README.
- Real-world examples (HackerNews, RSS, Wikipedia, GitHub, crates.io adapters) in the trustfall org.

> Trustfall is a query engine for querying any kind of data source — from APIs and databases to files on disk and AI models.
