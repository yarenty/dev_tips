---
title: "rumqtt — pure-Rust MQTT client and broker"
main_link: https://github.com/bytebeamio/rumqtt
keywords: [rust, rumqtt, mqtt, rumqttc, rumqttd, broker, client, iot, bytebeam]
status: reviewed
---

# rumqtt — pure-Rust MQTT client and broker

**Main link:** <https://github.com/bytebeamio/rumqtt>

## Summary

[rumqtt](https://github.com/bytebeamio/rumqtt) is a small, **pure-Rust** MQTT v3.1.1 / v5 implementation by [Bytebeam](https://bytebeam.io/). It ships two crates: **`rumqttc`** (a high-level async client built on tokio) and **`rumqttd`** (an embeddable broker — single binary or library, suitable for edge / on-device deployments). It's the pick when you want MQTT in Rust without dragging in a C library.

## Insight

The Rust MQTT picker is short:

- **`rumqttc`** — pure Rust, async, idiomatic. The default for new code unless you have a hard reason to pick something else.
- **[`paho-mqtt`](paho_mqtt.md)** — Eclipse Paho's Rust binding to the C library. More mature feature parity with the broader Eclipse ecosystem (e.g. `Persistence` plugins, exotic transports), but you pay with FFI and a build dep.
- **MQTT-only async runtimes for embedded** ([`embedded-mqtt`](https://github.com/factbird/embedded-mqtt), `mqttrs`) — for `no_std` / microcontroller use; out of scope for most server-side code.

For the **broker** side, `rumqttd` is unusual in being embeddable: you can run it as a library inside another Rust program (handy for edge gateways that want a local broker that bridges up to a cloud broker), or as a standalone binary. The mainstream brokers — Mosquitto / EMQX / HiveMQ / VerneMQ / NanoMQ — are the right pick for general-purpose deployments; `rumqttd` shines specifically when you want to **bundle the broker inside your app** or you need a single tiny static binary for an edge box.

Use cases where rumqttc lands well:

- Telemetry from many small Rust services up to a cloud MQTT broker (HiveMQ Cloud, EMQX Cloud, AWS IoT Core).
- Industrial / IoT edge gateways that aggregate sensor data over MQTT and forward over Kafka or HTTP.
- Embedded-Linux devices where adding a JVM or libpaho is overkill.

Gotchas:

1. **Last-Will + retained messages** are foot-guns of the spec, not the library. Re-read the OASIS spec before relying on QoS-2 / retained semantics.
2. **TLS feature flags.** `rumqttc` defaults to no TLS; enable `use-rustls` (the rustls path is the modern default) or `use-native-tls`.
3. **Manual event-loop drive.** The async client returns an `EventLoop` you must `.poll()` in a task — easy to forget; if your client never receives messages, check that the loop is being driven.
4. **`rumqttd` is not a Mosquitto replacement.** Less mature ACL / bridging / persistence story than the established brokers. Pin a known-good version for production.

## Similar / related topics

- [[paho_mqtt]] — Eclipse Paho Rust binding (FFI to C); pick when you need feature parity with the broader Paho ecosystem.
- [[messaging/mqtt|MQTT (the protocol)]] — the substrate-side article: spec, QoS levels, brokers, when to use vs. Kafka/NATS.
- [Mosquitto](https://mosquitto.org/) — the open-source reference broker; almost certainly what you'll point `rumqttc` at first.
- [EMQX](https://www.emqx.io/) / [HiveMQ](https://www.hivemq.com/) / [NanoMQ](https://nanomq.io/) — production-grade brokers with clustering, bridges, web dashboards.
- [`async-nats`](https://docs.rs/async-nats/) — reach for this when you want a broker-y pub/sub but MQTT semantics aren't a hard requirement; see [[nats|the sibling NATS Rust client article]].

## Internal links

- [[programming/rust/net/README|Rust net]] — section landing page.
- [[paho_mqtt]] — sibling article for the Paho FFI binding.
- [[programming/rust/net/nats|NATS Rust client]] — sibling article for the next-most-common pub/sub.
- [[messaging/mqtt|messaging/mqtt]] — substrate-side MQTT landing page (broker comparison, QoS, when-to-use).
- [[iot/README|iot]] — most rumqtt deployments are on the IoT / edge side.

## Keywords

`#rust` `#rumqtt` `#mqtt` `#rumqttc` `#rumqttd` `#broker` `#iot` `#async`

## References / raw notes

### rumqtt

<https://github.com/bytebeamio/rumqtt>

> rumqtt is an opensource set of libraries written in Rust to implement the MQTT standard while striving to be simple, robust and performant.

| Crate | Description |
|-------|-------------|
| `rumqttc` | High-level, easy-to-use async MQTT client |
| `rumqttd` | High-performance, embeddable MQTT broker |

### Native — no C dependency

The headline differentiator vs. `paho-mqtt`. Pure Rust, builds straight from `cargo`, single static binary at the end.
