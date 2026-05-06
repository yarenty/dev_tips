---
title: Federated query execution
keywords: [federated-query, query-execution, trino, presto, calcite, dremio]
status: reviewed
---

# Federated query execution

Notes on **federated query execution** architecture — i.e. how a federated
SQL engine actually pushes work down to its backends, plans the cross-source
join, and stitches the results back together. The canonical references for
the topic are external.

## External entry points

- [Trino](https://trino.io/) — modern fork of Presto; canonical federated
  SQL engine.
- [Presto](https://prestodb.io/) — original Facebook federated engine.
- [Apache Calcite](https://calcite.apache.org/) — relational-algebra
  framework / pluggable optimizer used by many federated engines.
- [Dremio](https://www.dremio.com/) — Arrow-native federated query
  platform.

## Cross-section see-also

- [[db/distributed/federated/README|federated]] — sibling notes.
- [[db/distributed/ballista_distributed/README|ballista_distributed]] —
  related distributed-execution architecture (single-engine rather than
  federated).
- [[db/distributed/README|distributed]] — parent section.

## Keywords

`#federated-query` `#query-execution` `#trino` `#presto` `#calcite` `#dremio`
