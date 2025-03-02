# Petgraph

https://github.com/petgraph/petgraph/tree/master

![petgraph](https://github.com/petgraph/petgraph/raw/master/assets/graphosaurus-512.png)

Graph data structure library. Please read the API documentation here.

Supports Rust 1.64 and later.

Crates.io docs.rs MSRV Discord chat build_status

Crate feature flags:

- graphmap (default) enable GraphMap.
- stable_graph (default) enable StableGraph.
- matrix_graph (default) enable MatrixGraph.
- serde-1 (optional) enable serialization for Graph, StableGraph, GraphMap using serde 1.0. Requires Rust version as required by serde.
- rayon (optional) enable parallel iterators for the underlying data in GraphMap. Requires Rust version as required by Rayon.



## Graph types
- Graph - An adjacency list graph with arbitrary associated data.
- StableGraph - Similar to Graph, but it keeps indices stable across removals.
- GraphMap - An adjacency list graph backed by a hash table. The node identifiers are the keys into the table.
- MatrixGraph - An adjacency matrix graph.
- CSR - A sparse adjacency matrix graph with arbitrary associated data.

## Generic parameters

Each graph type is generic over a handful of parameters. All graphs share 3 common parameters, N, E, and Ty. This is a broad overview of what those are. Each type’s documentation will have finer detail on these parameters.

N & E are called weights in this implementation, and are associated with nodes and edges respectively. They can generally be of arbitrary type, and don’t have to be what you might conventionally consider weight-like. For example, using &str for N will work. Many algorithms that require costs let you provide a cost function that translates your N and E weights into costs appropriate to the algorithm. Some graph types and choices do impose bounds on N or E. min_spanning_tree for example requires edge weights that implement PartialOrd. GraphMap requires node weights that can serve as hash map keys, since that graph type does not create standalone node indices.

Ty controls whether edges are Directed or Undirected.

Ix appears on graph types that use indices. It is exposed so you can control the size of node and edge indices, and therefore the memory footprint of your graphs. Allowed values are u8, u16, u32, and usize, with u32 being the default.

## Shorthand types
Each graph type vends a few shorthand type definitions that name some specific generic choices. For example, DiGraph<_, _> is shorthand for Graph<_, _, Directed>. UnMatrix<_, _> is shorthand for MatrixGraph<_, _, Undirected>. Each graph type’s module documentation lists the available shorthand types.

## Crate features
serde-1 - Defaults off. Enables serialization for Graph, StableGraph, GraphMap using serde 1.0. May require a more recent version of Rust than petgraph alone.
graphmap - Defaults on. Enables GraphMap.
stable_graph - Defaults on. Enables StableGraph.
matrix_graph - Defaults on. Enables MatrixGraph.



