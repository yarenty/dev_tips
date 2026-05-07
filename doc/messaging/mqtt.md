---
title: "MQTT: the lightweight pub/sub protocol of IoT"
main_link: https://mqtt.org/
keywords: [mqtt, iot, messaging, pubsub, mosquitto, hivemq, emqx, paho, oasis-standard]
status: reviewed
---

# MQTT: the lightweight pub/sub protocol of IoT

**Main link:** <https://mqtt.org/>

OASIS spec (v5.0): <https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html> · MQTT 3.1.1: <https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html>

## Summary

MQTT (Message Queuing Telemetry Transport) is an OASIS-standard pub/sub messaging protocol designed by IBM in 1999 for telemetry from oil-pipeline sensors over satellite links. The shape stuck: a tiny binary header (2-byte minimum), publish/subscribe over TCP (or WebSockets, or QUIC in v5), three QoS levels, retained messages, last-will/testament, and a topic-tree namespace (`sensors/floor1/temperature`). Today it's the de-facto IoT transport — almost every smart-home hub, industrial sensor gateway, and connected-car platform speaks it — and it's also turned up in unexpected places like Facebook Messenger's chat transport.

## Insight

Reach for MQTT when:

- You have **many small clients pushing telemetry** to a small number of consumers. Sensor → broker → analytics is the canonical shape.
- Bandwidth and battery are tight. The 2-byte minimum header genuinely matters on a coin-cell-powered LoRa sensor or over cellular.
- You want **fan-out / fan-in** without writing your own routing — topic wildcards (`sensors/+/temperature`, `sensors/#`) handle it for free.

Don't reach for MQTT when:

- You need **request/reply** semantics. MQTT v5 added "response topics" but it's still awkward; use HTTP, gRPC, or NATS-request-reply.
- You need **strong ordering across topics** or **persistent log replay** weeks later. That's [Kafka](https://kafka.apache.org/) territory; MQTT is publish-and-forget.
- You need **per-message authorization beyond ACLs**. MQTT brokers do topic-level ACLs; anything more granular needs an in-broker plugin or a different bus.

The four things that bite people coming from HTTP-shaped systems:

1. **QoS is per-publish, per-direction.** QoS 0 = fire-and-forget, QoS 1 = at-least-once (you may see duplicates), QoS 2 = exactly-once (slow, two-handshake). Most IoT uses QoS 0 or 1; QoS 2 is rare in production.
2. **Retained messages are a footgun.** Set `retain: true` and the broker keeps the *last* message on that topic forever and replays it to every new subscriber. Useful for "current sensor state" topics. Catastrophic when accidentally retaining a 5 MB binary payload.
3. **Topic design matters and is hard to change later.** Topics aren't versioned; once `sensors/+/temp` is in production firmware, changing it is a coordinated fleet-update.
4. **No in-broker schema.** MQTT is just bytes. Pair with [MessagePack](https://msgpack.org/), [CBOR](https://cbor.io/), [Protobuf](https://protobuf.dev/), or JSON on top, and put a schema-registry in front of the broker if you have multiple producer teams.

### Brokers: pick one

| Broker | Style | When to pick |
|--------|-------|-------------|
| **[Eclipse Mosquitto](https://mosquitto.org/)** | Single-binary C, dead simple | Hobby projects, single-node deployments, embedded gateways. The default. |
| **[EMQX](https://www.emqx.io/)** | Erlang, clusterable, OSS + commercial | Production fleets >10k clients; the "I need clustering" answer. |
| **[HiveMQ](https://www.hivemq.com/)** | Java, commercial-with-CE-edition | Enterprise (esp. automotive), strong MQTT 5 support, paid tooling. |
| **[VerneMQ](https://vernemq.com/)** | Erlang, clusterable, OSS | EMQX alternative; smaller community. |
| **[NanoMQ](https://nanomq.io/)** | C, edge-focused | When you need a broker on the *device* itself. |
| **AWS IoT Core / Azure IoT Hub / GCP IoT Core (RIP)** | Managed | When you don't want to run a broker. (GCP killed theirs in 2023; the others are still around.) |

### Client libraries

- **[Eclipse Paho](https://www.eclipse.org/paho/)** — the reference client family: C, C++, Java, JS, Go, **Rust** (`paho-mqtt` crate), Python.
- **[rumqtt](https://github.com/bytebeamio/rumqtt)** — pure-Rust, more idiomatic than Paho's bindings. Often the better Rust pick. See [[mq]] (`programming/rust/net/mq.md`).
- **[paho.mqtt.python](https://pypi.org/project/paho-mqtt/)** — the canonical Python client.
- **[MQTT.js](https://github.com/mqttjs/MQTT.js)** — Node.js / browser-WebSocket client.

## Similar / related topics

- [[paho_mqtt]] — Eclipse Paho Rust binding (FFI-shaped, mature).
- [[mq]] — `rumqtt` — pure-Rust MQTT client, often the better Rust pick.
- [[kafka]] — when you need durable log + replay; MQTT bridges to Kafka are a common pattern (Kafka Connect MQTT, Confluent Sink Connector).
- [NATS](https://nats.io/) — alternative pub/sub bus; better for *intra-cluster* messaging, weaker for the long tail of IoT clients.
- [[drogue]] — Rust IoT framework (now archived) that ingested MQTT.
- [[shodan]] — finds your accidentally-exposed brokers on the public internet.

## Internal links

- [[paho_mqtt]] — Rust FFI binding to Eclipse Paho
- [[mq]] — `rumqtt` — pure-Rust client
- [[kafka]] — long-term-log alternative; bridges well from MQTT
- [[drogue]] — IoT framework that consumed MQTT
- [[shodan]] — for auditing your broker isn't on the public internet by accident

## Keywords

`#messaging` `#mqtt` `#iot` `#pubsub` `#mosquitto` `#hivemq` `#emqx` `#paho` `#oasis-standard`

## References / raw notes

### Pitch (from mqtt.org)

> MQTT: The Standard for IoT Messaging. MQTT is an OASIS standard messaging protocol for the Internet of Things (IoT). It is designed as an extremely lightweight publish/subscribe messaging transport that is ideal for connecting remote devices with a small code footprint and minimal network bandwidth.

### Wire structure (rough mental model)

A `PUBLISH` packet is:

```text
[ fixed header: 1 byte type+flags ][ remaining length: 1-4 bytes (variable) ]
[ topic name: 2-byte length + UTF-8 bytes ]
[ packet ID: 2 bytes (only if QoS > 0) ]
[ properties: v5 only ]
[ payload: arbitrary bytes ]
```

Minimum publish overhead is ~7 bytes for QoS 0 with a 1-char topic. Compare to a minimal HTTP/1.1 request (~70+ bytes) — that's the order-of-magnitude difference that matters on a sensor budget.

### QoS levels in one paragraph

- **QoS 0** — at most once. Fire-and-forget, no ack, message can be lost. Default and what almost all "telemetry" topics should use.
- **QoS 1** — at least once. PUBLISH → PUBACK handshake; the publisher keeps retrying until acked. Receiver may see duplicates (deduplicate by message ID at the application layer).
- **QoS 2** — exactly once. Four-step handshake (PUBLISH → PUBREC → PUBREL → PUBCOMP). Real cost; almost never worth it. Use idempotent application logic + QoS 1 instead.

### MQTT 3.1.1 vs MQTT 5

MQTT 5.0 (2019) added: response topics for request/reply, user-defined message properties, content-type metadata, shared subscriptions (load-balance across consumer group), reason codes on every ack, session expiry intervals. **Pick MQTT 5 for new work** — every modern broker supports it, and the user properties + shared subs are real ergonomics wins. Embedded clients sometimes still ship 3.1.1 only; check before committing to v5 features.

### Public test brokers

A handful of public brokers exist for hobby use (no auth, ephemeral data, do not put anything sensitive on them):

- `test.mosquitto.org` (Eclipse) — port 1883 / 8883 (TLS) / 8080 (WS)
- `broker.hivemq.com` — port 1883 / 8883
- `mqtt.eclipseprojects.io`

Wiki of community brokers: <https://github.com/mqtt/mqtt.org/wiki/public_brokers>

### Useful tutorials and entry points

- HiveMQ "Get Started" series (single best intro): <https://www.hivemq.com/blog/how-to-get-started-with-mqtt/>
- HiveMQ MQTT Essentials (free PDF, ~80 pages, deep dive): <https://www.hivemq.com/mqtt-essentials/>
- *MQTT and IoT* video intro: <https://www.youtube.com/watch?v=AzqA4ZwknW4>
- Paho MQTT Rust crate: <https://docs.rs/paho-mqtt/>
- Paho MQTT Rust async subscribe example: <https://github.com/eclipse/paho.mqtt.rust/blob/master/examples/async_subscribe.rs>
- Less-popular pure-Rust protocol-only crate: <https://docs.rs/mqtt-protocol/>

### Mosquitto in 30 seconds (macOS)

```sh
brew install mosquitto
brew services start mosquitto              # listens on tcp/1883

# In one terminal: subscribe
mosquitto_sub -h localhost -t 'sensors/#' -v

# In another: publish
mosquitto_pub -h localhost -t 'sensors/floor1/temp' -m '21.5'
mosquitto_pub -h localhost -t 'sensors/floor1/temp' -m '21.6' -r   # retained
```

### Bridge to Kafka (when telemetry needs to land in a log)

The dominant pattern for "small-device fan-in to analytics": Mosquitto/EMQX broker at the edge → MQTT-to-Kafka bridge → Kafka topic → Spark/Flink/whatever. Bridges:

- [Confluent Kafka Connect MQTT](https://www.confluent.io/hub/confluentinc/kafka-connect-mqtt) (commercial)
- [Lenses MQTT Source](https://docs.lenses.io/5.0/integrations/connectors/stream-reactor/sources/mqttsourceconnector/)
- EMQX has built-in Kafka bridging.
