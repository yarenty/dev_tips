---
title: "paho-mqtt — Eclipse Paho Rust MQTT client (FFI)"
main_link: https://crates.io/crates/paho-mqtt
keywords: [rust, paho-mqtt, mqtt, eclipse-paho, ffi, c-binding, iot]
status: reviewed
---

# paho-mqtt — Eclipse Paho Rust MQTT client (FFI)

**Main link:** <https://crates.io/crates/paho-mqtt>

## Summary

[`paho-mqtt`](https://crates.io/crates/paho-mqtt) is the Rust binding to the [Eclipse Paho MQTT C client](https://www.eclipse.org/paho/index.php?page=clients/c/index.php) — a mature, battle-tested C library that has been the cross-language MQTT reference implementation for over a decade. The Rust crate is a safe wrapper around the C library and exposes the full Paho feature surface: MQTT v3.1, v3.1.1, and v5; TLS / WebSockets / proxies; QoS 0/1/2; LWT; persistence (file / memory / user-defined); automatic reconnect; offline buffering; HA broker lists; and three API flavours (async/await, traditional async with tokens, and sync/blocking).

## Insight

paho-mqtt is the **mature, FFI-shaped** Rust MQTT option. Reach for it when:

- You need a **broker-tested, spec-tested** client and the Eclipse Foundation badge matters (regulated industries, IoT certifications).
- You're matching a feature in the C client that hasn't reached `rumqttc` yet (some persistence backends, exotic transport combinations, MQTT-5 corner cases).
- You're already linking against `libpaho-mqtt3a` for other reasons.

For most new Rust code, **prefer [[mq|`rumqttc`]]** — it's pure Rust, builds cleanly with cargo, doesn't drag in a C dep, and covers ~95% of the MQTT spec that real applications use.

How it slots:

- **vs. [[mq|`rumqttc`]]** — paho-mqtt is FFI to a C library; rumqttc is pure Rust. Paho has more years of bug-finding behind it; rumqttc has a more idiomatic Rust API and zero build deps. New code → rumqttc unless you have a reason.
- **vs. cloud-vendor SDKs** (AWS IoT Device SDK, Azure IoT Hub SDK) — those usually wrap paho or a vendor variant, so you're often already on Paho indirectly.
- **vs. [[messaging/mqtt|the MQTT protocol article]]** — that's the substrate-side write-up (broker comparison, QoS levels, when MQTT vs. Kafka/NATS); this article is just about the Rust client.

Gotchas:

1. **Build dependency.** Pulling `paho-mqtt` builds `paho.mqtt.c` from source unless you set `vendored = false` and have it system-installed. CMake + a C toolchain required.
2. **Three APIs, easy to mix accidentally.** The same crate exposes blocking, callback-async, and futures-async clients. Pick one per service; mixing them within a single connection is asking for confusion.
3. **Persistence path.** File persistence writes to disk on every QoS-1/2 message. On flash-constrained edge devices, prefer memory persistence + your own durable store.
4. **C-library version pinning.** The Rust crate pins a specific Paho C version; security CVEs in `paho.mqtt.c` need a Rust-crate update to pick up — watch the crate's release cadence.

## Similar / related topics

- [[mq|`rumqttc`]] — pure-Rust MQTT client, the modern default.
- [[messaging/mqtt|messaging/mqtt]] — substrate-side article: brokers (Mosquitto, EMQX, HiveMQ), QoS levels, when-to-use vs. Kafka/NATS.
- [Eclipse Paho project](https://www.eclipse.org/paho/) — umbrella for MQTT clients in many languages; Rust is one of ~15.
- [`mqttrs`](https://github.com/00imvj00/mqttrs) — small no-std MQTT codec; useful when paho/rumqtt are too heavy for embedded.
- [HiveMQ MQTT client comparison](https://www.hivemq.com/article/mqtt-client-library-encyclopedia/) — HiveMQ-curated overview of every MQTT client library.

## Internal links

- [[programming/rust/net/README|Rust net]] — section landing page.
- [[mq|rumqtt]] — the pure-Rust sibling; usually the better default.
- [[messaging/mqtt|messaging/mqtt]] — substrate-side companion article.
- [[programming/rust/net/nats|NATS Rust client]] — sibling messaging client article.
- [[iot/README|iot]] — most paho deployments live in IoT / edge contexts.

## Keywords

`#rust` `#paho-mqtt` `#mqtt` `#eclipse-paho` `#ffi` `#iot` `#c-binding`

## References / raw notes

### Crate

<https://crates.io/crates/paho-mqtt>

> The Eclipse Paho MQTT Rust client library on memory-managed operating systems such as Linux/Posix, Mac, and Windows. The Rust crate is a safe wrapper around the Paho C Library.

### Features (from upstream)

- MQTT v5, v3.1.1, v3.1
- Network transports: standard TCP, SSL/TLS (with optional ALPN), WebSockets (secure + insecure), optional proxies
- QoS 0, 1, 2
- Last Will and Testament (LWT)
- Message persistence: file or memory, plus user-defined key/value (Redis example included)
- Automatic reconnect, offline buffering, high-availability broker lists
- Three APIs: async/await with `Future`/`Stream`, traditional token-based async, blocking sync

### See also

- Paho project home: <https://www.eclipse.org/paho/>
- Paho C source: <https://github.com/eclipse/paho.mqtt.c>
