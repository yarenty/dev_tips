---
title: Pulsar
main_link: https://pulsar.apache.org/
keywords: [pulsar, apache, multi, kafka, kubernetes]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Pulsar

**Main link:** <https://pulsar.apache.org/>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[kafka_2]] — Kafka _(score 28.3)_
- [[red_panda]] — Red Panda _(score 28.3)_
- [[kafka]] — Kafka _(score 28.3)_
- [[ml/bigquery/links|links]] — Links _(score 19.8)_
- [[apache]] — apache _(score 17.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#pulsar` `#messaging` `#apache` `#system` `#cloud` `#multi`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Pulsar




https://pulsar.apache.org/

## What is Apache Pulsar?
Apache Pulsar is a cloud-native, multi-tenant, high-performance solution for server-to-server messaging and queuing built on the publisher-subscribe (pub-sub) pattern. Pulsar combines the best features of a traditional messaging system like RabbitMQ with those of a pub-sub system like Apache Kafka – scaling up or down dynamically without downtime. It's used by thousands of companies for high-performance data pipelines, microservices, instant messaging, data integrations, and more.

## Pulsar Features
### Cloud-native

A multiple layer approach separating compute from storage to work with cloud infrastructures and Kubernetes.

### Serverless functions

Write serverless functions with developer-friendly APIs to natively process data immediately upon arrival. No need to run your own stream processing engine.

### Horizontally scalable

Expand capacity seamlessly to hundreds of nodes.

### Low latency with durability

Low publish latency (< 5ms) at scale with strong durability guarantees.

### Geo-replication

Configurable replication between data centers across multiple geographic regions.

### Multi-tenancy

Built from the ground up as a multi-tenant system. Supports isolation, authentication, authorization, and quotas.

### Persistent storage

Persistent message storage based on Apache BookKeeper. IO-level isolation between write, and read operations.

### Client libraries

Flexible messaging models with high-level APIs for Java, Go, Python, C++, Node.js, WebSocket and C#.

### Operability

REST Admin API for provisioning, administration, tools and monitoring. Can be deployed on bare metal, Kubernetes, Amazon Web Services(AWS), and DataCenter Operating System(DC/OS).

## Pulsar Users
Run in production at scale with millions of messages per second across millions of topics, Pulsar is now used by thousands of companies for real-time workloads.



## Docker

https://pulsar.apache.org/docs/next/deploy-docker

https://pulsar.apache.org/docs/next/deploy-bare-metal
