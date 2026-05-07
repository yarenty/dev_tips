---
title: "Grafana (the dashboard canvas) + Mimir for long-term metrics"
main_link: https://grafana.com/
keywords: [grafana, mimir, prometheus, dashboards, observability-overlap]
status: reviewed
review_date: 2026/05/03
---

# Grafana (the dashboard canvas) + Mimir for long-term metrics

**Main link:** <https://grafana.com/>

Mimir (the long-term Prometheus storage): <https://github.com/grafana/mimir>

> Grafana also has a separate "operator's view" article under `observability/grafana.md` covering the *running and configuring* side. This article is about Grafana **as a visualisation tool** — what you put on the dashboard, not how you keep it alive — plus the long-term metrics layer (Mimir) that makes those dashboards still answer questions a year later.

## Summary

[Grafana](https://grafana.com/) is the de-facto open-source dashboard canvas for time-series data. It connects to dozens of data sources (Prometheus, Loki, Tempo, InfluxDB, MySQL, PostgreSQL, BigQuery, Elasticsearch, ...) and lets you assemble panels — line charts, stat cells, heatmaps, tables — into shareable dashboards. [Grafana Mimir](https://grafana.com/oss/mimir/) is the companion project for *storing* Prometheus metrics at scale (object-store backed, horizontally scalable, multi-tenant) so that the dashboards above keep working past Prometheus's local-disk retention.

## Insight

Two things people miss when they first reach for Grafana:

1. **It's a viewer, not a database.** Grafana stores almost nothing of value (just dashboard JSON, users, datasource definitions). The interesting state lives in whatever you wired up as a datasource. That makes it cheap and disposable but also means you have to think separately about retention.
2. **The dashboards are JSON.** Treat them as code. Provision via [grafana-as-code](https://grafana.com/docs/grafana/latest/administration/provisioning/) or [Grafonnet](https://github.com/grafana/grafonnet-lib) / [grafanalib](https://github.com/weaveworks/grafanalib), version them in git, code-review changes. The "click around in the UI to add a panel" workflow is fine for exploration but doesn't scale past a handful of dashboards.

For storage past Prometheus's default ~15-day local retention, **Mimir** is the path of least surprise if you already speak Prometheus — it's wire-compatible, scales to ~1B active series in their published benchmarks, and uses object stores (S3 / GCS / Azure Blob / Swift) for the long-term tier. The alternatives are [Thanos](https://thanos.io/) (older, similar shape) and [VictoriaMetrics](https://victoriametrics.com/) (single-binary friendly, often quoted as more efficient).

## Similar / related topics

- [[observability/grafana|grafana]] — same product, but framed as "how to run and configure it as part of an observability stack".
- [[prometheus]] — the canonical metrics source for Grafana.
- [Loki](https://grafana.com/oss/loki/) — Grafana's logs equivalent of Mimir.
- [Tempo](https://grafana.com/oss/tempo/) — Grafana's distributed tracing storage.
- [VictoriaMetrics](https://victoriametrics.com/) / [Thanos](https://thanos.io/) — Mimir alternatives for long-term Prometheus.
- [[rerun]] — for the *robotics / multimodal* side of "look at signals over time", where Grafana is the wrong tool.

## Internal links

- [[observability/grafana|grafana]] — operator's view (under `observability/`)
- [[prometheus]] — primary metrics source
- [[plotters]] — for ad-hoc programmatic plots when a Grafana dashboard is overkill

## Keywords

`#visualization` `#grafana` `#mimir` `#prometheus` `#dashboards` `#metrics` `#timeseries`

## References / raw notes

### Local install (macOS via Homebrew)

```sh
brew install grafana
brew services start grafana
# then open http://localhost:3000  (default admin / admin)
```

Foreground / no-launchd alternative if you'd rather not install a service:

```sh
grafana-server \
  --config=/usr/local/etc/grafana/grafana.ini \
  --homepath=/usr/local/share/grafana \
  --packaging=brew \
  cfg:default.paths.logs=/usr/local/var/log/grafana \
  cfg:default.paths.data=/usr/local/var/lib/grafana \
  cfg:default.paths.plugins=/usr/local/var/lib/grafana/plugins
```

Upgrade after `brew upgrade`:

```sh
brew services restart grafana
```

### Mimir at a glance

From the project's pitch:

- **Single binary, monolithic mode** for getting started (no extra dependencies).
- **Horizontally scalable** to ~1B active time series in internal benchmarks.
- **Global view**: queries can aggregate series from multiple Prometheus instances.
- **Object-store backed**: AWS S3, GCS, Azure Blob, OpenStack Swift, anything S3-compatible.
- **HA via replication**: zero-downtime restarts, upgrades, downgrades.
- **Native multi-tenancy** with per-tenant limits and quality-of-service controls.

### Other useful entry points

- Grafana docs: <https://grafana.com/docs/grafana/latest/>
- Getting started: <https://grafana.com/docs/grafana/latest/getting-started/getting-started/>
- Mimir repo: <https://github.com/grafana/mimir>
- Provisioning dashboards as YAML/JSON: <https://grafana.com/docs/grafana/latest/administration/provisioning/>
