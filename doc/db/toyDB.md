# toyDB


**https://github.com/erikgrinaker/toydb/tree/main**

Distributed SQL database in Rust, built from scratch as an educational project. Main features:

- Raft distributed consensus for linearizable state machine replication.

- ACID transactions with MVCC-based snapshot isolation.

- Pluggable storage engine with BitCask and in-memory backends.

- Iterator-based query engine with heuristic optimization and time-travel support.

- SQL interface including joins, aggregates, and transactions.

I originally wrote toyDB in 2020 to learn more about database internals. Since then, I've spent several years building real distributed SQL databases at CockroachDB and Neon. Based on this experience, I've rewritten toyDB as a simple illustration of the architecture and concepts behind distributed SQL databases.

toyDB is intended to be simple and understandable, and also functional and correct. Other aspects like performance, scalability, and availability are non-goals -- these are major sources of complexity in production-grade databases, and obscure the basic underlying concepts. Shortcuts have been taken where possible.