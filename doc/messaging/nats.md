---
title: "NATS: lightweight pub/sub messaging for cloud-native services"
main_link: https://nats.io/
keywords: [nats, messaging, pubsub, jetstream, request-reply, cloud-native, cncf, go]
status: reviewed
---

# NATS: lightweight pub/sub messaging for cloud-native services

**Main link:** <https://nats.io/>

Repo: <https://github.com/nats-io/nats-server> · Docs: <https://docs.nats.io/>

## Summary

[NATS](https://nats.io/) is a CNCF-graduated, Go-based messaging system that started as a no-frills pub/sub bus and has grown into a credible all-in-one messaging story: classic pub/sub + request/reply, **JetStream** (the persistence + streaming layer), key/value store, object store, and **leaf nodes** for hub-and-spoke or geo-distributed deployments. The whole thing is one ~30 MB Go binary with no external dependencies. Original author: [Derek Collison](https://github.com/derekcollison) (also wrote the Apcera platform's messaging layer; CTO/founder of Synadia, the company behind NATS).

## Insight

Reach for NATS when:

- You want **request/reply with low latency** between microservices. NATS does request/reply natively via reply subjects; Kafka can't do this cleanly and MQTT only added it in v5.
- You want a **single binary** that does pub/sub, queues (load-balanced subscribers), persistence (JetStream), KV (like a tiny etcd), and object storage. Each piece is good enough that you don't need separate tools for each.
- You're running **edge / multi-cluster / hub-and-spoke** topologies. Leaf nodes let you run a NATS server next to your edge workloads that transparently forwards traffic up to a central cluster, with TLS, auth, and store-and-forward.
- You want **zero operational footprint** for a small bus. Drop the binary on a box, point clients at it, done.

Don't reach for NATS when:

- You need **Kafka-style durable log replay over weeks**. JetStream persists messages but its model is closer to "queue with optional history" than "log forever". For long-term replay use [[kafka]] or [[redpanda]].
- You need **Kafka ecosystem connectors** (Debezium, JDBC sink, S3 sink, Schema Registry). The ecosystem around NATS is much smaller.
- You're shopping for the **most popular** option for a given role. NATS sits adjacent to most popular categories — better than Kafka for request/reply, simpler than Pulsar for pub/sub, lighter than RabbitMQ for queues — but it's rarely the *most popular* choice for any single one.

The under-rated NATS feature: **subjects with wildcards** (`orders.*.created`, `orders.>`) make routing dramatically more expressive than topic-name string-matching in Kafka. Once you've designed a subject hierarchy that maps to your domain, much of what you'd build in Kafka with topic-naming-conventions-and-tooling falls out for free.

## Similar / related topics

- [[kafka]] / [[redpanda]] — when you need durable log + replay + huge ecosystem.
- [[messaging/mqtt|mqtt]] — different niche (sensor fan-in); NATS is the "intra-cluster bus" version of the same pub/sub idea.
- [RabbitMQ](https://www.rabbitmq.com/) — older, AMQP-focused, more queue-shaped; what NATS often replaces in microservice stacks.
- [[redis]] — Redis Streams + Pub/Sub overlap NATS's lighter use cases; Redis wins if you already have it for cache.
- [[programming/rust/net/nats|nats Rust client]] — the Rust client crate.

## Internal links

<!-- reviewed -->

- [[kafka]] — durable-log alternative for the heavy end
- [[redpanda]] — same as Kafka note
- [[messaging/mqtt|mqtt]] — sensor-fan-in alternative
- [[programming/rust/net/nats|Rust nats client]] — async Rust client crate

## Keywords

`#messaging` `#nats` `#pubsub` `#jetstream` `#request-reply` `#cloud-native` `#cncf` `#go`

## References / raw notes

### Install (macOS / Homebrew)

```sh
brew install nats-server
brew services start nats-server          # listens on tcp/4222 (clients) + 8222 (HTTP monitoring)
```

Foreground / no-launchd:

```sh
/usr/local/opt/nats-server/bin/nats-server
# Or with JetStream + monitoring + cluster
nats-server -js -m 8222 --cluster_name=demo
```

Restart after upgrade:

```sh
brew services restart nats-server
```

### Install (Docker)

```sh
docker run -p 4222:4222 -p 8222:8222 nats:latest -js -m 8222
```

### Quick smoke test with the `nats` CLI

The CLI lives in a separate package — install via `brew install nats-io/nats-tools/nats` or grab a release from <https://github.com/nats-io/natscli>.

```sh
# Subscribe in one shell
nats sub 'orders.*'

# Publish in another
nats pub orders.created '{"id": 42}'
nats pub orders.cancelled '{"id": 42}'

# Request/reply
nats reply 'help.echo' --command 'echo {{.Request}}'   # responder
nats request help.echo 'hello, NATS!'                  # client
```

### JetStream (persistence) in a few commands

```sh
# Create a stream that captures every order.* subject
nats stream add ORDERS --subjects 'orders.*' --storage=file --retention=limits --max-age=24h

# Push some messages
nats pub orders.created '{"id": 1}'
nats pub orders.created '{"id": 2}'

# Replay
nats consumer add ORDERS PULL_CONSUMER --pull --ack=explicit --replay=instant
nats consumer next ORDERS PULL_CONSUMER --count=2
```

### Useful entry points

- Site: <https://nats.io/>
- Docs: <https://docs.nats.io/>
- Server source: <https://github.com/nats-io/nats-server>
- CLI: <https://github.com/nats-io/natscli>
- JetStream concepts: <https://docs.nats.io/nats-concepts/jetstream>
- Synadia (the company; also offers NGS managed cloud): <https://synadia.com/>
