---
title: "Redpanda: Kafka-wire-compatible single-binary streaming platform"
main_link: https://redpanda.com/
keywords: [redpanda, kafka, streaming, c-plus-plus, single-binary, rpk, vectorized, no-zookeeper]
status: reviewed
---

# Redpanda: Kafka-wire-compatible single-binary streaming platform

**Main link:** <https://redpanda.com/>

Docs: <https://docs.redpanda.com/> · Repo: <https://github.com/redpanda-data/redpanda>

## Summary

[Redpanda](https://redpanda.com/) (originally from Vectorized; now Redpanda Data) is a streaming platform written in C++ that speaks the Apache Kafka wire protocol — your existing Kafka clients, Kafka Connect connectors, and Kafka Streams applications all work against it unchanged. The differences are operational: it's a **single statically-linked binary** with no JVM and no [ZooKeeper](https://zookeeper.apache.org/) (and no need for KRaft either — Redpanda has its own built-in Raft from day one), built on the [Seastar](https://seastar.io/) thread-per-core async framework. Its CLI tool [`rpk`](https://docs.redpanda.com/current/reference/rpk/) ("Redpanda Keeper") wraps cluster setup, topic management, ACLs, and a built-in Docker harness for spinning up local clusters.

## Insight

Reach for Redpanda when:

- You want **Kafka semantics** (durable log, topic/partition model, consumer groups, replay) but **don't want to run Kafka's operational footprint** (JVM, ZooKeeper or KRaft, Schema Registry, multiple JARs, GC tuning).
- You're targeting **edge / single-host / containerised** deployments. The single binary fits in a Docker image you can actually fit on a Raspberry Pi.
- You want measurably **lower tail latency**. Redpanda's thread-per-core architecture (one core per shard, no shared state, kernel-bypass for I/O when available) genuinely produces lower P99 latencies than Kafka under the same load. The benchmarks vary by workload; treat single-vendor numbers with the usual scepticism but the architectural advantage is real.

Don't reach for Redpanda when:

- You're deeply invested in the **Kafka ecosystem operator-tooling** that integrates at the JMX/admin-API level. Most works (Connect, Streams, Schema Registry) but check the specific tool. [Strimzi](https://strimzi.io/), notably, is Kafka-only.
- You need **the absolute latest Kafka feature** (e.g. KIP-848 next-gen consumer rebalance protocol) the day it lands. Redpanda tracks Kafka but lags on the bleeding-edge KIPs.
- You have a strong preference for the **Apache Foundation governance model** over a single-company OSS project. Redpanda's Community Edition is BSL-licensed (source-available, becomes Apache 2.0 after 4 years), which some procurement processes will reject.

The licensing matters: Redpanda Community Edition uses [BSL 1.1](https://github.com/redpanda-data/redpanda/blob/dev/licenses/bsl.md) — fully open source for normal use, but you can't run it as a managed competing service against Redpanda's hosted offering. For most real users this is a non-issue.

## Similar / related topics

- [[kafka]] — the protocol Redpanda implements; what to fall back to if you need every Apache feature.
- [[pulsar]] — the other "modern Kafka alternative"; very different architecture (storage/compute separation via BookKeeper).
- [WarpStream](https://www.warpstream.com/) — newer entrant; Kafka-wire-compatible but stores everything in S3 (no local disks, much cheaper at scale).
- [NATS JetStream](https://nats.io/) — different protocol entirely; lighter-weight, simpler, less Kafka-feature-complete.
- [[messaging/mqtt|mqtt]] — different niche (sensor fan-in); commonly bridged into a Redpanda topic for analytics.

## Internal links

<!-- reviewed -->

- [[kafka]] — the protocol Redpanda is wire-compatible with; same governance/ops tooling section
- [[pulsar]] — alternative "modern Kafka" with different design choices
- [[messaging/mqtt|mqtt]] — sensor-fan-in that often bridges into Redpanda

## Keywords

`#messaging` `#redpanda` `#kafka` `#streaming` `#c-plus-plus` `#single-binary` `#rpk` `#seastar` `#no-zookeeper`

## References / raw notes

### Pitch (paraphrased from the docs)

> Redpanda is a modern streaming platform for mission-critical workloads. With Redpanda you can get up and running with streaming quickly and be fully compatible with the Kafka ecosystem.

### Install on macOS (Homebrew tap)

```sh
brew install redpanda-data/tap/redpanda
rpk container start                  # spins up a local Docker-based cluster
```

This installs `rpk`, the Redpanda CLI. With Docker installed, `rpk container` provides a zero-config local cluster for testing — you don't have to set up brokers manually.

### Three-node local cluster via `rpk container`

```sh
rpk container start -n 3
# Cluster started! e.g.:
#   NODE ID  ADDRESS
#   0        127.0.0.1:50725
#   1        127.0.0.1:50726
#   2        127.0.0.1:50727

rpk cluster info --brokers 127.0.0.1:50725
# BROKERS
# =======
# ID    HOST       PORT
# 0*    127.0.0.1  50725
# ...

# When done
rpk container purge
```

### Single Docker container (1-node, manual)

For when you want a stable broker without `rpk container`'s ephemeral lifecycle:

```sh
docker run -d --pull=always --name=redpanda-1 --rm \
  -p 8081:8081 \
  -p 8082:8082 \
  -p 9092:9092 \
  -p 9644:9644 \
  docker.redpanda.com/vectorized/redpanda:latest \
  redpanda start \
  --overprovisioned \
  --smp 1 \
  --memory 1G \
  --reserve-memory 0M \
  --node-id 0 \
  --check=false
```

`--overprovisioned` accommodates Docker resource limitations (Redpanda normally wants exclusive cores). `--pull=always` keeps you on the latest image. After this, point any Kafka-compatible client at `127.0.0.1:9092`.

### Streaming demo

```sh
# Create a topic
docker exec -it redpanda-1 \
  rpk topic create twitch_chat --brokers=localhost:9092

# Produce — type messages, Ctrl+D between, Ctrl+C to exit
docker exec -it redpanda-1 \
  rpk topic produce twitch_chat --brokers=localhost:9092

# Consume
docker exec -it redpanda-1 \
  rpk topic consume twitch_chat --brokers=localhost:9092
```

Each consumed message comes back with metadata:

```json
{
  "message": "How do you stream with Redpanda?\n",
  "partition": 0,
  "offset": 1,
  "timestamp": "2021-02-10T15:52:35.251+02:00"
}
```

### Cleanup

```sh
docker stop redpanda-1 redpanda-2 redpanda-3 && \
  docker rm redpanda-1 redpanda-2 redpanda-3

# If you set up volumes/network earlier
docker volume rm redpanda1 redpanda2 redpanda3 && \
  docker network rm redpandanet
```

### Useful entry points

- Docs: <https://docs.redpanda.com/>
- FAQ: <https://docs.redpanda.com/docs/reference/faq/>
- `rpk` CLI reference: <https://docs.redpanda.com/current/reference/rpk/>
- BSL licence: <https://github.com/redpanda-data/redpanda/blob/dev/licenses/bsl.md>
- Source: <https://github.com/redpanda-data/redpanda>

### When using existing Kafka tooling against Redpanda

- **Java Kafka clients** — work unchanged.
- **`librdkafka`** (and `rdkafka` for Rust) — work unchanged.
- **Kafka Connect** — works; connectors run as normal.
- **Kafka Streams** — works.
- **Schema Registry** — Redpanda ships its own compatible Schema Registry (port `8081` in the Docker example above). Confluent's also works.
- **Strimzi** — does *not* manage Redpanda; Strimzi is Kafka-broker-specific.
- **Most YAML topic-management tools from the [[kafka]] ops section** (Julie, TopicCtl, Jikkou) — work, since they speak the Kafka admin API.
