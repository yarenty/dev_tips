---
title: smolagents
main_link: https://huggingface.co/blog/smolagents
keywords: [smolagents, agents, ml, hub, code, models]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# smolagents

**Main link:** <https://huggingface.co/blog/smolagents>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[llama_2]] — LLAMA _(score 25)_
- [[ollama]] — Ollama _(score 20)_
- [[providers]] — providers _(score 20)_
- [[anmell]] — anmell _(score 18)_
- [[hf]] — Hf _(score 18)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#smolagents` `#agents` `#ml` `#hub` `#code` `#model` `#support`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# smolagents

https://huggingface.co/blog/smolagents


smolagents is a library that enables you to run powerful agents in a few lines of code. It offers:

✨ Simplicity: the logic for agents fits in ~1,000 lines of code (see agents.py). We kept abstractions to their minimal shape above raw code!

🧑‍💻 First-class support for Code Agents. Our CodeAgent writes its actions in code (as opposed to "agents being used to write code"). To make it secure, we support executing in sandboxed environments via E2B or via Docker.

🤗 Hub integrations: you can share/pull tools or agents to/from the Hub for instant sharing of the most efficient agents!

🌐 Model-agnostic: smolagents supports any LLM. It can be a local transformers or ollama model, one of many providers on the Hub, or any model from OpenAI, Anthropic and many others via our LiteLLM integration.

👁️ Modality-agnostic: Agents support text, vision, video, even audio inputs! Cf this tutorial for vision.

🛠️ Tool-agnostic: you can use tools from LangChain, MCP, you can even use a Hub Space as a tool.
