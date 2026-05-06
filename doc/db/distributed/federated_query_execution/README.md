---
title: Federated query execution (working notes)
keywords: [federated-query, query-execution, trino, presto, calcite, screenshots]
status: reviewed
---

# Federated query execution (working notes)

Working-notes folder for **federated query execution** architecture — i.e.
how a federated SQL engine actually pushes work down to its backends, plans
the cross-source join, and stitches the results back together. The folder is
just a stash of screenshot captures (whiteboard / diagram session from
2024-03-06); the canonical references for the topic are external.

## What's in this folder

- `Screenshot 2024-03-06 at 17.36.25.png`
- `Screenshot 2024-03-06 at 17.37.11.png`
- `Screenshot 2024-03-06 at 17.38.03.png`
- `Screenshot 2024-03-06 at 17.38.30.png`
- `Screenshot 2024-03-06 at 17.38.49.png`
- `Screenshot 2024-03-06 at 17.40.28.png`

(All from the same working session — diagrams and whiteboard captures.)

## External entry points

- [Trino](https://trino.io/) — modern fork of Presto; canonical federated
  SQL engine.
- [Presto](https://prestodb.io/) — original Facebook federated engine.
- [Apache Calcite](https://calcite.apache.org/) — relational-algebra
  framework / pluggable optimizer used by many federated engines.
- [Dremio](https://www.dremio.com/) — Arrow-native federated query
  platform.

## Cross-section see-also

- [[db/distributed/federated/README|federated]] — sibling folder with the
  intro deck.
- [[db/distributed/ballista_distributed/README|ballista_distributed]] —
  related distributed-execution architecture (single-engine rather than
  federated).
- [[db/distributed/README|distributed]] — parent section.

## Keywords

`#federated-query` `#query-execution` `#trino` `#presto` `#whiteboard-notes`
