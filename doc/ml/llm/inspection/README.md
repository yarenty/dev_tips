---
title: LLM inspection — leaderboards, attention viz, alignment surgery
keywords: [inspection, evaluation, leaderboards, interpretability, safety, alignment]
status: reviewed
---

# LLM inspection — leaderboards, attention viz, alignment surgery

This section collects tools and resources for **inspecting** LLM behaviour: where they sit on public leaderboards, how to visualise their internals, and the research-grade techniques used to probe (or undo) alignment. It is the diagnostic counterpart to the model articles in [[../models/README|llm/models]] and the runtime articles in [[../runtimes/README|llm/runtimes]].

## What's here

### Leaderboards and benchmarks

- [LLM leaderboards landscape](leaderboards.md) — Chatbot Arena (LMSYS), HF Open LLM Leaderboard, AlpacaEval, MT-Bench, MMLU, GPQA, Aider polyglot, SWE-Bench, LiveCodeBench, ArtificialAnalysis, HELM. Includes the Goodhart's-law caveats.
- [Scale's SEAL leaderboard](seal.md) — private-prompt evals run by Scale AI to dodge benchmark contamination.

### Interpretability and attention visualisation

- [Inspectus — Jupyter attention/activation viz](inspectus.md) — labmlai's lightweight viz library; useful for "what is the model attending to here?".

### Alignment / safety research techniques

- [Abliteration — un-aligning LLMs by orthogonalising the refusal direction](abliteration.md) — Maxime Labonne's recipe; safety-research curiosity, debated practical value.

## Decision shortcut

| You want to… | Reach for |
|--------------|-----------|
| Pick the best general-purpose chat model right now | [Chatbot Arena](https://lmarena.ai/) — community vote, hard to game |
| Compare open-weights models on standardised tasks | [HF Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard) |
| Compare on coding | [Aider polyglot](https://aider.chat/docs/leaderboards/) or [LiveCodeBench](https://livecodebench.github.io/) |
| Compare on agentic SWE | [SWE-Bench](https://www.swebench.com/) |
| Get private-prompt-eval numbers (no contamination) | [Scale SEAL](seal.md) |
| Get cost/latency/quality trade-offs at a glance | [Artificial Analysis](https://artificialanalysis.ai/) |
| Visualise attention in a notebook | [Inspectus](inspectus.md), BertViz, TransformerLens |
| Strip refusals from a model (research) | [Abliteration](abliteration.md) |

## Cross-section see-also

- [[../README|llm]] — parent landing.
- [[../models/README|llm/models]] — the things you're inspecting.
- [[../../tools/README|ml/tools]] — broader ML tools / workflow utilities.
- [[../../fundamentals/README|ml/fundamentals]] — eval / metric foundations.

## Keywords

`#inspection` `#evaluation` `#leaderboards` `#interpretability` `#safety` `#alignment`
