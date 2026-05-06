---
title: "node_exporter: host-level Prometheus metrics for *nix machines"
main_link: https://github.com/prometheus/node_exporter
keywords: [node-exporter, prometheus, observability, host-metrics, cpu, memory, disk, network, hardware]
status: reviewed
---

# node_exporter: host-level Prometheus metrics for *nix machines

**Main link:** <https://github.com/prometheus/node_exporter>

Docs: <https://prometheus.io/docs/guides/node-exporter/> · Releases: <https://github.com/prometheus/node_exporter/releases>

## Summary

[`node_exporter`](https://github.com/prometheus/node_exporter) is the canonical Prometheus exporter for **host-level metrics** on *nix machines (Linux, macOS, *BSD, ...). One static Go binary, no dependencies, listens on `:9100/metrics`, exposes ~50 collector groups covering CPU (`node_cpu_seconds_total`), memory (`node_memory_*`), disk I/O (`node_disk_*`), filesystems (`node_filesystem_*`), network (`node_network_*`), load average, hardware sensors, NTP drift, systemd unit states, and more. It's what every Prometheus install scrapes first.

For Windows hosts, the equivalent is [`windows_exporter`](https://github.com/prometheus-community/windows_exporter) (different repo, same idea).

## Insight

If you're running [[prometheus]], you almost certainly want `node_exporter` on every Linux box you care about. It's the no-brainer first scrape target — you get the [Node Exporter Full](https://grafana.com/grafana/dashboards/1860-node-exporter-full/) Grafana dashboard (one of the all-time most-installed dashboards) and a baseline of "is this machine healthy?" alerts for free.

Three operational notes worth knowing:

1. **Pick your collectors deliberately.** The default set is sensible but heavy; some collectors (`hwmon`, `nfs`, `mdadm`, `arp`) only matter on specific hardware. Disable noisy ones with `--no-collector.<name>` flags to reduce scrape time and cardinality.
2. **`textfile` collector is the integration escape hatch.** Drop a file into the configured `--collector.textfile.directory` and `node_exporter` exposes its contents as metrics. This is how cron jobs, backup scripts, and bespoke shell tools emit metrics without becoming HTTP servers themselves.
3. **Cardinality on `device` and `mountpoint`.** On boxes with thousands of mounts (Docker / Kubernetes nodes), `node_filesystem_*` series multiply fast. Use `--collector.filesystem.mount-points-exclude='^/(dev|proc|sys|var/lib/docker|var/lib/containers)($|/)'` to drop the noise.

For containers you usually want **[cAdvisor](https://github.com/google/cadvisor)** alongside `node_exporter` — `node_exporter` reports the host, `cAdvisor` reports per-container metrics from `/sys/fs/cgroup`. They're complementary, not redundant.

## Similar / related topics

- [[prometheus]] — the metrics server `node_exporter` feeds; pretty much non-negotiable here.
- [[observability/grafana|grafana]] — what you point at Prometheus to render the data; the [Node Exporter Full dashboard](https://grafana.com/grafana/dashboards/1860-node-exporter-full/) is the canonical starting point.
- [`cadvisor`](https://github.com/google/cadvisor) — container-level metrics; complementary, not a replacement.
- [`windows_exporter`](https://github.com/prometheus-community/windows_exporter) — Windows equivalent.
- [`process-exporter`](https://github.com/ncabatoff/process-exporter) — when you need per-process metrics beyond what node_exporter exposes.
- [Prometheus exporter list](https://prometheus.io/docs/instrumenting/exporters/) — for everything else (postgres_exporter, redis_exporter, mysqld_exporter, ...).

## Internal links

<!-- reviewed -->

- [[prometheus]] — the server that scrapes node_exporter
- [[observability/grafana|grafana]] — Grafana dashboards on top
- [[openobserve]] — alternative observability stack that also ingests node_exporter metrics

## Keywords

`#observability` `#node-exporter` `#prometheus` `#host-metrics` `#cpu` `#memory` `#disk` `#network` `#textfile-collector`

## References / raw notes

### Install (macOS / Homebrew)

```sh
brew install node_exporter
brew services start node_exporter      # listens on http://localhost:9100/metrics
```

The brew formula reads CLI flags from `/usr/local/etc/node_exporter.args`. Edit that to enable/disable collectors.

Foreground / no-launchd:

```sh
/usr/local/opt/node_exporter/bin/node_exporter_brew_services
```

### Install (Linux — direct binary)

```sh
NODE_EXPORTER_VERSION=1.8.2
ARCH=linux-amd64

cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v$NODE_EXPORTER_VERSION/node_exporter-$NODE_EXPORTER_VERSION.$ARCH.tar.gz
tar xf node_exporter-$NODE_EXPORTER_VERSION.$ARCH.tar.gz
sudo mv node_exporter-$NODE_EXPORTER_VERSION.$ARCH/node_exporter /usr/local/bin/
```

Minimal systemd unit (`/etc/systemd/system/node_exporter.service`):

```ini
[Unit]
Description=Prometheus node_exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter \
  --collector.systemd \
  --no-collector.hwmon \
  --collector.textfile.directory=/var/lib/node_exporter/textfile_collector
Restart=always

[Install]
WantedBy=multi-user.target
```

### Wire it into Prometheus

In `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets:
          - 'host1:9100'
          - 'host2:9100'
          - 'host3:9100'
        labels:
          env: prod
```

For dynamic targets, use `file_sd_configs`, `consul_sd_configs`, `ec2_sd_configs`, or [Kubernetes pod-discovery](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#kubernetes_sd_config) instead.

### `textfile` collector — emit metrics from any cron job

```sh
# /etc/cron.daily/backup-metrics
#!/usr/bin/env bash
set -euo pipefail
TEMP=$(mktemp)
{
  echo "# HELP last_backup_unixtime Unix timestamp of last successful backup"
  echo "# TYPE last_backup_unixtime gauge"
  echo "last_backup_unixtime $(date +%s)"
  echo "# HELP backup_size_bytes Size of the latest backup in bytes"
  echo "# TYPE backup_size_bytes gauge"
  echo "backup_size_bytes $(stat -c%s /backups/latest.tar.gz)"
} > "$TEMP"
mv "$TEMP" /var/lib/node_exporter/textfile_collector/backup.prom
```

Atomic move (`mv` is atomic within the same filesystem) avoids `node_exporter` reading a half-written file.

### Useful entry points

- Repo: <https://github.com/prometheus/node_exporter>
- Collector reference: <https://github.com/prometheus/node_exporter#collectors>
- Grafana dashboard "Node Exporter Full" (1860): <https://grafana.com/grafana/dashboards/1860-node-exporter-full/>
- Prometheus's own getting-started guide: <https://prometheus.io/docs/guides/node-exporter/>
