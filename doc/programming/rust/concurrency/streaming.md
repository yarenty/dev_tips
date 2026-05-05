---
title: par-stream
main_link: https://github.com/jerry73204/par-stream
keywords: [streaming, rust, parallel, streams]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# par-stream

**Main link:** <https://github.com/jerry73204/par-stream>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[actors]] — Actix _(score 17.1)_
- [[timely]] — Timely Dataflow _(score 17.1)_
- [[salsa]] — Salsa _(score 17.1)_
- [[approximate_continuous_querying_over_distributed_streams]] — Approximate Continuous Querying  over Distributed Streams _(score 14.7)_
- [[rtic]] — RTIC _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#streaming` `#concurrency` `#rust` `#programming` `#parallel` `#stream` `#par` `#code`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->


# par-stream

https://github.com/jerry73204/par-stream


par-stream is an asynchronous parallel stream processing library for Rust.


https://github.com/jerry73204/par-stream/blob/master/src/par_stream.rs



Examples
- Ordered parallel processing dataflow (code)
- Unordered parallel processing dataflow (code)
- Scatter and gather (code)
- Parallel merge-sort (code)
- Parallel shuffle (code)


Good example is in those web collector:

https://github.com/lycheeverse/lychee/blob/master/lychee-lib/src/collector.rs
