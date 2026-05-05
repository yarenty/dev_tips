---
title: ruMQTT
main_link: https://github.com/bytebeamio/rumqtt
keywords: [mq, rust, programming, nats, mqtt, paho, jetstream]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# ruMQTT

**Main link:** <https://github.com/bytebeamio/rumqtt>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#mq` `#rust` `#programming` `#nats` `#mqtt` `#paho` `#jetstream`

## TODO

- This file contains **3 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# ruMQTT

https://github.com/bytebeamio/rumqtt


rumqtt is an opensource set of libraries written in rust-lang to implement the MQTT standard while striving to be simple, robust and performant.


| Crate |	Description	|
|----|----|
| rumqttc	| A high level, easy to use mqtt client	|
| rumqttd	| A high performance, embeddable MQTT broker |	


## NATIVE!


# paho MQTT


https://crates.io/crates/paho-mqtt

The Eclipse Paho MQTT Rust client library on memory-managed operating systems such as Linux/Posix, Mac, and Windows.

The Rust crate is a safe wrapper around the Paho C Library.

## Features
The initial version of this crate is a wrapper for the Paho C library, and includes all of the features available in that library, including:

Supports MQTT v5, 3.1.1, and 3.1
Network Transports:
Standard TCP support
SSL / TLS (with optional ALPN protocols)
WebSockets (secure and insecure), and optional Proxies
QoS 0, 1, and 2
Last Will and Testament (LWT)
Message Persistence
File or memory persistence
User-defined key/value persistence (including example for Redis)
Automatic Reconnect
Offline Buffering
High Availability
Several API's:
Async/Await with Rust Futures and Streams for asynchronous operations.
Traditional asynchronous (token/wait) API
Synchronous/blocking API



# NATS

https://crates.io/crates/nats


## NATS 2.2
NATS 2.2 is the largest feature release since version 2.0. The 2.2 release provides highly scalable, highly performant, secure and easy-to-use next generation streaming in the form of JetStream, allows remote access via websockets, has simplified NATS account management, native MQTT support, and further enables NATS toward our goal of securely democratizing streams and services for the hyperconnected world we live in.

## Next Generation Streaming

JetStream is the next generation streaming platform for NATS, highly resilient, highly available, and easy to use. We’ve spent a long time listening to our community, learning from our experiences, looking at the needs of today, and thinking deeply about the needs of tomorrow. We built JetStream to address these needs.
JetStream:
- is easy to deploy and manage, built into the NATS server
- simplifies and accelerates development
- supports wildcard subjects
- supports at least once delivery and exactly once within a window
- is horizontally scalable at runtime with no interruptions
- persists data via streams and delivers or replays via consumers
- supports multiple patterns to consume data on the same stream
- supports push and pull modes when consuming messages
- is account aware
- allows for detailed granularity of security, by stream, by consumer, by function
