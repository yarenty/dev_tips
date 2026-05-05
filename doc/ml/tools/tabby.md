---
title: TabbyML
main_link: https://github.com/TabbyML/tabby
keywords: [tabby, ide, self, gpus, cuda]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# TabbyML

**Main link:** <https://github.com/TabbyML/tabby>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tract]] — tract _(score 16.0)_
- [[unsloth]] — unsloth _(score 13.3)_
- [[candle]] — Candle _(score 10.9)_
- [[deeplake]] — DeepLake _(score 10.4)_
- [[compilation_cache]] — sccache - Shared Compilation Cache _(score 6.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#tabby` `#tools` `#ml` `#ide` `#self` `#open` `#cloud`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# TabbyML

https://github.com/TabbyML/tabby


Tabby is a self-hosted AI coding assistant, offering an open-source and on-premises alternative to GitHub Copilot. It boasts several key features:

Self-contained, with no need for a DBMS or cloud service.
OpenAPI interface, easy to integrate with existing infrastructure (e.g Cloud IDE).
Supports consumer-grade GPUs.
Open in Playground

Demo

🔥 What's New
04/22/2024 v0.10.0 released, featuring the latest Reports tab with team-wise analytics for Tabby usage.
04/19/2024 📣 Tabby now incorporates locally relevant snippets(declarations from local LSP, and recently modified code) for code completion!
04/17/2024 CodeGemma and CodeQwen model series have now been added to the official registry!
Archived
👋 Getting Started
You can find our documentation here.

📚 Installation
💻 IDE/Editor Extensions
⚙️ Configuration
Run Tabby in 1 Minute
The easiest way to start a Tabby server is by using the following Docker command:

docker run -it \
--gpus all -p 8080:8080 -v $HOME/.tabby:/data \
tabbyml/tabby \
serve --model TabbyML/StarCoder-1B --device cuda
For additional options (e.g inference type, parallelism), please refer to the documentation page.

🤝 Contributing
