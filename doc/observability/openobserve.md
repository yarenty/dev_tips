---
title: "OpenObserve: Rust-based logs + metrics + traces in a single binary"
main_link: https://openobserve.ai/
keywords: [openobserve, observability, logs, metrics, traces, rust, elasticsearch-alternative, parquet, s3]
status: reviewed
---

# OpenObserve: Rust-based logs + metrics + traces in a single binary

**Main link:** <https://openobserve.ai/>

Repo: <https://github.com/openobserve/openobserve> · Docs: <https://openobserve.ai/docs/>

## Summary

[OpenObserve](https://openobserve.ai/) (the project, sometimes "O2") is a Rust-written, single-binary observability platform that handles **logs, metrics, traces, RUM (real-user monitoring), and front-end errors** in one process. It speaks the [OpenTelemetry](https://opentelemetry.io/) protocol natively, accepts data through Elasticsearch-compatible bulk APIs (so a Filebeat or Fluent Bit pipeline can target it without changes), stores everything as [Parquet](https://parquet.apache.org/) on local disk or S3-compatible object storage, and queries with SQL. The pitch is "Elasticsearch + Loki + Tempo + Grafana, but ~140× cheaper to run and one binary instead of seven".

## Insight

Reach for OpenObserve when:

- You're scared of the **operational complexity** of an ELK or Loki/Mimir/Tempo stack and you don't have a dedicated observability team.
- Your **storage cost** for logs is the line item that hurts. Parquet+S3 with column-pruning gets dramatic compression vs row-store inverted indices, and OpenObserve's published benchmarks (take with usual single-vendor scepticism) claim ~140× cost reduction on equivalent workloads.
- You're **already on OpenTelemetry** and want a backend that consumes OTLP natively without a sidecar collector translating formats.

Don't reach for OpenObserve when:

- You're invested in a **mature Elasticsearch** ecosystem (Kibana plugins, lots of bespoke ES queries, Logstash pipelines tuned over years). The Elasticsearch-compatibility layer covers ingest but not the full query API.
- You need the **giant Grafana ecosystem** of dashboards. OpenObserve has its own UI which is good but doesn't have anywhere near the [grafana.com/dashboards](https://grafana.com/grafana/dashboards/) library. (You *can* point Grafana at OpenObserve as a datasource — they support it — but then you've added back a piece you were trying to remove.)
- You're at very small scale. For a single-server homelab the ELK / Loki overhead is fine, and OpenObserve's specific advantages (Parquet+S3, single binary) are less of a win.

The interesting trend OpenObserve is part of: a generation of **Rust-rewritten data infrastructure** ([[redpanda]] for Kafka, [InfluxDB IOx](https://www.influxdata.com/blog/announcing-influxdb-iox/), [GreptimeDB](https://greptime.com/), [QuestDB](https://questdb.io/), OpenObserve here) that runs as one statically-linked binary, leans on object storage for cheap durability, and tends to be 5-50× more efficient than the JVM/Go incumbents on equivalent workloads. Worth tracking even if you don't deploy them.

## Similar / related topics

- [[observability/grafana|grafana]] + [[prometheus]] — the canonical alternative stack OpenObserve competes with (also add Loki for logs, Tempo for traces).
- [Elastic Stack (ELK)](https://www.elastic.co/elastic-stack) — the older "everything-in-one" alternative; OpenObserve's Elasticsearch-compat layer means existing ELK ingestion pipelines keep working.
- [Grafana Loki](https://grafana.com/oss/loki/) — Loki specifically for logs; cheaper than ES, but you still need Tempo + Mimir for traces and metrics.
- [SigNoz](https://signoz.io/) — another one-binary OTel-native observability stack; closest peer to OpenObserve.
- [VictoriaMetrics](https://victoriametrics.com/) + [VictoriaLogs](https://docs.victoriametrics.com/victorialogs/) — same "one binary, much cheaper" energy in a slightly different shape.
- [OpenTelemetry](https://opentelemetry.io/) — the wire protocol all the modern stacks (including OpenObserve) consume.

## Internal links

- [[prometheus]] + [[observability/grafana|grafana]] — the stack OpenObserve competes against
- [[node_exporter]] — host metrics; works against OpenObserve via the Prometheus-remote-write endpoint
- [[redpanda]] — same "Rust single-binary infra rewrite" trend, different problem space

## Keywords

`#observability` `#openobserve` `#logs` `#metrics` `#traces` `#rust` `#parquet` `#s3` `#opentelemetry` `#elasticsearch-alternative`

## References / raw notes

### Pitch (paraphrased from the project page)

> OpenObserve is a cloud-native observability platform built specifically for logs, metrics, traces, analytics, RUM (Real User Monitoring — Performance, Errors, Session Replay) designed to work at petabyte scale.

Stated headline numbers (per the project's own published benchmarks):

- **~140× lower storage cost** vs Elasticsearch on equivalent log workloads (Parquet on S3 vs row-store on local disk).
- **Single static binary** (~150 MB) vs ELK's many-component / many-JVM footprint.
- **Stateless ingester / querier processes** so you can scale horizontally without coordination overhead.

### Quickstart — single binary

```sh
# Download a release for your platform from
# https://github.com/openobserve/openobserve/releases

ZO_ROOT_USER_EMAIL="root@example.com" \
ZO_ROOT_USER_PASSWORD="Complexpass#123" \
./openobserve

# UI at http://localhost:5080 (default)
```

### Quickstart — Docker

```sh
docker run -d \
  --name openobserve \
  -p 5080:5080 \
  -v $PWD/data:/data \
  -e ZO_ROOT_USER_EMAIL="root@example.com" \
  -e ZO_ROOT_USER_PASSWORD="Complexpass#123" \
  public.ecr.aws/zinclabs/openobserve:latest
```

For S3-backed storage, set `ZO_LOCAL_MODE=false` plus the standard `ZO_S3_*` environment variables (bucket, endpoint, credentials) — all documented at <https://openobserve.ai/docs/environment-variables/>.

### Send logs from Fluent Bit (Elasticsearch-compatible bulk endpoint)

```ini
[OUTPUT]
    Name           es
    Match          *
    Host           openobserve.internal
    Port           5080
    HTTP_User      root@example.com
    HTTP_Passwd    Complexpass#123
    Index          default
    Type           _doc
    tls            Off
```

Existing Filebeat / Logstash / Vector configs targeting Elasticsearch's `_bulk` API just need the `Host` / `Port` / credentials swapped.

### Send OTLP traces / metrics

OpenObserve speaks OTLP/HTTP and OTLP/gRPC natively. Point your OpenTelemetry SDK or OTel Collector at:

- OTLP/HTTP: `https://openobserve.internal:5080/api/<org>/v1/traces` (or `/v1/metrics`, `/v1/logs`)
- OTLP/gRPC: `openobserve.internal:5081`

with `Authorization: Basic <base64(user:pass)>` (or an API token).

### Useful entry points

- Site: <https://openobserve.ai/>
- Source: <https://github.com/openobserve/openobserve>
- Docs: <https://openobserve.ai/docs/>
- Releases: <https://github.com/openobserve/openobserve/releases>
- Public benchmark vs Elasticsearch (vendor source, scepticism applies): <https://openobserve.ai/blog/openobserve-vs-elasticsearch>
