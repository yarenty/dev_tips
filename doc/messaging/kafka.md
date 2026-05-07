---
title: "Apache Kafka: distributed log + the operations ecosystem around it"
main_link: https://kafka.apache.org/
keywords: [kafka, apache, streaming, log, distributed, brokers, zookeeper, kraft, rdkafka, strimzi, julie]
status: reviewed
review_date: 2026/05/03
---

# Apache Kafka: distributed log + the operations ecosystem around it

**Main link:** <https://kafka.apache.org/>

Quickstart: <https://kafka.apache.org/quickstart> · Streams docs: <https://kafka.apache.org/documentation/streams/>

## Summary

[Apache Kafka](https://kafka.apache.org/) is the canonical distributed *log* — a horizontally-scalable, durable, ordered, replayable record store, dressed up as a publish-subscribe messaging system. Producers append records to topics; topics are partitioned across brokers; consumers read at their own pace and can replay history. Underneath it's much closer to "an ordered file system that lots of clients write into and read from" than to a queue. Originally built at LinkedIn (~2011), donated to Apache, now the ingestion layer behind effectively every modern data platform.

The deployment shape changed in 2022: Kafka used to require [Apache ZooKeeper](https://zookeeper.apache.org/) for cluster coordination, but the **KRaft** mode (Kafka Raft) replaces it with a built-in consensus layer. New deployments should use KRaft; ZooKeeper-mode is deprecated and removed in Kafka 4.0+.

## Insight

Reach for Kafka when:

- You want **durable, replayable history** — analytics teams need to re-derive a metric from yesterday's data, a new microservice needs to bootstrap from "everything that ever happened", a CDC pipeline needs to backfill.
- You have **high throughput** (10k+ messages/sec sustained) and the workload is *publish-and-many-consumers* shaped.
- You want a **bus that decouples teams**: producer and N consumers can scale independently and don't need to know about each other.

Don't reach for Kafka when:

- You need **low setup overhead**. A 3-node Kafka + Schema Registry + Kafka Connect + monitoring stack is genuinely a lot of moving parts. Try [[redpanda]] (single binary, Kafka-wire-compatible, dramatically simpler ops) or [NATS JetStream](https://nats.io/) instead.
- You need **request/reply** or **per-message ACK semantics**. Kafka is fire-and-forget at the producer; consumer acks are at the partition-offset level. Use HTTP, gRPC, or NATS for request/reply.
- You need **device-shaped fan-in** with thousands of small clients on flaky links. That's [[messaging/mqtt|MQTT]]'s niche; bridge MQTT → Kafka for the analytics tail.

The four things that bite people coming from a queue-shaped system:

1. **Topics are append-only logs, not queues.** Consumers manage their own offset; "consumed" doesn't delete anything; multiple consumer groups read the same topic independently.
2. **Partitions are the unit of parallelism *and* ordering.** Records with the same key land on the same partition (preserving per-key order) but ordering across partitions isn't preserved. Pick partition keys deliberately.
3. **Retention is by time or size, not by consumption.** Default 7 days; set to weeks/months for replay scenarios; set to "compacted" for changelog topics where you only care about the latest value per key.
4. **The Java client is the reference.** Other-language clients (`librdkafka` for C/Rust/Go/Python, the Confluent JS client, ...) sometimes lag the Java client on new features. Pick `librdkafka`-based clients for everything non-JVM.

### The wider ecosystem (where Kafka actually lives)

- **Schema Registry** — [Confluent Schema Registry](https://github.com/confluentinc/schema-registry) or [Apicurio](https://www.apicur.io/registry/) — versioned Avro/Protobuf/JSON schemas so producers and consumers stay compatible.
- **Kafka Connect** — JDBC source/sink, Debezium CDC, S3 sink, Elasticsearch sink, MQTT bridge — the connector ecosystem is massive and almost always cheaper than writing your own.
- **Kafka Streams** (Java) / **ksqlDB** — in-Kafka stream processing without spinning up a separate Flink cluster.
- **[Strimzi](https://strimzi.io/)** — the canonical way to run Kafka on Kubernetes (CRDs for clusters, topics, users, connectors).
- **Operations / governance tooling**: see the "Kafka Ops" cheat-sheet below — [Strimzi](https://strimzi.io/), [Julie Ops](https://github.com/kafka-ops/julie), [TopicCtl](https://github.com/segmentio/topicctl), [Jikkou](https://github.com/streamthoughts/jikkou), [ns4kafka](https://github.com/michelin/ns4kafka), [Koperator](https://github.com/banzaicloud/koperator), and several others, all aimed at "manage Kafka topics + ACLs declaratively from YAML in git".

## Similar / related topics

- [[redpanda]] — Kafka-wire-compatible single-binary alternative; much simpler to operate.
- [[pulsar]] — Apache Pulsar; storage/compute separation, geo-replication, multi-tenancy first-class.
- [NATS JetStream](https://nats.io/) — lighter-weight alternative; better for intra-cluster messaging.
- [[messaging/mqtt|mqtt]] — different niche (sensor fan-in); commonly bridged into Kafka for analytics.
- [[kinesis]] — AWS managed equivalent; trade-off is vendor lock-in for zero ops.
- [[mq]] — Rust client survey (kafka-rust, rdkafka, samsa).

## Internal links

- [[redpanda]] — Kafka-compatible single-binary alternative
- [[pulsar]] — Apache Pulsar — different architecture, similar problem space
- [[messaging/mqtt|mqtt]] — IoT fan-in often bridged into Kafka downstream
- [[kinesis]] — AWS managed alternative
- [[mq]] — Rust Kafka clients (kafka-rust, rdkafka, samsa)

## Keywords

`#messaging` `#kafka` `#apache` `#streaming` `#log` `#distributed` `#brokers` `#kraft` `#zookeeper` `#rdkafka` `#strimzi`

## References / raw notes

### Quickest possible local install — macOS / Homebrew

```sh
# Install both (KRaft mode is now the default; older brew formulas pulled ZooKeeper too)
brew install kafka
brew services start kafka          # listens on tcp/9092

# Older brew formulas / pre-KRaft setups also need ZooKeeper:
brew services start zookeeper      # only needed for legacy ZK-mode deployments
```

Important paths the brew formula uses:

```sh
/usr/local/etc/kafka/              # configs
/usr/local/var/log/kafka/          # logs
/usr/local/opt/kafka/bin/          # CLI tools
```

If you see `Connection refused: localhost/127.0.0.1:2181`, your setup expects ZooKeeper but it isn't running. Either start it (`brew services start zookeeper`) or upgrade to a KRaft-mode config.

Stop everything:

```sh
brew services stop kafka
brew services stop zookeeper       # if you started it
```

Reference: [How to install Apache Kafka on Mac with Homebrew (Conduktor)](https://www.conduktor.io/kafka/how-to-install-apache-kafka-on-mac-with-homebrew/).

### Quickest possible local install — Linux / WSL (manual tarball)

```sh
cd /opt
wget https://downloads.apache.org/kafka/<VERSION>/kafka_2.13-<VERSION>.tgz
tar xf kafka_2.13-<VERSION>.tgz
cd kafka_2.13-<VERSION>

# Legacy (ZooKeeper) mode
bin/zookeeper-server-start.sh config/zookeeper.properties &
bin/kafka-server-start.sh config/server.properties &

# Modern (KRaft) mode — see config/kraft/server.properties for KRaft
# bin/kafka-server-start.sh config/kraft/server.properties
```

### Topic + producer + consumer — mac / brew paths

```sh
# Create a topic
/usr/local/opt/kafka/bin/kafka-topics --create \
  --topic ufos --bootstrap-server localhost:9092 \
  --partitions 3 --replication-factor 1

# Inspect
/usr/local/opt/kafka/bin/kafka-topics --describe \
  --topic ufos --bootstrap-server localhost:9092

# Produce
/usr/local/opt/kafka/bin/kafka-console-producer \
  --topic ufos --bootstrap-server localhost:9092

# Consume from the beginning
/usr/local/opt/kafka/bin/kafka-console-consumer \
  --topic ufos --from-beginning --bootstrap-server localhost:9092 \
  --property print.key=true --property print.timestamp=true
```

Same flow against a Redpanda local cluster (Kafka-wire-compatible) using `rpk`:

```sh
rpk container start -n 3
rpk topic create ufo --brokers 127.0.0.1:50494,127.0.0.1:50500,127.0.0.1:50499 --partitions 3
rpk topic consume ufo --brokers 127.0.0.1:50494,127.0.0.1:50500,127.0.0.1:50499 --partitions 3
rpk container purge
```

### Confluent all-in-one (Schema Registry + Connect + Control Center via Docker)

```sh
curl --output docker-compose.yml \
  https://raw.githubusercontent.com/confluentinc/cp-all-in-one/7.1.1-post/cp-all-in-one/docker-compose.yml
docker-compose up -d
docker-compose ps

# Tear down + clean volumes
docker-compose stop
docker system prune -a --volumes --filter "label=io.confluent.docker"
```

### Rust clients

Two main options, picked by your runtime preference:

| Crate | Style | Notes |
|-------|-------|-------|
| **[`rdkafka`](https://github.com/fede1024/rust-rdkafka)** | Async (futures), `librdkafka` FFI | The default. Tracks `librdkafka` (currently 2.x); fully async; good throughput; production-grade. |
| **[`kafka-rust`](https://github.com/kafka-rust/kafka-rust)** | Sync, pure Rust | Smaller, no native deps; fewer features; better when you want zero C dependencies. |
| **[`samsa`](https://github.com/CallistoLabsNYC/samsa)** | Async, pure Rust | Newer; emerging alternative to `rdkafka` without the C-FFI compile pain. |

`rdkafka` quickstart:

```toml
# Cargo.toml
rdkafka = { version = "0.36", features = ["tokio"] }
```

```rust
use rdkafka::config::ClientConfig;
use rdkafka::consumer::{Consumer, StreamConsumer};
use rdkafka::Message;
use futures::StreamExt;

#[tokio::main]
async fn main() {
    let consumer: StreamConsumer = ClientConfig::new()
        .set("bootstrap.servers", "localhost:9092")
        .set("group.id", "demo-group")
        .set("enable.auto.commit", "true")
        .create()
        .expect("consumer creation failed");

    consumer.subscribe(&["ufos"]).expect("subscribe failed");

    let mut messages = consumer.stream();
    while let Some(msg) = messages.next().await {
        match msg {
            Ok(m) => println!("{:?} {:?}", m.key(), m.payload()),
            Err(e) => eprintln!("error: {e}"),
        }
    }
}
```

`kafka-rust` console-consumer example: <https://github.com/kafka-rust/kafka-rust/blob/master/examples/console-consumer.rs>.

Benchmark suite for the Rust ecosystem: <https://github.com/fede1024/kafka-benchmark>.

### `librdkafka` reference

The C library that backs `rdkafka` and most non-JVM Kafka clients. Broker-version compatibility matrix lives in <https://github.com/edenhill/librdkafka/blob/master/INTRODUCTION.md#broker-version-compatibility>.

### Kafka ops / governance tooling cheat sheet

Almost every team running Kafka past a handful of topics ends up wanting "manage topics + ACLs from YAML in git". The contenders:

| Tool | Lang | Notes |
|------|------|-------|
| **[Strimzi](https://strimzi.io/)** | Java / Kubernetes operator | Canonical Kubernetes-native option. CRDs for `Kafka`, `KafkaTopic`, `KafkaUser`, `KafkaConnect`, `KafkaConnector`. The default if you're on Kubernetes. |
| **[Julie Ops](https://github.com/kafka-ops/julie)** | Java / Docker / RPM | Topics, configuration, metadata, ACLs (traditional + Confluent RBAC). Strong topic-naming-convention enforcement. |
| **[TopicCtl](https://github.com/segmentio/topicctl)** (Segment) | Go | Declarative YAML topic management + REPL for interactive cluster exploration. |
| **[Terraform provider for Kafka](https://github.com/Mongey/terraform-provider-kafka)** | Go | If you want Kafka as just one more thing in your Terraform state. |
| **[Kafka GitOps](https://github.com/devshawn/kafka-gitops)** | Go | Desired-state YAML for topics + ACLs; works across self-hosted, managed, and Confluent Cloud. |
| **[Kafka Helmsman](https://github.com/teslamotors/kafka-helmsman)** (Tesla) | Java | Topic enforcer, consumer-freshness tracker, kafka roller, quota enforcer. |
| **[Jikkou](https://github.com/streamthoughts/jikkou)** | Java | Topics, ACLs, quotas, brokers as resource specs. Pluggable validation rules. Stateless. |
| **[ns4kafka](https://github.com/michelin/ns4kafka)** (Michelin) | Java | Kubernetes-shaped namespace isolation for multi-team Kafka. CLI is `kafkactl`, modelled on `kubectl`. |
| **[kafkaer](https://github.com/navdeepsekhon/kafkaer)** | Java | Topic + ACL automation across environments via templated configs. |
| **[Koperator](https://github.com/banzaicloud/koperator)** (Banzai/Cisco) | Go / Kubernetes operator | Production-grade Kafka on Kubernetes; advanced external access via Envoy; Cruise-Control-driven self-healing. |
| **[KafkaWize / Klaw](https://github.com/aiven-open/klaw)** | Java | Self-service web portal for topics, ACLs, schemas, connectors with role-based authorization. |

If you're on Kubernetes, start with **Strimzi**. If you're not, start with **Julie** or **Jikkou** depending on whether you prefer the more-features or the more-pluggable shape. Avoid bespoke shell scripts; this is a solved problem.

### Useful entry points for tuning and benchmarking

- LinkedIn: *Benchmarking Apache Kafka — 2 million writes/sec on three cheap machines*: <https://engineering.linkedin.com/kafka/benchmarking-apache-kafka-2-million-writes-second-three-cheap-machines>
- Red Hat: *Benchmarking Kafka producer throughput with Quarkus*: <https://developers.redhat.com/articles/2021/07/19/benchmarking-kafka-producer-throughput-quarkus>
- Confluent: *Crossing the streams: joins in Apache Kafka*: <https://www.confluent.io/blog/crossing-streams-joins-apache-kafka/>
- Confluent quickstart (CP all-in-one Docker): <https://docs.confluent.io/platform/current/quickstart/ce-docker-quickstart.html>
- Streams quickstart: <https://kafka.apache.org/31/documentation/streams/quickstart>
- Stephane Maarek's Kafka YouTube intro (still one of the best free overviews): <https://www.youtube.com/watch?v=PzPXRmVHMxI>
