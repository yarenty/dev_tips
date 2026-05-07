---
title: "Rust net — RPC, messaging clients, tunnels"
keywords: [rust, net, grpc, mqtt, nats, tonic, rumqtt, paho, trpc, bore, tunnelling]
status: reviewed
review_date: 2026/05/03
---

# Rust net — RPC, messaging clients, tunnels

This section covers **application-layer networking primitives in Rust** — RPC frameworks (gRPC, tRPC analogues), MQTT / NATS clients, and TCP tunnels. The sister sections that often come up alongside:

- **HTTP / REST** lives in [`../web/`](../web/README.md) (axum, actix-web, reqwest).
- **Substrate-side messaging** (broker selection, ops, when MQTT vs. NATS vs. Kafka) lives in [[messaging/README]].
- **Lower-level transport / tunnelling tools** (Wireguard, ngrok, rathole) live in [[tools/networking/README|tools/networking]] and [[tools/security/README|tools/security]].

## Articles

### RPC

- [[grpc|gRPC in Rust (tonic, grpc-rs)]] — the cross-language schema-driven RPC standard; tonic is the de-facto Rust impl.
- [[trpc|tRPC and the Rust analogues (tarpc, RSPC)]] — TypeScript-first RPC framework, with the Rust-side picker (tarpc, RSPC, gRPC).

### Messaging clients

- [[mq|rumqtt — pure-Rust MQTT client and broker]] — `rumqttc` + `rumqttd`; the modern default for MQTT in Rust.
- [[paho_mqtt|paho-mqtt — Eclipse Paho Rust binding]] — FFI to the C Paho client; mature, feature-rich, heavier build.
- [[programming/rust/net/nats|async-nats — official Rust NATS client]] — pub/sub + JetStream + KV; pairs with [[messaging/nats|messaging/nats]] for the substrate side.

### Tunnels

- [[tunnelling|bore — minimal TCP tunnel in Rust]] — Eric Zhang's tiny self-hostable ngrok alternative.

## Decision shortcut

| You need… | Reach for |
|-----------|-----------|
| Cross-language RPC with codegen | gRPC + [[grpc|tonic]] |
| Rust↔Rust typed RPC, no codegen | `tarpc` ([[trpc]]) |
| Rust server + TS client typed end-to-end | RSPC ([[trpc]]) |
| MQTT, pure-Rust | [[mq|rumqttc]] |
| MQTT, Eclipse-Paho-tested | [[paho_mqtt]] |
| Pub/sub + JetStream | [[nats|async-nats]] |
| Expose local TCP via NAT | [[tunnelling|bore]] |
| HTTP / REST API | `../web/` (axum / actix) |

## See also

- [[messaging/README|Messaging]] — substrate side: brokers, when-to-use comparisons.
- [[programming/rust/web/README|Rust web]] — HTTP framework articles.
- [[programming/rust/concurrency/README|Rust concurrency]] — `tokio`, the runtime everything in this section uses.
- [[tools/networking/README|tools/networking]] — operator-side tunnelling tools (ngrok, rathole, mtr).
- [[iot/README|IoT]] — most MQTT use cases live in this section's stack.

## Keywords

`#rust` `#net` `#grpc` `#tonic` `#mqtt` `#nats` `#tunnelling`
