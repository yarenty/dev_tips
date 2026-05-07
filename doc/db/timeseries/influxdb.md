---
title: "InfluxDB — multi-generation time-series DB (1.x / 2.x / 3.x IOx)"
main_link: https://www.influxdata.com/products/influxdb/
keywords: [influxdb, timeseries, metrics, iox, datafusion, flux, influxql, rust]
status: reviewed
---

# InfluxDB — multi-generation time-series DB (1.x / 2.x / 3.x IOx)

**Main link:** <https://www.influxdata.com/products/influxdb/>

Docs: <https://docs.influxdata.com/> · 3.x (IOx) blog: <https://www.influxdata.com/blog/influxdb-3-0-system-architecture/>

## Summary

InfluxDB is a purpose-built time-series database from InfluxData, currently shipping in **three substantially different major versions** that share little more than a name and an HTTP port:

- **1.x** — Go, custom TSM storage engine, InfluxQL query language. The classic; still widely deployed.
- **2.x** — Go, TSM storage, **Flux** as the new query language (functional, pipe-shaped). Bundles Telegraf-like data ingest, dashboards, alerts. The version most older blog posts refer to.
- **3.x (formerly IOx)** — full **Rust** rewrite on top of Apache Arrow, DataFusion, and Parquet, with object-storage-first design and SQL as a first-class query language. Effectively a different database that happens to wear the InfluxDB badge.

This three-way split is the single most confusing thing about InfluxDB. Tutorials, plugins, and operational guides may refer to any of the three; double-check what you're reading.

## Insight

How to think about which one to pick today:

- **Greenfield, cloud-native, want SQL and Parquet:** InfluxDB 3 (Cloud Serverless / Cloud Dedicated / Clustered / Edge). Object-storage-first means cheap retention; SQL means tools just work.
- **You inherited an InfluxDB 1.x or 2.x deployment:** stay there until you have a reason to move; data migration between major versions is non-trivial.
- **You really want Flux:** stay on 2.x. Flux is being de-emphasised in 3; SQL is the future.
- **You're on Postgres already:** seriously consider TimescaleDB — hypertables + continuous aggregates cover most TSDB needs without adding another database.
- **Your workload is "scrape Prometheus exporters":** just use [[prometheus]] + Mimir/Thanos/VictoriaMetrics. InfluxDB is overkill for plain SRE metrics.
- **Your workload is OLAP-on-time** (millions of events/sec, dashboards): [[db/timeseries/greptimedb|GreptimeDB]], ClickHouse, [[druid]] are stronger fits.

The 3.x rewrite is genuinely interesting as an architectural exhibit — it's one of the highest-profile real-world systems built on Arrow + DataFusion + Parquet, and a useful reference point alongside [[db/timeseries/greptimedb|GreptimeDB]] and [Apache DataFusion](https://datafusion.apache.org/) itself.

## Operational notes (Homebrew, macOS — InfluxDB 2.x)

```sh
# Install via brew
brew install influxdb

# Start as a service
brew services start influxdb

# Or run in foreground
INFLUXD_CONFIG_PATH=/usr/local/etc/influxdb2/config.yml influxd

# Default UI:
open http://localhost:8086/

# Restart after upgrade
brew services restart influxdb

# Or, no service:
/usr/local/opt/influxdb/bin/influxd
```

### First-time setup (2.x)

The 2.x onboarding flow at `http://localhost:8086/` walks you through:

1. Click **Get Started**.
2. Set up an initial user (e.g. `yarenty`).
3. Set username/password for that user (e.g. `q1t5`).
4. Initial **organization name** (e.g. `com.yarenty.metrics`).
5. Initial **bucket name** (e.g. `metrics`).
6. Click Continue.

InfluxDB is then initialised with a primary user, organisation, and bucket, and you can start writing or scraping data.

### Sample startup log

A typical 2.x startup looks like:

```
==> Summary
🍺  /usr/local/Cellar/influxdb/2.0.7: 9 files, 215.5MB
[..] : influxd
2021-06-17T12:00:11Z  info  Welcome to InfluxDB  {"version": "2.0.7", "build_date": "2021-06-17T12:00:02Z"}
2021-06-17T12:00:12Z  info  Resources opened     {"service": "bolt", "path": "~/.influxdbv2/influxd.bolt"}
2021-06-17T12:00:12Z  info  Bringing up metadata migrations  {"migration_count": 15}
2021-06-17T12:00:15Z  info  Using data dir       {"path": "~/.influxdbv2/engine/data"}
2021-06-17T12:00:16Z  info  Listening            {"transport": "http", "addr": ":8086"}
```

## Similar / related topics

- [TimescaleDB](https://www.timescale.com/) — Postgres extension; usually the right answer if you're already on Postgres.
- [[db/timeseries/greptimedb|GreptimeDB]] — Rust + Arrow + DataFusion + object storage; same architectural school as InfluxDB 3.
- [[questdb]] — high-write-rate Java time-series DB; financial / IoT niche.
- [[druid]] — OLAP on time-bucketed events at large scale.
- [[prometheus]] — for scrape-style operational metrics.

## Internal links

- [[db/timeseries/greptimedb|GreptimeDB]]
- [[questdb]]
- [[druid]]
- [[prometheus]]
- [[postgresql]]

## Keywords

`#influxdb` `#timeseries` `#tsdb` `#iox` `#datafusion` `#arrow` `#parquet` `#flux` `#sql` `#db`
