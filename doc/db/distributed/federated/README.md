---
title: Federated query (placeholder)
keywords: [federated-query, trino, presto, calcite, fedml]
status: reviewed
---

# Federated query (placeholder)

Placeholder folder for future notes on **federated query systems** — engines
that present a single SQL surface over multiple heterogeneous backends
(relational + NoSQL + cloud storage + APIs) without first physically
consolidating the data.

The canonical examples in this space are [Trino](https://trino.io/),
[Presto](https://prestodb.io/), and [Apache Calcite](https://calcite.apache.org/);
on the federated-ML side, [FedML](https://fedml.ai/) and similar projects
extend the same idea to model training over partitioned, privacy-sensitive
data.

## External entry points

- [Trino docs](https://trino.io/docs/current/)
- [Presto docs](https://prestodb.io/docs/current/)
- [Apache Calcite](https://calcite.apache.org/)
- [FedML](https://fedml.ai/)

## Cross-section see-also

- [[db/distributed/federated_query_execution/README|federated_query_execution]] —
  sibling folder with whiteboard captures on the execution side.
- [[db/distributed/README|distributed]] — parent section.

## Keywords

`#federated-query` `#trino` `#presto` `#calcite` `#placeholder`
