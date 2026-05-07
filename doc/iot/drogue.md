---
title: "Drogue IoT: Rust-based device-to-cloud framework (now retired)"
main_link: https://book.drogue.io/
keywords: [drogue, iot, rust, embedded, device-to-cloud, mqtt, lorawan, kubernetes, archived]
status: reviewed
---

# Drogue IoT: Rust-based device-to-cloud framework (now retired)

**Main link:** <https://book.drogue.io/>

Repos: <https://github.com/drogue-iot> · Original site: <https://www.drogue.io/> (intermittent)

> **Status note (2024):** the Drogue IoT project is no longer actively developed. Red Hat retired it in late 2023. The repos and book are still online and the patterns are still instructive, but if you're picking *new* infrastructure today see the alternatives section below.

## Summary

Drogue IoT was a Red Hat-sponsored Rust framework for the full IoT stack — embedded device firmware (`drogue-device`) plus a Kubernetes-native cloud back-end (`drogue-cloud`) that ingested telemetry over **MQTT**, **HTTP**, **LoRaWAN**, or **CoAP**, normalised it as **CloudEvents**, and routed it onward to Kafka or via Knative. The pitch was "the same Rust code on the microcontroller and on the cloud, with all the device-to-cloud plumbing solved for you".

## Insight

Worth knowing about historically because Drogue made some genuinely interesting design choices that are still worth borrowing even though the project itself is over:

- **CloudEvents as the wire format** between device and cloud. This decoupled *what protocol the device speaks* (MQTT, HTTP, LoRaWAN, CoAP) from *what downstream applications consume*. The pattern is good; the Drogue implementation isn't the only place to get it.
- **Async Rust on `no_std`** in the device firmware (`drogue-device`, built on top of [Embassy](https://embassy.dev/)) was an early proof that microcontrollers don't need to be C-only.
- **Operator-friendly cloud side**: deploy with Helm onto any Kubernetes; bring-your-own broker, identity, and Kafka.

For *new* projects today, look at:

- **[ThingsBoard](https://thingsboard.io/)** — open-source Java IoT platform; closest "everything in one box" replacement.
- **[Eclipse Hono](https://www.eclipse.org/hono/)** — sibling project, still active; what Drogue's cloud side most resembled.
- **[EMQX](https://www.emqx.io/)** + **[Kafka Connect MQTT](https://www.confluent.io/hub/confluentinc/kafka-connect-mqtt)** — DIY stack: managed MQTT broker + bridge to Kafka + your own analytics layer.
- **[[messaging/mqtt|MQTT]]** + **[NATS](https://nats.io/)** — even simpler if you don't need full IoT-platform features.
- For embedded Rust on its own: **[Embassy](https://embassy.dev/)** is the spiritual successor to `drogue-device` and is actively maintained.

## Similar / related topics

- [[messaging/mqtt|mqtt]] — the most-common transport Drogue (and its replacements) ingest.
- [[shodan]] — different angle on IoT: Drogue is "how to build it", Shodan is "how exposed everyone else's is".
- [[balena]] — different layer of the stack (device OS + fleet management) but adjacent problem space.
- [[k3s]] — what `drogue-cloud` would have run on for self-hosting.
- [Embassy](https://embassy.dev/) — async Rust embedded framework; what the device side was built on.

## Internal links

- [[messaging/mqtt|mqtt]] — primary device-to-cloud transport
- [[shodan]] — IoT security / discovery angle (sister article in this section)
- [[balena]] — alternative IoT stack layer
- [[k3s]] — Kubernetes substrate Drogue Cloud was deployed on

## Keywords

`#iot` `#drogue` `#rust` `#embedded` `#device-to-cloud` `#mqtt` `#lorawan` `#kubernetes` `#cloudevents` `#archived`

## References / raw notes

### Pitch (paraphrased from the project's site, while it lasted)

> The Drogue IoT project aims to bring together data and users in an Internet of Things world.
>
> Data comes from sensors and needs to travel a long way to reach its users. Of course, there is also the way back. All of these steps in the middle are still needed and must be managed in some way.
>
> Drogue IoT provides you with tools, to create safe and efficient IoT solutions. Starting on the embedded side up to the cloud layer.

### What was in the box

| Component | Purpose |
|-----------|---------|
| `drogue-device` | Async Rust framework for `no_std` microcontrollers (built on Embassy). |
| `drogue-cloud` | Kubernetes-native back-end: MQTT/HTTP/LoRaWAN/CoAP ingress → CloudEvents → Kafka / Knative. |
| `drt` (CLI) | Tool for managing devices and applications in `drogue-cloud`. |
| Drogue book | Tutorial-style docs; still online: <https://book.drogue.io/> |

### Useful entry points (still online)

- Docs / book: <https://book.drogue.io/>
- GitHub org: <https://github.com/drogue-iot>
- Embassy (the async-embedded layer underneath): <https://embassy.dev/>

### Why mention an archived project at all?

Two reasons. First, the *patterns* (CloudEvents-shaped IoT ingestion, async Rust on microcontrollers) outlived the project and are worth knowing. Second, if you find a Drogue-shaped tutorial or PoC on someone's blog, you should know that following it past the "hello world" stage will hit dead links and abandoned issues.
