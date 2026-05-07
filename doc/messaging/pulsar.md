---
title: "Apache Pulsar: cloud-native streaming with separated storage and compute"
main_link: https://pulsar.apache.org/
keywords: [pulsar, apache, streaming, messaging, bookkeeper, geo-replication, multi-tenant, cloud-native]
status: reviewed
---

# Apache Pulsar: cloud-native streaming with separated storage and compute

**Main link:** <https://pulsar.apache.org/>

Docs: <https://pulsar.apache.org/docs/> · Quickstart (Docker): <https://pulsar.apache.org/docs/next/deploy-docker>

## Summary

[Apache Pulsar](https://pulsar.apache.org/) is a multi-tenant, cloud-native distributed messaging + streaming system originally built at Yahoo, donated to Apache in 2017. The architectural distinguishing feature: **storage and serving are separate processes** — brokers handle the protocol and routing, and an underlying [Apache BookKeeper](https://bookkeeper.apache.org/) layer holds the durable log. This means you can scale brokers and storage independently, replace either tier without downtime, and add brokers without rebalancing data. On top of that Pulsar gives you geo-replication between data centres, real multi-tenancy with per-tenant quotas and isolation, sub-5 ms publish latency at scale, and a serverless functions runtime.

## Insight

Reach for Pulsar when:

- You're building a **multi-tenant platform** where teams or customers should be isolated by namespace, with quotas and ACLs that actually mean something. Pulsar's tenant/namespace/topic hierarchy was designed for this from day one; Kafka's wasn't.
- You need **geo-replication** between regions as a first-class feature, not an add-on.
- You want to **scale storage and compute independently**. Onboarding a new noisy team that fills 100 TB shouldn't require rebalancing your whole Kafka cluster.
- You want **per-message TTL** and selective retention. Pulsar handles this much more cleanly than Kafka's all-or-nothing topic retention.

Don't reach for Pulsar when:

- You want **fewer moving parts**. Pulsar = brokers + BookKeeper + ZooKeeper (or Pulsar's "Bookie-only" mode in newer versions). [[redpanda]] or [WarpStream](https://www.warpstream.com/) ship as one binary; Kafka itself in KRaft mode ships as one process per broker. Pulsar is the most operationally complex of the three.
- You're committed to the **Kafka client / connector / Streams ecosystem**. Pulsar has a Kafka-protocol shim (KoP) but it's not the primary surface; you'll be reaching for Pulsar's own client libraries.
- You're at small scale. The complexity tax is real, and Kafka or Redpanda will be simpler at <10 TB.

The most underrated Pulsar feature: **tiered storage** to S3 / GCS / Azure Blob. Topics older than X stay queryable but live on object storage at object-storage prices. This is what WarpStream now does with Kafka semantics; Pulsar has had it for years.

## Similar / related topics

- [[kafka]] — older, simpler architecture; the dominant choice; Pulsar's primary competitor.
- [[redpanda]] — single-binary Kafka-compatible alternative; opposite design philosophy (fewer moving parts vs Pulsar's more-modular approach).
- [Apache BookKeeper](https://bookkeeper.apache.org/) — the storage layer Pulsar sits on; useful to understand it independently.
- [WarpStream](https://www.warpstream.com/) — newer entrant taking Pulsar's tiered-storage idea further (S3-only, no local disks, Kafka-compatible).
- [[messaging/mqtt|mqtt]] — different niche; Pulsar has a [Pulsar IoT proxy](https://pulsar.apache.org/docs/next/io-mqtt-source/) for MQTT ingestion.

## Internal links

- [[kafka]] — primary competitor, simpler architecture
- [[redpanda]] — single-binary Kafka-compatible alternative
- [[messaging/mqtt|mqtt]] — IoT ingest that bridges in via Pulsar's connectors

## Keywords

`#messaging` `#pulsar` `#apache` `#streaming` `#bookkeeper` `#geo-replication` `#multi-tenant` `#cloud-native`

## References / raw notes

### Stated features (per the project page)

- **Cloud-native** — multiple-layer architecture separating compute from storage; designed for Kubernetes.
- **Serverless functions** — write functions with developer-friendly APIs; processed natively in the broker tier.
- **Horizontally scalable** — to hundreds of nodes.
- **Low latency with durability** — sub-5 ms publish latency at scale with strong durability guarantees.
- **Geo-replication** — configurable replication between data centres across multiple geographic regions.
- **Multi-tenancy** — first-class tenant/namespace/topic hierarchy with isolation, authn, authz, quotas.
- **Persistent storage** — Apache BookKeeper underneath; IO-level isolation between writes and reads.
- **Client libraries** — Java, Go, Python, C++, Node.js, WebSocket, C#.
- **Operability** — REST admin API; deployable on bare metal, Kubernetes, AWS, DC/OS.

### Quickest possible local install — single Docker container

```sh
docker run -it -p 6650:6650 -p 8080:8080 \
  --mount source=pulsardata,target=/pulsar/data \
  --mount source=pulsarconf,target=/pulsar/conf \
  apachepulsar/pulsar:latest \
  bin/pulsar standalone
```

This boots a single-node Pulsar in standalone mode (broker + Bookie + embedded ZooKeeper, all in one process). Service URL is `pulsar://localhost:6650`; admin API is `http://localhost:8080`.

### Sanity-check producer / consumer

```sh
# In one shell — start a consumer on persistent://public/default/my-topic
docker exec -it <container> bin/pulsar-client consume my-topic -s "test-sub" -n 0

# In another — produce 5 messages
docker exec -it <container> bin/pulsar-client produce my-topic --messages "hello-pulsar" -n 5
```

### Useful entry points

- Docs: <https://pulsar.apache.org/docs/>
- Docker quickstart: <https://pulsar.apache.org/docs/next/deploy-docker>
- Bare-metal deployment: <https://pulsar.apache.org/docs/next/deploy-bare-metal>
- Kubernetes deployment: <https://pulsar.apache.org/docs/next/deploy-kubernetes>
- StreamNative Hub (managed Pulsar + connectors): <https://hub.streamnative.io/>
- Pulsar IO connectors (Kafka source/sink, JDBC, MQTT, etc.): <https://pulsar.apache.org/docs/next/io-overview/>

### Comparison vs Kafka in one paragraph

Pulsar separates compute (brokers) from storage (BookKeeper); Kafka couples them per-broker. This means Pulsar can rebalance topics by reassigning broker ownership without moving data; Kafka has to physically copy partition data when rebalancing. The trade-off is operational surface area: Pulsar has more services to deploy, monitor, and upgrade. For most teams under ~50 TB of state, the simplicity of Kafka (or Redpanda) wins; past that, Pulsar's architectural advantages start to pay back.
