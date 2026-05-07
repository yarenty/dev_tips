---
title: Messaging
keywords: [messaging, kafka, redpanda, pulsar, mqtt, nats, kinesis, streaming, pubsub]
status: reviewed
review_date: 2026/05/03
---

# Messaging

Articles on message brokers, streaming platforms, and publish-subscribe systems. The lines between these blur — Kafka is "really a log", MQTT is "really a sensor protocol" — but the deciding question is almost always **what's the durability and ordering story?** That's how this section is organised.

## Decision tree: pick a messaging substrate

| What you need | Reach for |
|---------------|-----------|
| Durable log + replay over weeks, big ecosystem | [[kafka]] |
| Same as Kafka but operationally simpler (single binary, no ZK/JVM) | [[redpanda]] |
| Multi-tenant, geo-replicated, separate storage/compute | [[pulsar]] |
| Lightweight pub/sub + request/reply between microservices | [[nats]] |
| Tiny-device fan-in (sensors, IoT gateways), bandwidth-constrained | [[mqtt]] |
| Already deeply on AWS, want zero broker ops | [[kinesis]] |
| Two scripts need to talk over HTTP without writing a server | [[patchbay]] |

## Articles

- [[kafka]] — Apache Kafka: distributed log + the operations ecosystem (Strimzi, Julie, TopicCtl, Jikkou, ns4kafka, Koperator, Klaw, ...). Includes Mac/WSL/Docker install, Rust client survey, MQTT→Kafka bridge note.
- [[redpanda]] — Kafka-wire-compatible C++ single-binary platform. No JVM, no ZooKeeper, lower P99 latency. BSL-licensed.
- [[pulsar]] — Apache Pulsar: separate storage (BookKeeper) + compute (brokers); first-class multi-tenancy, geo-replication, tiered S3 storage.
- [[nats]] — Lightweight Go-based pub/sub + request/reply + JetStream persistence + KV + object store. CNCF-graduated.
- [[mqtt]] — *The* IoT messaging protocol. OASIS standard, lightweight binary header, perfect for sensors/edge.
- [[kinesis]] — AWS managed streaming family (Data Streams + Firehose + Managed Flink + Video Streams). Vendor-locked but strong AWS integration.
- [[patchbay]] — Tiny serverless HTTP pub/sub via curl. Hobby-tier; great for cross-machine notifications, file shares, webhook dev.

## Cross-section see-also

- [[messaging/mqtt|mqtt]] — see also [[drogue]] / [[balena]] / [[akri]] for the IoT side that produces MQTT traffic.
- [[paho_mqtt]] / [[mq]] — Rust client articles under `programming/rust/net/`.
- [[programming/rust/net/nats|Rust NATS client]] — Rust client article.
- [[observability/grafana|grafana]] / [[prometheus]] — for monitoring whichever broker you pick.
- [[k3s]] — natural Kubernetes substrate for self-hosting any of these (especially via [Strimzi](https://strimzi.io/) for Kafka or Helm charts for Redpanda/Pulsar).

## Keywords

`#messaging` `#kafka` `#redpanda` `#pulsar` `#mqtt` `#nats` `#kinesis` `#streaming` `#pubsub`
