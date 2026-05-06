---
title: providers
main_link: https://block.github.io/goose/docs/getting-started/providers
keywords: [providers, deepseek, terminal]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/ml/llm/runtimes/goose.md` by `article_split.py`. Heading: **providers**.

# providers

**Main link:** <https://block.github.io/goose/docs/getting-started/providers>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[goose]] — Goose _(score 23.8)_
- [[deepseek]] — Deepseek _(score 19.8)_
- [[extensions]] — Extensions _(score 16.0)_
- [[ollama]] — Ollama _(score 16.0)_
- [[tools/security/pake|pake]] — Pake _(score 7.8)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#providers` `#runtimes` `#llm` `#ml` `#deepseek` `#ollama` `#model` `#goose`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# providers


https://block.github.io/goose/docs/getting-started/providers


Check:
Ollama	Local model runner supporting Qwen, Llama, DeepSeek, and other open-source models. Because this provider runs locally, you must first download and run a model.

Ollama provides open source LLMs, such as DeepSeek-r1, that you can install and run locally. Note that the native DeepSeek-r1 model doesn't support tool calling, however, we have a custom model you can use with Goose.

warning
Note that this is a 70B model size and requires a powerful device to run smoothly.

Download and install Ollama from ollama.com.
In a terminal window, run the following command to install the custom DeepSeek-r1 model:


ollama run michaelneale/deepseek-r1-goose
