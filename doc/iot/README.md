---
title: IoT
keywords: [iot, drogue, mqtt, shodan, embedded, edge, security]
status: reviewed
review_date: 2026/05/03
---

# IoT

A small, opinionated section on Internet-of-Things tooling. The articles cover three different angles:

- **Building** — [[drogue]]: a Rust device-to-cloud framework (now archived) whose patterns are still worth knowing, even if you'll likely pick something else for *new* projects.
- **Transport** — pointer to [[messaging/mqtt|mqtt]] under `messaging/` (the canonical MQTT article lives there since the protocol isn't IoT-only).
- **Security / discovery** — [[shodan]]: search engine for everyone's accidentally-exposed devices, including yours.

For the wider IoT topic surface that doesn't fit here, see also:

- [[balena]] (`kubernetes/`) — fleet OS for embedded devices.
- [[k3s]] (`kubernetes/`) — lightweight Kubernetes for edge nodes.
- [[akri]] (`kubernetes/`) — exposes IoT leaf devices (cameras, USB, OPC UA) to Kubernetes pods.
- [[groot]] (`robot/`) — humanoid-robot foundation model; IoT-adjacent embodied-AI angle.

## Articles

- [[drogue]] — Drogue IoT: Rust device-to-cloud framework. Now retired (Red Hat archived it in 2023). Patterns + alternatives.
- [[shodan]] — search engine for internet-exposed devices. Defensive-recon framing, CLI cheat sheet, what to look for in your own perimeter.

## Cross-section see-also

- [[messaging/mqtt|mqtt]] — *the* IoT messaging protocol; lives under `messaging/` because it's not strictly IoT-only.
- [[balena]] — IoT fleet OS (under `kubernetes/`).
- [[k3s]] — lightweight Kubernetes for edge nodes (under `kubernetes/`).
- [[akri]] — Kubernetes device-discovery for IoT leaf devices (under `kubernetes/`).
- [[oracle_free_tier]] — natural target for a self-hosted IoT back-end (4 OCPU + 24 GB ARM, free).

## Keywords

`#iot` `#drogue` `#mqtt` `#shodan` `#embedded` `#edge` `#security`
