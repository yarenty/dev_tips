---
title: "async-nats — official Rust NATS client"
main_link: https://github.com/nats-io/nats.rs
keywords: [rust, nats, async-nats, jetstream, pubsub, request-reply, kv, cloud-native]
status: reviewed
---

# async-nats — official Rust NATS client

**Main link:** <https://github.com/nats-io/nats.rs>

## Summary

[`async-nats`](https://github.com/nats-io/nats.rs) is the **official Rust client** for [NATS](https://nats.io/) — Synadia's CNCF-graduated, Go-based pub/sub messaging system. It covers the full surface: classic pub/sub, request/reply, **JetStream** (persistence + at-least-once / exactly-once-within-a-window streaming), the KV and Object stores, and leaf-node-aware connections. The legacy [`nats`](https://crates.io/crates/nats) crate (synchronous) is deprecated in favour of `async-nats`. For NATS the *substrate* (broker, ops, when to choose vs. Kafka/MQTT), see the cross-section [[messaging/nats|messaging/nats]] article.

## Insight

Reach for `async-nats` when you've already decided NATS is the right substrate (see [[messaging/nats]] for that decision) and your service is in Rust. It's a first-party client, kept in lockstep with the server, and is the one Synadia engineers fix bugs in first.

The shape of the API:

- `client.subscribe("orders.>")` — wildcard subjects work as you'd expect; `>` is multi-token, `*` is single-token.
- `client.publish("orders.created", payload).await?` — fire-and-forget (QoS-0 equivalent).
- `client.request("rpc.add", payload).await?` — built-in request/reply via temporary inbox subjects. **This is the killer feature** — Kafka can't do this, MQTT only added it in v5.
- `jetstream::Context` for streams, consumers, KV, object store. Same client, separate sub-API.

How it slots vs. neighbours:

- **vs. [[messaging/nats|the substrate-level NATS]] article** — that one covers when NATS is the right *broker* (vs. Kafka, MQTT, Redpanda). This article is just about the Rust client wrapping it.
- **vs. [[mq|`rumqttc`]]** — different protocol entirely. MQTT is best when you need IoT-spec semantics (Last Will, retained messages, QoS-1/2 wire-level guarantees). NATS is best for low-latency request/reply between cloud services.
- **vs. [`tonic`](grpc.md)** — gRPC is RPC-first, schema-first, HTTP/2 transport. NATS request/reply is opinionated-less: any payload, any subject. Pick gRPC when you want strong typing across teams; pick NATS-RR when you want loose coupling and broadcast-shaped routing.
- **vs. Kafka clients** — Kafka is a log; NATS JetStream is closer to a queue with replay. Different mental model. NATS clusters in milliseconds; Kafka in minutes.

Gotchas:

1. **JetStream isn't enabled by default** on the server — the broker must be started with `-js` or have it in the config.
2. **Subject namespacing matters.** Subjects are global to the account; pick a hierarchy (`team.service.entity.event`) before you have 100 of them.
3. **Reconnection is automatic but silent.** The client will reconnect to a server in the cluster on disconnect; you may not get a hard error when you'd expect one. Use `client.events()` to observe connection state.
4. **Don't `block_on` from inside async.** `async-nats` is fully async; calling `.await` from a sync wrapper that uses `block_on` inside a tokio runtime will deadlock the runtime worker.

## Similar / related topics

- [[messaging/nats|messaging/nats]] — the substrate-side article: spec, JetStream, leaf nodes, when-to-use vs. Kafka/MQTT.
- [[mq|`rumqttc`]] — the MQTT sibling for IoT-spec workloads.
- [`tonic`](grpc.md) — when you'd rather have schema-first RPC than subject-based pub/sub.
- [Synadia's NATS server source](https://github.com/nats-io/nats-server) — the Go broker implementation; useful when debugging connection / cluster behaviour.
- [`async-nats` JetStream docs](https://docs.rs/async-nats/latest/async_nats/jetstream/) — start here for streams / consumers / KV.

## Internal links

<!-- reviewed -->

- [[programming/rust/net/README|Rust net]] — section landing page.
- [[messaging/nats|messaging/nats]] — substrate-side companion article (broker, ops, when-to-use).
- [[mq|rumqtt]] — sibling Rust client article for MQTT.
- [[paho_mqtt]] — Eclipse Paho FFI MQTT client.

## Keywords

`#rust` `#nats` `#async-nats` `#jetstream` `#pubsub` `#request-reply` `#cloud-native`

## References / raw notes

### Crate

- Repo (now hosting `async-nats`): <https://github.com/nats-io/nats.rs>
- Docs: <https://docs.rs/async-nats/>
- Legacy synchronous crate: <https://crates.io/crates/nats> (deprecated)

### NATS 2.2 (history) — JetStream and friends

> NATS 2.2 is the largest feature release since version 2.0. The 2.2 release provides highly scalable, highly performant, secure and easy-to-use next-generation streaming in the form of JetStream, allows remote access via WebSockets, has simplified NATS account management, native MQTT support, and further enables NATS toward our goal of securely democratizing streams and services for the hyperconnected world we live in.

JetStream highlights:

- Easy to deploy and manage; built into the NATS server (no separate process).
- Simplifies and accelerates development.
- Wildcard-subject aware.
- At-least-once delivery, with exactly-once within a window.
- Horizontally scalable at runtime, no interruptions.
- Persists data via streams; delivers/replays via consumers.
- Multiple consumption patterns on the same stream (push and pull).
- Account-aware, with per-stream / per-consumer / per-function security granularity.
