---
title: Kafka
main_link: https://github.com/fede1024/rust-rdkafka
keywords: [kafka, messaging, apache, topics, cluster]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Kafka

**Main link:** <https://github.com/fede1024/rust-rdkafka>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[red_panda]] — Red Panda _(score 33)_
- [[redpanda]] — redpanda _(score 23)_
- [[kafka_2]] — Kafka _(score 23)_
- [[apache]] — apache _(score 15)_
- [[pulsar]] — Pulsar _(score 13)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#kafka` `#messaging` `#apache` `#topics` `#topic` `#cluster`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 2 additional top-level heading(s) extracted into sibling files:
> - [Kafka](kafka_2.md)
> - [Red Panda](red_panda.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Kafka

https://developers.redhat.com/articles/2021/07/19/benchmarking-kafka-producer-throughput-quarkus#

https://engineering.linkedin.com/kafka/benchmarking-apache-kafka-2-million-writes-second-three-cheap-machines

https://www.youtube.com/watch?v=PzPXRmVHMxI

https://docs.confluent.io/platform/current/quickstart/ce-docker-quickstart.html

## Kafka - MAC

The possible easiest way!!!

```shell

brew services start zookeeper
brew services start kafka

#to see:
rpk topic consume output --brokers 127.0.0.1:9092 
 
 
```

Important directories:
```shell

# all definitions are here:
cd /usr/local/etc/kafka

# all logs are here
cd /usr/local/var/log/kafka

```
If there is error conecting to 2181 - didnt start zookeeper

More info on: https://www.conduktor.io/kafka/how-to-install-apache-kafka-on-mac-with-homebrew/



```shell
brew services stop kafka
brew services stop zookeeper
```




## KAFKA - WSL

```shell

cd /mnt/d/apps/kafka_2.13-3.2.0/

bin/zookeeper-server-start.sh config/zookeeper.properties

bin/kafka-server-start.sh config/server.properties

bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 3 --topic ufo

bin/kafka-console-consumer.sh --topic ufo --from-beginning --bootstrap-server localhost:9092  --property print.key=true --property print.timestamp=true

````


## Redpanda

```shell
rpk container start -n 3

rpk topic create ufo --brokers 127.0.0.1:50494,127.0.0.1:50500,127.0.0.1:50499 --partitions 3

rpk topic consume ufo --brokers 127.0.0.1:50494,127.0.0.1:50500,127.0.0.1:50499 --partitions 3

rpk container purge
```

## Confluence kafka

```shell
curl --output docker-compose.yml \                                                                                              https://raw.githubusercontent.com/confluentinc/cp-all-in-one/7.1.1-post/cp-all-in-one/docker-compose.yml

docker-compose up -d

docker-compose ps

docker-compose stop

docker system prune -a --volumes --filter "label=io.confluent.docker"

```




## RUST - rdkafka

https://github.com/fede1024/rust-rdkafka

```toml
rdkafka = "0.28"
```



A fully asynchronous, futures-enabled Apache Kafka client library for Rust based on librdkafka.

The library
rust-rdkafka provides a safe Rust interface to librdkafka. The master branch is currently based on librdkafka 1.8.2.


### Benchmark

https://github.com/fede1024/kafka-benchmark



## Librdkafka

https://github.com/edenhill/librdkafka/blob/master/INTRODUCTION.md#broker-version-compatibility
