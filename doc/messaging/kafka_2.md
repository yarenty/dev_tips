---
title: Kafka
main_link: https://github.com/kafka-rust/kafka-rust/blob/master/examples/console-consumer.rs
keywords: [kafka-2, messaging, apache, rust, documentation, streams]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/messaging/kafka.md` by `article_split.py`. Heading: **Kafka**.

# Kafka

**Main link:** <https://github.com/kafka-rust/kafka-rust/blob/master/examples/console-consumer.rs>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#kafka-2` `#messaging` `#apache` `#rust` `#documentation` `#streams`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Kafka



https://docs.rs/kafka/latest/kafka/



https://kafka.apache.org/documentation.html#quickstart



https://kafka.apache.org/31/documentation/streams/quickstart



https://kafka.apache.org/31/documentation/streams/



## Kafka usage


```shell
/usr/local/opt/kafka/bin/kafka-topics --create --topic ufos --bootstrap-server localhost:9092

/usr/local/opt/kafka/bin/kafka-topics --describe --topic ufos --bootstrap-server localhost:9092

/usr/local/opt/kafka/bin/kafka-console-producer --topic ufos --bootstrap-server localhost:9092

/usr/local/opt/kafka/bin/kafka-console-consumer --topic ufos --from-beginning --bootstrap-server localhost:9092

```



## Rust examples:

https://github.com/kafka-rust/kafka-rust/blob/master/examples/console-consumer.rs



## Confluence

https://www.confluent.io/blog/crossing-streams-joins-apache-kafka/





---
