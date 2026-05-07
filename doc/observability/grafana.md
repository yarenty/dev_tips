---
title: "Grafana (operator's view): install, run, configure"
main_link: https://grafana.com/docs/grafana/latest/
keywords: [grafana, observability, dashboards, install, brew, docker, datasources]
status: reviewed
---

# Grafana (operator's view): install, run, configure

**Main link:** <https://grafana.com/docs/grafana/latest/>

> A separate, more visualisation-focused article lives at `visualization/grafana.md` and covers Grafana **as a dashboard canvas** + Mimir for long-term metrics. This article is the *operator's view*: how to install it, run it, point it at datasources, and not lose your dashboards when the box dies.

## Summary

[Grafana](https://grafana.com/) is the open-source dashboards-and-alerts frontend that sits at the top of almost every modern observability stack. As an operator, the things you actually need to know are: (1) it's a stateless web app whose only durable state is `grafana.db` (SQLite by default, swap to Postgres or MySQL for HA), (2) datasources / dashboards / users / API tokens can all be **provisioned from YAML on disk** so the running instance is reproducible, and (3) it speaks to anything time-series-shaped via plugins ([Prometheus](https://prometheus.io/), [Loki](https://grafana.com/oss/loki/), [Tempo](https://grafana.com/oss/tempo/), InfluxDB, Elasticsearch, [PostgreSQL](https://www.postgresql.org/), CloudWatch, ...).

## Insight

Three things to do early to avoid pain later:

1. **Move off SQLite as soon as you have more than one node.** The default `grafana.db` is fine for a laptop, but it locks the whole instance and forbids HA. Switch the `[database]` section in `grafana.ini` to point at Postgres or MySQL before you put anything important in there.
2. **Provision dashboards as code.** Drop YAML files into `/etc/grafana/provisioning/{datasources,dashboards}/` and let Grafana load them at boot. This means dashboard changes go through PRs, the same as code, and a fresh Grafana boot ends up identical to the last one. Tools to author the JSON: [Grafonnet](https://github.com/grafana/grafonnet-lib) (Jsonnet), [grafanalib](https://github.com/weaveworks/grafanalib) (Python), or just hand-edit the JSON the UI exports.
3. **Don't store dashboards only in the UI.** "Save dashboard" writes to `grafana.db`. If that dies, so do all your panels. Either provision from disk (best) or run a nightly `grafana-backup-tool` cron (works but messy).

The default admin password (`admin/admin`) is a foot-gun if Grafana is exposed past localhost. Set `GF_SECURITY_ADMIN_PASSWORD` or `[security] admin_password` before starting, never after.

## Similar / related topics

- [[visualization/grafana|grafana]] — same product, but framed as the dashboard-design / Mimir long-term-storage view.
- [[prometheus]] — the canonical metrics datasource.
- [Loki](https://grafana.com/oss/loki/) — Grafana's logs aggregation backend.
- [Tempo](https://grafana.com/oss/tempo/) — Grafana's distributed-tracing backend.
- [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) — what Grafana's alert rules typically route to (or use Grafana's own unified alerting introduced in v8).

## Internal links

- [[prometheus]] — primary metrics datasource
- [[visualization/grafana|grafana]] — dashboard-design framing of the same product

## Keywords

`#observability` `#grafana` `#dashboards` `#install` `#brew` `#docker` `#provisioning` `#datasources`

## References / raw notes

### Install (macOS, Homebrew)

```sh
brew install grafana
brew services start grafana            # listens on http://localhost:3000
# default credentials: admin / admin  (you WILL be prompted to change on first login)
```

Foreground / no-launchd run:

```sh
/usr/local/opt/grafana/bin/grafana server \
  --config   /usr/local/etc/grafana/grafana.ini \
  --homepath /usr/local/opt/grafana/share/grafana \
  --packaging=brew \
  cfg:default.paths.logs=/usr/local/var/log/grafana \
  cfg:default.paths.data=/usr/local/var/lib/grafana \
  cfg:default.paths.plugins=/usr/local/var/lib/grafana/plugins
```

Restart after `brew upgrade grafana`:

```sh
brew services restart grafana
```

### Install (Docker, single container)

```sh
docker run -d \
  --name=grafana \
  -p 3000:3000 \
  -v grafana-storage:/var/lib/grafana \
  -e "GF_SECURITY_ADMIN_PASSWORD=change-me" \
  grafana/grafana-oss:latest
```

The `grafana-storage` named volume holds `grafana.db` — back this up if you don't move to an external Postgres/MySQL.

### Provisioning datasources from YAML

`/etc/grafana/provisioning/datasources/prometheus.yaml`:

```yaml
apiVersion: 1
datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:9090
    isDefault: true
    jsonData:
      timeInterval: 15s
```

### Provisioning dashboards

Two files: a *provider* (`/etc/grafana/provisioning/dashboards/default.yaml`) and the dashboard JSON it points at (`/var/lib/grafana/dashboards/<my>.json`):

```yaml
# default.yaml
apiVersion: 1
providers:
  - name: 'default'
    orgId: 1
    folder: ''
    type: file
    disableDeletion: false
    updateIntervalSeconds: 30
    options:
      path: /var/lib/grafana/dashboards
```

JSON dashboards in `/var/lib/grafana/dashboards/` will then be auto-loaded at boot and refreshed every 30 s. You can grab JSON via the **Share → Export → "Save to file"** menu in any dashboard.

### External backing database (HA)

In `grafana.ini`:

```ini
[database]
type = postgres
host = db.internal:5432
name = grafana
user = grafana
password = ${GRAFANA_DB_PASSWORD}
```

This unlocks: multiple Grafana replicas behind a load balancer, zero-data-loss restarts, and the ability to back up your Grafana state with the same machinery you already use for Postgres.

### Useful entry points

- Docs: <https://grafana.com/docs/grafana/latest/>
- Provisioning reference: <https://grafana.com/docs/grafana/latest/administration/provisioning/>
- Plugin catalogue: <https://grafana.com/grafana/plugins/>
- Source: <https://github.com/grafana/grafana>
