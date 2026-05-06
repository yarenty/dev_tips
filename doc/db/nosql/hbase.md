---
title: "HBase — Apache wide-column store from the Hadoop era"
main_link: https://hbase.apache.org/
keywords: [hbase, hadoop, hdfs, wide-column, bigtable, java, legacy]
status: reviewed
---

# HBase — Apache wide-column store from the Hadoop era

**Main link:** <https://hbase.apache.org/>

Docs: <https://hbase.apache.org/book.html> · Bigtable paper (the design ancestor): <https://research.google/pubs/pub27898/>

## Summary

Apache HBase is an open-source, distributed, wide-column NoSQL store modelled on Google's Bigtable paper, designed to sit on top of HDFS as part of the Hadoop ecosystem. Sparse, sorted, multi-dimensional maps indexed by `(row, column-family:qualifier, timestamp)`. Originally aimed at petabyte-scale random read/write on commodity hardware. Java; ZooKeeper-coordinated; integrates tightly with Hadoop, Hive, MapReduce, and Spark.

## Insight

Honest take: **rarely the right pick for a new project in 2025.** HBase is the database equivalent of a vintage tractor — it works, it's reliable in the right hands, but the operational overhead and ecosystem assumptions are hard to justify when better options exist.

When HBase still makes sense:

- **You already operate a Hadoop estate** with HDFS, ZooKeeper, YARN, and Hive. Adding HBase has marginal cost; standing it up green does not.
- **Big-row, wide-column workloads with timestamps as a first-class dimension** (some analytics, some IoT historical archives).
- **Tight integration with MapReduce / Spark batch jobs** that already run on the same Hadoop cluster.

When you should pick something else:

- **For a new wide-column / KV workload at scale**: [Apache Cassandra](https://cassandra.apache.org/) (peer-to-peer, no HDFS dependency, simpler ops) or [ScyllaDB](https://www.scylladb.com/) (C++ rewrite of Cassandra, dramatically faster).
- **For "cloud-native Bigtable"**: Google Cloud Bigtable directly, or AWS DynamoDB.
- **For time-series / observability**: [[db/timeseries/greptimedb|GreptimeDB]], [[influxdb]], ClickHouse, VictoriaMetrics.
- **For "I just want a fast KV store"**: [[redis]] / Valkey (in-memory) or RocksDB-based stores like TiKV.
- **For analytics on rows + cols**: ClickHouse, [[druid]], [[questdb]].

The Hadoop ecosystem itself is in long decline. Cloudera/Hortonworks have consolidated, MapReduce is rare in new pipelines (Spark replaced it), HDFS is mostly being replaced by object storage (S3 / GCS / R2). HBase's centre of gravity has moved with it. If you're not already there, don't go there.

## Operational notes (Homebrew, macOS — historical)

```sh
# Start with brew services
brew services start hbase

# Or run in foreground (the kitchen-sink form Homebrew prints):
HBASE_HOME="/usr/local/opt/hbase/libexec" \
HBASE_IDENT_STRING="root" \
HBASE_LOG_DIR="/usr/local/var/hbase" \
HBASE_LOG_PREFIX="hbase-root-master" \
HBASE_LOGFILE="hbase-root-master.log" \
HBASE_MASTER_OPTS=" -XX:PermSize=128m -XX:MaxPermSize=128m" \
HBASE_NICENESS="0" \
HBASE_OPTS="-XX:+UseConcMarkSweepGC" \
HBASE_PID_DIR="/usr/local/var/run/hbase" \
HBASE_REGIONSERVER_OPTS=" -XX:PermSize=128m -XX:MaxPermSize=128m" \
HBASE_ROOT_LOGGER="INFO,RFA" \
HBASE_SECURITY_LOGGER="INFO,RFAS" \
/usr/local/opt/hbase/bin/hbase --config /usr/local/opt/hbase/libexec/conf master start
```

The `PermSize` flags are JVM 7 vintage; modern JVMs (8+) ignore them. Treat the above as a relic of when HBase was easier to install via brew than to set up properly.

## Similar / related topics

- [Google Bigtable paper (2006)](https://research.google/pubs/pub27898/) — the design HBase is modelled on; still worth reading.
- [Apache Cassandra](https://cassandra.apache.org/) — peer-to-peer wide-column without the HDFS baggage.
- [ScyllaDB](https://www.scylladb.com/) — C++ Cassandra-compatible rewrite, much faster.
- [Google Cloud Bigtable](https://cloud.google.com/bigtable) / [AWS DynamoDB](https://aws.amazon.com/dynamodb/) — managed equivalents.
- [[redis]] — when in-memory KV is what you actually need.

## Internal links

<!-- reviewed -->

- [[redis]]
- [[postgresql]]
- [[db/timeseries/greptimedb|GreptimeDB]]

## Keywords

`#hbase` `#hadoop` `#hdfs` `#wide-column` `#bigtable` `#java` `#legacy` `#db`
