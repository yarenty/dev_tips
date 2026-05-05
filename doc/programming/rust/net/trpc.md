---
title: tRPC
main_link: https://trpc.io/
keywords: [trpc, rust, automatic, end]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# tRPC

**Main link:** <https://trpc.io/>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[paho_mqtt]] — paho MQTT _(score 18.7)_
- [[tunelling]] — Bore _(score 18.7)_
- [[grpc]] — GRPC _(score 18.7)_
- [[mq]] — ruMQTT _(score 18.7)_
- [[rtic]] — RTIC _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#trpc` `#rust` `#programming` `#automatic` `#end` `#easy` `#add`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# tRPC


https://trpc.io/

End-to-end typesafe APIs made easy


- Automatic typesafety
Automatic typesafety & autocompletion inferred from your API-paths, their input data, & outputs.
 
- Light & Snappy DX
No code generation, run-time bloat, or build pipeline. Zero dependencies & a tiny client-side footprint.

- Add to existing brownfield project
Easy to add to your existing brownfield project with adapters for Connect/Express/Next.js.

trpc/client depends on some babel runtime helpers + that a fetch() polyfill/ponyfill is used if the browser doesn't support it. @trpc/react is built on top of react-query.
