---
title: paho MQTT
main_link: https://crates.io/crates/paho-mqtt
keywords: [paho-mqtt, rust, programming, paho, mqtt, api, library]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/net/mq.md` by `article_split.py`. Heading: **paho MQTT**.

# paho MQTT

**Main link:** <https://crates.io/crates/paho-mqtt>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#paho-mqtt` `#rust` `#programming` `#paho` `#mqtt` `#api` `#library`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

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
