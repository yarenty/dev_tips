---
title: "Prometheus: pull-based metrics + PromQL"
main_link: https://prometheus.io/
keywords: [prometheus, observability, metrics, promql, time-series, monitoring]
status: reviewed
review_date: 2026/05/03
---

# Prometheus: pull-based metrics + PromQL

**Main link:** <https://prometheus.io/>

Repo: <https://github.com/prometheus/prometheus>

## Summary

[Prometheus](https://prometheus.io/) is the de-facto open-source monitoring system: a single Go binary that scrapes HTTP endpoints exposing metrics in its plain-text exposition format, stores them in a local time-series database, and exposes a query language ([PromQL](https://prometheus.io/docs/prometheus/latest/querying/basics/)) for slicing them. It pairs almost universally with [[observability/grafana|grafana]] for dashboards and with [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) for routing alerts to Slack/PagerDuty/etc. The ecosystem of [exporters](https://prometheus.io/docs/instrumenting/exporters/) (`node_exporter`, `cadvisor`, `postgres_exporter`, ...) means almost every system you'd want to monitor already has a way to emit Prometheus-shaped metrics.

## Insight

Three properties define how Prometheus thinks, and you'll fight it forever if you don't internalise them:

1. **Pull, not push.** Prometheus *scrapes* its targets on a schedule (default 15 s). This is the right model for long-running services (you can drop a target's IP into config and forget) but the wrong model for batch jobs and ephemeral scripts — for those you want the [Pushgateway](https://github.com/prometheus/pushgateway) and you should *only* want it for those.
2. **Local disk, ~15-day retention by default.** A single Prometheus is not your long-term metrics store. If you need a year of history, put [Mimir](https://grafana.com/oss/mimir/), [Thanos](https://thanos.io/), or [VictoriaMetrics](https://victoriametrics.com/) downstream.
3. **High cardinality kills you.** Each unique label combination is a separate time-series. A `path` label that includes user IDs, or a `pod_name` label in a system that creates thousands of pods per day, will blow up your active series count and make queries slow. Use [recording rules](https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/) to pre-aggregate; use [relabeling](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#relabel_config) to drop labels at scrape time.

PromQL has four metric types — counter (only-goes-up; use `rate()` to read it), gauge (any value; read directly), histogram (`_bucket`, `_sum`, `_count` triplet; use `histogram_quantile()`), summary (pre-computed quantiles client-side; usually a worse choice than histogram). The `rate()` / `irate()` / `increase()` / `histogram_quantile()` functions are 90% of what you'll write.

## Similar / related topics

- [[observability/grafana|grafana]] — what you put in front of Prometheus to draw the dashboards.
- [VictoriaMetrics](https://victoriametrics.com/) — wire-compatible drop-in replacement, often quoted as more efficient at scale.
- [Thanos](https://thanos.io/) / [Mimir](https://grafana.com/oss/mimir/) — long-term storage layers that sit *behind* (or alongside) Prometheus.
- [OpenTelemetry](https://opentelemetry.io/) — newer, vendor-neutral spec; Prometheus can scrape OTLP metrics via the OTLP receiver, and OTel collectors can push *to* Prometheus-compatible endpoints.
- [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) — the routing/grouping/silencing piece that consumes Prometheus alerts.

## Internal links

- [[observability/grafana|grafana]] — paired dashboard frontend
- [[visualization/grafana|grafana]] — Mimir long-term-storage discussion

## Keywords

`#observability` `#prometheus` `#metrics` `#promql` `#time-series` `#monitoring` `#alertmanager`

## References / raw notes

### Install (macOS, Homebrew)

```sh
brew install prometheus
brew services start prometheus           # listens on http://localhost:9090
```

The brew formula uses CLI flags from `/usr/local/etc/prometheus.args`. Edit that file to change the config path or retention.

Foreground / no-launchd:

```sh
/usr/local/opt/prometheus/bin/prometheus_brew_services
```

### Minimal `prometheus.yml`

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']     # node_exporter on this host
```

### PromQL cheat sheet

```promql
# Per-second rate of an HTTP-request counter, averaged over 5m
rate(http_requests_total[5m])

# Same, sliced by status code
sum by (status) (rate(http_requests_total[5m]))

# 95th-percentile latency from a histogram
histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))

# Pods using more than 1 GiB of memory right now
container_memory_working_set_bytes{pod!=""} > 1024 * 1024 * 1024
```

### Recording rule (pre-aggregate to keep dashboards fast)

```yaml
groups:
  - name: example
    interval: 30s
    rules:
      - record: job:http_requests:rate5m
        expr: sum by (job) (rate(http_requests_total[5m]))
```

### Useful entry points

- Docs: <https://prometheus.io/docs/prometheus/latest/>
- PromQL basics: <https://prometheus.io/docs/prometheus/latest/querying/basics/>
- Exporters list: <https://prometheus.io/docs/instrumenting/exporters/>
- Source: <https://github.com/prometheus/prometheus>
- Awesome list: <https://github.com/roaldnefs/awesome-prometheus>
