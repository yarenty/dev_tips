---
title: blaze
main_link: 
keywords: [blaze, datafusion, data, rust, spark, native, apache, plan]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# blaze

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#blaze` `#datafusion` `#data` `#rust` `#spark` `#native` `#apache` `#plan`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# blaze

BLAZE
TPC-DS master-ce7-builds

The Blaze accelerator for Apache Spark leverages native vectorized execution to accelerate query processing. It combines the power of the Apache Arrow-DataFusion library and the scale of the Spark distributed computing framework.

Blaze takes a fully optimized physical plan from Spark, mapping it into DataFusion's execution plan, and performs native plan computation in Spark executors.

Blaze is composed of the following high-level components:

Spark Extension: hooks the whole accelerator into Spark execution lifetime.
Spark Shims: specialized codes for different versions of spark.
Native Engine: implements the native engine in rust, including:
ExecutionPlan protobuf specification
JNI gateway
Customized operators, expressions, functions
Based on the inherent well-defined extensibility of DataFusion, Blaze can be easily extended to support:

Various object stores.
Operators.
Simple and Aggregate functions.
File formats.
We encourage you to extend DataFusion capability directly and add the supports in Blaze with simple modifications in plan-serde and extension translation.
