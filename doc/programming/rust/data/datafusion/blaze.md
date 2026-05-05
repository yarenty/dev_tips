---
title: blaze
main_link: 
keywords: [blaze, rust, apache, distributed]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# blaze

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[gluten]] — gluten _(score 33.8)_
- [[rapids]] — Spark rapids _(score 33.8)_
- [[vortex]] — Vortex (2024-10-17) _(score 32.2)_
- [[delta]] — DeltaLake _(score 32.2)_
- [[iceberg]] — Iceberg _(score 32.2)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
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
