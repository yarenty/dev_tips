---
title: Cloud
keywords: [cloud, oci, free-tier, hobby-vps, hetzner, fly-io]
status: reviewed
---

# Cloud

This section is small on purpose — it covers the *one* cloud-provider thing I keep coming back to (Oracle's Always Free tier, because nobody else gives you 4 ARM cores and 24 GB of RAM forever) rather than trying to be a general "how to use AWS / GCP / Azure" guide.

For most cloud topics the relevant articles live in adjacent sections:

- **Containers / orchestration** → [Kubernetes](../kubernetes/README.md), in particular [[k3s]] for hobby-scale clusters that fit on a single Always-Free Ampere instance.
- **Web-server config / TLS** → [WWW](../www/README.md), in particular [[apache]] and [[ssl]].
- **Observability** → [Observability](../observability/README.md), in particular [[prometheus]] + [[observability/grafana|grafana]].
- **Messaging / event buses** → [Messaging](../messaging/README.md).
- **DPUs & accelerated networking** → [DPU](../dpu/README.md).

## Articles

- [[oracle_free_tier]] — Oracle Cloud Always Free Tier, especially the 4-OCPU / 24-GB Ampere A1 ARM instance: what's in the quota, how to open inbound ports on the VCN (the most-missed step), and a condensed Apache + PHP install recipe.

## When to reach for which free / cheap option (rough decision tree)

| Want | Use |
|------|-----|
| Long-running ARM VPS, free forever | **OCI Always Free** — see [[oracle_free_tier]] |
| Cheapest *paid* KVM instance, ~€4/mo, no capacity games | [Hetzner Cloud](https://www.hetzner.com/cloud) |
| Auto-scaling tiny apps, generous credits | [Fly.io](https://fly.io/) |
| Static site + edge functions | [Cloudflare Pages](https://pages.cloudflare.com/) / [Workers](https://workers.cloudflare.com/) |
| 12 months of paid-tier compute (then it expires) | [AWS Free Tier](https://aws.amazon.com/free/) / [GCP Free Tier](https://cloud.google.com/free) |

## Keywords

`#cloud` `#oci` `#oracle-cloud` `#free-tier` `#hobby-vps` `#hetzner` `#fly-io`
