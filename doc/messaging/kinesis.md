---
title: "Amazon Kinesis: AWS-managed streaming (Data Streams + Firehose + others)"
main_link: https://aws.amazon.com/kinesis/
keywords: [kinesis, aws, managed-streaming, firehose, kpl, kcl, data-streams, video-streams]
status: reviewed
---

# Amazon Kinesis: AWS-managed streaming (Data Streams + Firehose + others)

**Main link:** <https://aws.amazon.com/kinesis/>

Getting started: <https://aws.amazon.com/kinesis/getting-started/>

## Summary

Kinesis is AWS's managed streaming family. The name is overloaded — there are actually **four products** under the brand:

- **Kinesis Data Streams** — durable, ordered, replayable streams with explicit shards. The closest analogue to [[kafka]] / [[redpanda]].
- **Kinesis Data Firehose** (now "Amazon Data Firehose") — fully managed deliver-it-somewhere ingest: source → optional transform → S3 / Redshift / OpenSearch / Splunk. No code, no consumers to write.
- **Kinesis Data Analytics** (now "Managed Service for Apache Flink") — stateful stream processing in SQL or Java/Python via Flink, against streams.
- **Kinesis Video Streams** — separate service for video / camera / time-series binary blobs with WebRTC support.

## Insight

Reach for Kinesis when:

- You're **already deeply in AWS** and the integration cost (IAM, CloudWatch, VPC, Lambda triggers, S3/Redshift sinks via Firehose) is significant. The whole pipeline lights up with managed glue.
- You don't want to **operate Kafka brokers**. No JVM, no ZooKeeper, no node failures, no rebalancing surgery — AWS handles it. The cost is the per-shard / per-PUT pricing, which gets eye-watering at high throughput compared to running your own [[redpanda]].
- You want **Firehose** specifically. The "ingest into S3 / Redshift / OpenSearch with zero code" use case is *very* nice when it fits and not really replicable elsewhere.

Don't reach for Kinesis when:

- You're not in AWS, or you want to stay portable. Kinesis is the most vendor-locked of the streaming options on the market.
- You need **per-record latency under ~70 ms** at the consumer. Kinesis polls at fixed intervals; tail latency is much higher than Kafka or NATS. (Kinesis Enhanced Fan-Out improves this somewhat at additional cost.)
- You'll exceed ~1 MB/s per shard. Past that you have to manually re-shard, and re-sharding Kinesis is genuinely painful compared to Kafka partition rebalancing.

The pricing model deserves a special mention: Kinesis Data Streams charges per shard-hour + per PUT-payload-unit, plus extra for Enhanced Fan-Out, plus extra for extended retention. At high throughput a self-hosted Kafka or Redpanda cluster becomes dramatically cheaper. Run the numbers on your specific workload before committing.

## Similar / related topics

- [[kafka]] — open-source self-hosted equivalent; cheaper at scale, more operational overhead.
- [[redpanda]] — Kafka-wire-compatible single-binary alternative; runs anywhere including AWS.
- [[pulsar]] — Apache Pulsar; similar problem space, more architectural complexity.
- [Google Pub/Sub](https://cloud.google.com/pubsub) — GCP's roughly-equivalent managed offering.
- [Azure Event Hubs](https://azure.microsoft.com/en-us/products/event-hubs/) — Microsoft's equivalent; speaks the Kafka protocol natively.
- [[messaging/mqtt|mqtt]] — different niche; AWS IoT Core handles MQTT and routes into Kinesis Firehose.

## Internal links

<!-- reviewed -->

- [[kafka]] — open-source self-hosted alternative
- [[redpanda]] — Kafka-compatible self-hosted alternative
- [[pulsar]] — Apache Pulsar alternative
- [[messaging/mqtt|mqtt]] — AWS IoT Core uses MQTT and routes to Kinesis

## Keywords

`#messaging` `#kinesis` `#aws` `#managed-streaming` `#firehose` `#data-streams` `#video-streams` `#kpl` `#kcl`

## References / raw notes

### Pitch (paraphrased from the AWS page)

> Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. With Amazon Kinesis, you can ingest real-time data such as video, audio, application logs, website clickstreams, and IoT telemetry data for machine learning, analytics, and other applications.

### The four sub-products in one table

| Service | What it does | When to use |
|---------|--------------|-------------|
| **Kinesis Data Streams** | Durable ordered streams with explicit shards | When you want Kafka-like semantics inside AWS without operating Kafka |
| **Amazon Data Firehose** (was Kinesis Data Firehose) | Fully managed source → optional transform → S3 / Redshift / OpenSearch / Splunk delivery | When the destination is one of the supported sinks and you want zero code |
| **Managed Service for Apache Flink** (was Kinesis Data Analytics) | Stateful stream processing in SQL / Flink Java / Python | When you'd otherwise be running your own Flink cluster |
| **Kinesis Video Streams** | Video / time-series binary streams; WebRTC; HLS / MPEG-DASH playback | Camera / surveillance / real-time video pipelines |

### Quick AWS CLI smoke test (Data Streams)

```sh
# Create a 1-shard stream
aws kinesis create-stream --stream-name demo --shard-count 1

# Wait for ACTIVE
aws kinesis describe-stream --stream-name demo \
  --query 'StreamDescription.StreamStatus'

# Put a record
aws kinesis put-record --stream-name demo \
  --data $(echo -n 'hello' | base64) \
  --partition-key key-1

# Get the shard iterator and read
aws kinesis get-shard-iterator --stream-name demo \
  --shard-id shardId-000000000000 \
  --shard-iterator-type TRIM_HORIZON \
  --query 'ShardIterator' --output text \
  | xargs -I {} aws kinesis get-records --shard-iterator {}
```

### Producer / consumer libraries

- **KPL** (Kinesis Producer Library, Java) — high-throughput producer with batching + record aggregation.
- **KCL** (Kinesis Client Library, Java / Python / Node / Ruby / .NET) — handles checkpointing + lease coordination via DynamoDB.
- **AWS SDK** — every-language SDK has direct Kinesis support; use this for simple cases.
- **`amazon-kinesis-producer`** for languages with no KPL binding tend to use the bundled native daemon.

For Rust, the AWS SDK (`aws-sdk-kinesis`) is the official path; there's no first-class KCL equivalent.

### What you'll find archived

The repo previously linked (`amazon-archives/aws-weathergen`) is a 2017-era weather-data-streaming demo. It's archived but is still useful as a reference for the "ingest IoT-shaped data into Kinesis" pattern. Modern AWS examples live under <https://github.com/aws-samples/>.

### Useful entry points

- Product hub: <https://aws.amazon.com/kinesis/>
- Pricing calculator: <https://aws.amazon.com/kinesis/data-streams/pricing/>
- Firehose docs: <https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html>
- KCL repo: <https://github.com/awslabs/amazon-kinesis-client>
- Comparison: *Kinesis vs Kafka* (Confluent's perspective, take with sodium): <https://www.confluent.io/learn/kafka-vs-kinesis/>
