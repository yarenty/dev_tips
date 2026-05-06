---
title: Kubernetes (and adjacent edge / container topics)
keywords: [kubernetes, k3s, dokku, balena, akri, kuasar, edge, iot, container-runtime]
status: reviewed
---

# Kubernetes (and adjacent edge / container topics)

This section is deliberately mixed: the main Kubernetes-shaped article is [[k3s]] (lightweight Kubernetes for edge / IoT / homelab), but two of the articles here aren't strictly Kubernetes — they're alternatives or adjacent layers in the same problem space ("how do I run my code on a fleet of small Linux boxes"):

- [[balena]] — container-based fleet OS; not Kubernetes, parallel ecosystem.
- [[dokku]] — single-host self-hosted Heroku; not Kubernetes either, much smaller scope.

The remaining two are genuinely Kubernetes-flavoured:

- [[akri]] — exposes IoT / leaf devices (cameras, USB, OPC UA) as Kubernetes resources.
- [[kuasar]] — Rust container runtime built on the containerd Sandbox API; matters for serverless / multi-tenant infra.

## Articles

- [[k3s]] — lightweight Kubernetes (~40 MB binary, ~500 MB RAM); the canonical pick for ARM, edge, homelab. Covers install + Raspberry Pi cluster recipe + arkade + OpenFaaS + GitOps + learning resources.
- [[akri]] — CNCF Sandbox project that turns IoT leaf devices into schedulable Kubernetes resources.
- [[kuasar]] — Rust container runtime; multi-sandbox (runC + MicroVM + WasmEdge) on the containerd Sandbox API.
- [[balena]] — container-based fleet OS for IoT (balenaOS + balenaEngine + balenaCloud). Not Kubernetes.
- [[dokku]] — self-hosted single-box mini-Heroku via `git push`. Not Kubernetes.

## Decision tree: "I have a fleet of small Linux boxes, what should I run on them?"

| Constraint | Reach for |
|------------|-----------|
| Already invested in Kubernetes ecosystem | [[k3s]] |
| Need atomic OTA updates for a commercial IoT product | [[balena]] |
| Just want `git push` to deploy a hobby app to one VPS | [[dokku]] |
| Need to expose USB / IP-camera / OPC UA devices to pods | [[k3s]] + [[akri]] |
| Building a serverless platform, care about cold-start | [[kuasar]] (still niche) |
| Even smaller than K3s (single-host, no Kubernetes at all) | [faasd](https://github.com/openfaas/faasd) (mentioned in [[k3s]]) |

## Cross-section see-also

- [[oracle_free_tier]] — natural target host (4 OCPU + 24 GB ARM, free) for K3s, Dokku, or balenaOS.
- [[apache]] / [[ssl]] — relevant for ingress + TLS termination on top of any of the above.
- [[observability/grafana|grafana]] / [[prometheus]] — the canonical observability stack for K3s clusters.
- [[messaging/mqtt|mqtt]] — typical telemetry transport from devices Akri exposes or Balena fleets emit.
- [[groot]] — example of an embodied-AI workload that benefits from K3s + Akri + GPU device-plugin.

## Keywords

`#kubernetes` `#k3s` `#dokku` `#balena` `#akri` `#kuasar` `#edge` `#iot` `#container-runtime`
