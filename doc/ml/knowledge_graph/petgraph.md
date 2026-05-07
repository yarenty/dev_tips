---
title: petgraph — Rust graph data-structure library
main_link: https://github.com/petgraph/petgraph
keywords: [petgraph, rust, graph, graphmap, stablegraph, matrixgraph, csr, dijkstra]
status: reviewed
review_date: 2026/05/03
---

# petgraph — Rust graph data-structure library

**Main link:** <https://github.com/petgraph/petgraph>

## Summary

`petgraph` is the **canonical Rust graph data-structure crate** — adjacency-list (`Graph`, `StableGraph`), hash-backed (`GraphMap`), dense matrix (`MatrixGraph`), and sparse-CSR (`CSR`) variants, plus the standard inventory of graph algorithms (Dijkstra, Bellman-Ford, A*, BFS/DFS, Tarjan SCC, topological sort, minimum spanning tree, isomorphism). It's the substrate used by `cargo`'s dependency resolver, `cargo-deps`, `rustc-ap-graphviz` and a long tail of compilers / planners / static-analysis tools in the Rust ecosystem. Filed under `ml/knowledge_graph/` because graph data-structures are the substrate for KG / GNN work — though most ML uses Python (NetworkX / DGL / PyG) rather than petgraph.

## Insight

When to reach for it: any Rust project that needs **in-process graph data structures with algorithms attached** — compiler IRs, dependency graphs, build systems, planners, finite state machines, social-network analytics, knowledge-graph indexing layers. `petgraph` has been the default since the early Rust days (Ulrik Sverdrup / @bluss); it's stable, MSRV-conscious, no surprises.

The picker among petgraph's graph types:

- **`Graph<N, E, Ty, Ix>`** — adjacency-list. Default pick. Indices are `NodeIndex<u32>` so you can have ~4B nodes per graph.
- **`StableGraph`** — same shape but indices stay valid across `remove_node`. Pay a small overhead; reach for it when you delete nodes mid-traversal.
- **`GraphMap<N, E, Ty>`** — node IDs *are* the keys (e.g. `&str`, `String`, custom hashable type), no separate index lookup. Best when your nodes have natural identifiers.
- **`MatrixGraph` / `CSR`** — adjacency matrix and compressed-sparse-row. Reach for these only on dense or static graphs where the matrix layout matters for cache-locality / algorithm requirements.

What `petgraph` is *not*: a graph **database** (no on-disk storage, no query language), a **persistent** structure (mutations are in-place; if you want immutable persistent graphs use `im` or roll your own), or a **GraphQL** / traversal-query layer (use `[[programming/rust/data/trustfall|trustfall]]` for that, or one of the Cypher-in-Rust efforts). For *distributed* graph processing think `[[programming/rust/data/datafusion/README|datafusion]]` + Substrait, not petgraph.

Comparison with the broader Rust graph ecosystem:

- **`petgraph`** — in-process data structures + classical algorithms. The default.
- **`trustfall`** — query engine over arbitrary data sources (cargo-semver-checks substrate). *Different layer*.
- **`graphlib`** — older, less feature-complete; petgraph won.
- **`pathfinding`** — focused on path-finding algorithms (A*, Dijkstra) over user-defined graphs without owning a graph type. Pairs nicely with petgraph or with your own representation.
- **`gix-revwalk`** — `gitoxide`'s git-history walker; specialised, not general.

For ML/AI specifically: petgraph is the *right* substrate when you need a Rust-native graph in a Rust pipeline, but most actual KG / GNN work happens in Python. If you want to do GNNs in Rust, you'd combine petgraph (data structure) with `candle` / `tch-rs` (tensor compute). The Rust GNN ecosystem is small.

## Similar / related topics

- **NetworkX** (Python) — the closest equivalent in scope; widely used in ML / network-science.
- **DGL / PyG** (Python) — graph neural networks; petgraph is the data structure, these are the *learning* libraries.
- **`pathfinding`** crate — Rust path-finding algorithms over user graphs.
- **`trustfall`** — Rust query engine over arbitrary data; *different layer* from petgraph.
- **PyTorch-BigGraph** (Meta) — large-scale KG embedding training (mentioned in [[meta]]).

## Internal links

- [[README]] — section landing.
- [[meta]] — sibling: data2vec / doc2vec embedding notes.
- [[ml/knowledge_graph/papers|kg/papers]] — sibling: KG research reading list.
- [[../fundamentals/graphnn|graphnn]] — Graph Neural Networks landing (PyG / DGL — the Python-side counterparts).
- [[programming/rust/data/trustfall|trustfall]] — Rust *query engine* over arbitrary data sources; complements petgraph (different layer).

## Keywords

`#petgraph` `#rust` `#graph` `#graphmap` `#stablegraph` `#matrixgraph` `#csr`

## References / raw notes

- Repo: <https://github.com/petgraph/petgraph>
- Docs: <https://docs.rs/petgraph>
- Crates.io: <https://crates.io/crates/petgraph>
- Maintainer: Ulrik Sverdrup (`@bluss`) and contributors.
- Origins: one of the oldest still-active Rust libraries; predates Rust 1.0.
- MSRV: Rust 1.64+ (recent versions).

### Crate feature flags

- `graphmap` (default) — enables `GraphMap`.
- `stable_graph` (default) — enables `StableGraph`.
- `matrix_graph` (default) — enables `MatrixGraph`.
- `serde-1` (optional) — serialization for `Graph` / `StableGraph` / `GraphMap` via serde 1.0.
- `rayon` (optional) — parallel iterators for `GraphMap`.

### Graph type cheat-sheet

| Type | Storage | Indices stable across removals? | When to use |
|------|---------|-------------------------------|-------------|
| `Graph<N, E, Ty>` | Adjacency list | No | Default; static or grow-only graphs |
| `StableGraph<N, E, Ty>` | Adjacency list | Yes | Graphs that delete nodes mid-traversal |
| `GraphMap<N, E, Ty>` | Adjacency list backed by HashMap | N/A (keys are node IDs) | Nodes have natural keys (strings, IDs) |
| `MatrixGraph<N, E, Ty>` | Adjacency matrix | No | Dense graphs, fixed structure |
| `CSR<N, E, Ty>` | Compressed sparse row | No | Read-heavy sparse graphs, cache-locality |

### Generic parameters

- `N` — node weight type (arbitrary; can be `&str`, `()`, custom struct).
- `E` — edge weight type (arbitrary; algorithms requiring costs need `PartialOrd`).
- `Ty` — `Directed` or `Undirected`.
- `Ix` — index integer type (`u8`, `u16`, `u32` default, or `usize`).

### Shorthand types

- `DiGraph<N, E>` = `Graph<N, E, Directed>`.
- `UnGraph<N, E>` = `Graph<N, E, Undirected>`.
- `UnMatrix<N, E>` = `MatrixGraph<N, E, Undirected>`.
- See each module's docs for the full list.

### Algorithms (non-exhaustive)

`dijkstra`, `bellman_ford`, `astar`, `bfs`, `dfs`, `dfs_post_order`, `tarjan_scc`, `kosaraju_scc`, `toposort`, `min_spanning_tree`, `is_isomorphic`, `connected_components`, `floyd_warshall`, `johnson`, `k_shortest_path`, `page_rank`, `feedback_arc_set` — see `petgraph::algo`.
