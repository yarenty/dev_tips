---
title: LLM leaderboards — Chatbot Arena, HF Open LLM, Aider, SWE-Bench, etc.
main_link: https://lmarena.ai/
keywords: [leaderboards, evaluation, chatbot-arena, lmsys, mmlu, swe-bench, aider, livecodebench, helm, artificial-analysis]
status: reviewed
---

# LLM leaderboards — Chatbot Arena, HF Open LLM, Aider, SWE-Bench, etc.

**Main link:** <https://lmarena.ai/>

## Summary

The LLM-leaderboard landscape has fragmented into three useful axes: **human-preference Elo** (LMSYS Chatbot Arena), **standardised academic benchmarks** (HF Open LLM Leaderboard, MMLU, GPQA, MMLU-Pro, BBH, MATH), and **task-specific real-world evals** (Aider polyglot, LiveCodeBench, SWE-Bench, SWE-Bench Verified, WebArena, BFCL for tool-use). Aggregators like [Artificial Analysis](https://artificialanalysis.ai/) sit on top, providing cost/latency/quality scatter plots that are far more decision-relevant than any single number. The headline social fact is **leaderboard-gaming is real and pervasive**: any benchmark that becomes load-bearing eventually contaminates training corpora and gets optimised against, so leaderboard rank becomes a Goodhart's-law signal rather than a quality signal — which is why private-prompt evals like [SEAL](seal.md) keep being introduced.

## Insight

Use leaderboards as a **starting filter**, not a verdict. The practical decision recipe in 2025:

1. **Pick a candidate set** from Artificial Analysis (good cost/latency/quality plots) or Chatbot Arena (good for chat quality).
2. **Filter by task type** with a more specific board: Aider polyglot or LiveCodeBench for coding, SWE-Bench Verified for agentic SWE, BFCL for tool-use, GPQA / MMLU-Pro for hard-knowledge tasks (the original MMLU is too saturated to differentiate top models any more).
3. **Run your own private eval** on a held-out set of representative prompts from your real workload. This is the only number that actually matters.

The benchmark-gaming reality is now severe enough that you should explicitly distrust:

- Any open-weights release whose published numbers are only on benchmarks that pre-date the release.
- Any model that wins a single benchmark by a large margin while being middling elsewhere.
- Any leaderboard whose evaluation prompts have leaked publicly (which by now is almost all of them — see the LiveBench / SEAL / Aider strategies of rotating prompts to mitigate this).
- Aggregate scores on multi-task suites without breakdowns; "averages" hide enormous task-level variance.

For genuinely uncontaminated signal, the current best practices are: **Chatbot Arena** (battles are private and human-judged), **LiveCodeBench** (rotates problems sourced from new contests), **SEAL** (Scale's private prompts; see [[seal]]), **SWE-Bench Verified** (smaller, audited subset of SWE-Bench), and **your own private eval set** above all.

## Similar / related topics

- HELM (Stanford CRFM) — the broadest-and-deepest academic benchmark suite; slow-moving but methodologically careful.
- BIG-bench — older Google-led collaborative benchmark; mostly historical interest now.
- AlpacaEval / MT-Bench — earlier-generation LLM-judge-style benchmarks; partially superseded by Arena-Hard.
- LMSYS Arena-Hard — auto-eval distilled from Chatbot Arena's hardest prompts; cheaper than running real Arena evals.
- Vellum / OpenRouter rankings — usage-driven popularity rankings; useful for "what are people actually paying for?".

## Internal links
<!-- reviewed -->
- [[README|llm/inspection]]
- [[../README|llm]]
- [[seal]] — private-prompt sibling.
- [[../models/README|llm/models]]
- [[../../fundamentals/courses|fundamentals/courses]] — eval/metric fundamentals.

## Keywords

`#leaderboards` `#evaluation` `#chatbot-arena` `#lmsys` `#mmlu` `#swe-bench` `#aider` `#livecodebench` `#helm` `#artificial-analysis`

## References / raw notes

### Public general-purpose leaderboards

| Board | URL | Strength | Weakness |
|-------|-----|----------|----------|
| LMSYS **Chatbot Arena** (lmarena) | <https://lmarena.ai/> | Human-preference Elo; battles, hard to game | Bias toward chatty, formatting-heavy answers; some prompt categories under-represented |
| **HF Open LLM Leaderboard** | <https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard> | Standardised reproducible benchmarks for open-weights models (MMLU-Pro, GPQA, MUSR, BBH, MATH, IFEval) | Open-weights only; saturation on top models |
| **Artificial Analysis** | <https://artificialanalysis.ai/> | Cost/latency/quality scatter plots; great for selecting an API model | Not a primary benchmark; aggregates third-party results |
| **HELM** | <https://crfm.stanford.edu/helm/> | Methodologically careful; many scenarios | Updates slowly; doesn't always include the latest models |

### Coding-specific

| Board | URL | What it measures |
|-------|-----|------------------|
| **Aider polyglot** | <https://aider.chat/docs/leaderboards/> | Edit-driven coding across many languages; closely tracks real coding-agent UX |
| **LiveCodeBench** | <https://livecodebench.github.io/> | Competitive-programming problems sourced after model training cutoffs (rotates) |
| **SWE-Bench / SWE-Bench Verified** | <https://www.swebench.com/> | End-to-end issue-resolution on real GitHub repos |
| **BigCodeBench** | <https://huggingface.co/spaces/bigcode/bigcodebench-leaderboard> | More realistic-than-HumanEval coding tasks |
| **HumanEval / MBPP** (legacy) | various | Heavily contaminated; do not use as primary signal |

### Agentic / tool-use / web

- **Berkeley Function Calling Leaderboard (BFCL)** — <https://gorilla.cs.berkeley.edu/leaderboard.html>
- **WebArena** / **VisualWebArena** / **OSWorld** — web/desktop agentic tasks.
- **GAIA** — general AI assistant benchmark from Meta/HF.

### Reasoning-heavy

- **GPQA** (graduate-level Q&A; hard, low ceiling).
- **MMLU-Pro** (replaces saturated MMLU).
- **MATH** / **AIME** / **FrontierMath** for math; **ARC-AGI** for abstract reasoning.

### Private-prompt / contamination-resistant

- **SEAL** by Scale AI — private prompt sets, see [[seal]].
- **LiveBench** — Abacus / Yann LeCun's team; rotates problems monthly.
- Your own held-out eval set — always the most decision-relevant.

### The Goodhart problem in one paragraph

> Any benchmark that becomes commercially load-bearing eventually:
> - leaks into the training corpus (web crawl, instruction-tune corpora),
> - gets explicitly optimised against in post-training,
> - attracts adversarial submissions (small prompt-engineering tricks, decoding-time inference recipes that don't generalise),
> until the metric stops correlating with the underlying capability you cared about. This is why benchmark *churn* (rotating problems, private prompts, periodic refresh) is a feature, not a bug, of the modern eval landscape.
