---
title: "Apache Druid — real-time OLAP on time-bucketed events"
main_link: https://druid.apache.org/
keywords: [druid, olap, timeseries, analytics, real-time, java, columnar, apache]
status: reviewed
---

# Apache Druid — real-time OLAP on time-bucketed events

**Main link:** <https://druid.apache.org/>

Quickstart tutorial: <https://druid.apache.org/docs/latest/tutorials/index.html> · Repo: <https://github.com/apache/druid>

## Summary

Apache Druid is an open-source distributed analytical database built for **real-time analytics on time-bucketed event data**: clickstreams, ad serving, user behaviour, application telemetry, security events. It combines a column-oriented storage layer (segments are time-partitioned, then column-compressed), a streaming + batch ingestion model (Kafka, Kinesis, S3, HDFS), and a query engine optimised for sub-second aggregations across billions of rows. Java-based, multi-process architecture (Coordinator, Overlord, Broker, Historical, MiddleManager, Router).

Originally built at Metamarkets (2011), Apache top-level project since 2018, commercially backed by Imply.

## Insight

Druid's natural habitat is the "interactive dashboard over enormous event streams" job:

- **Hundreds of millions to trillions of events**, partitioned by time.
- **Many concurrent queries** that aggregate, filter, and group-by — typical analytics dashboards.
- **A real-time + historical mix:** new events arriving from Kafka and immediately queryable, old events compacted into long-term segments.
- **High concurrency** with sub-second p95 latency on aggregation queries.

Where Druid is the wrong tool:

- **Pure metrics scraping** — [[prometheus]] is the boring default.
- **Application time-series with low-to-medium write rate** — [[influxdb]] / [[questdb]] / [[db/timeseries/greptimedb|GreptimeDB]] / TimescaleDB are simpler.
- **General-purpose OLAP without the real-time requirement** — ClickHouse will be smaller, faster, and operationally simpler. ClickHouse has eaten significant Druid market share since 2020.
- **Anything transactional** — Druid is append-mostly; updates and deletes are second-class.

The operational footprint is the main thing to weigh: Druid is a **multi-process distributed system** that wants ZooKeeper and a metadata store. It's not a single binary. For a small team, ClickHouse or [[db/timeseries/greptimedb|GreptimeDB]] is far less work to run; for a large data platform team that needs the throughput / concurrency profile, Druid still earns its keep.

Compare also: [Apache Pinot](https://pinot.apache.org/) (very similar niche, originally from LinkedIn) and [StarRocks](https://www.starrocks.io/) (newer C++ MPP).

## Quickstart (micro)

Download Druid from the project page and unpack it. Then:

```sh
./bin/start-micro-quickstart
```

Open the unified console:

```
http://localhost:8888/unified-console.html
```

### Cleaning up between tutorials

After stopping Druid services you can start fresh by deleting the `var/` directory under the Druid root and re-running `bin/start-micro-quickstart`. Useful before working through a different ingestion tutorial that creates the same datasource (e.g. the Wikipedia example).

## Similar / related topics

- [ClickHouse](https://clickhouse.com/) — leaner column-store OLAP; usually the default-better choice for new deployments.
- [Apache Pinot](https://pinot.apache.org/) — closest peer; very similar architecture, originally LinkedIn-built.
- [StarRocks](https://www.starrocks.io/) — newer C++ MPP analytical DB.
- [[db/timeseries/greptimedb|GreptimeDB]] — Rust hybrid TSDB with overlapping ambitions; lighter ops.
- [[influxdb]] — InfluxDB 3 (IOx) overlaps for the time-series-shaped subset.

## Internal links

<!-- reviewed -->

- [[influxdb]]
- [[questdb]]
- [[db/timeseries/greptimedb|GreptimeDB]]
- [[postgresql]]

## Keywords

`#druid` `#olap` `#timeseries` `#real-time-analytics` `#columnar` `#apache` `#java` `#analytics` `#db`
