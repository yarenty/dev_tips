---
title: Goose
main_link: https://github.com/block/goose
keywords: [goose, runtimes, llm, ml, deepseek, ollama, block, model]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Goose

**Main link:** <https://github.com/block/goose>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#goose` `#runtimes` `#llm` `#ml` `#deepseek` `#ollama` `#block` `#model`

## TODO

- This file contains **3 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Goose

https://block.github.io/goose/

- Open Source
Built with transparency and collaboration in mind, goose empowers developers to contribute, customize, and innovate freely.

- Runs Locally
Goose runs locally to execute tasks efficiently, keeping control in your hands.

- Extensible
Customize goose with your preferred LLM and enhance its capabilities by connecting it to any external MCP server or API.

- Autonomous
Goose independently handles complex tasks, from debugging to deployment, freeing you to focus on what matters most.



https://github.com/block/goose


an open-source, extensible **AI agent** that goes beyond code suggestions
install, execute, edit, and test with any LLM




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



# Extensions

https://block.github.io/goose/docs/getting-started/using-extensions/




## Manage Goose Sessions
https://block.github.io/goose/docs/guides/managing-goose-sessions/ 



## Hints - tips


https://block.github.io/goose/docs/guides/using-goosehints
