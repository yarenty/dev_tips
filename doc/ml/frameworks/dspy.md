---
title: DSPy — declarative LLM programming with prompt-and-weight compilation
main_link: https://dspy.ai/
keywords: [dspy, llm, prompting, optimization, stanford, agents, rag, frameworks]
status: reviewed
---

# DSPy — declarative LLM programming with prompt-and-weight compilation

**Main link:** <https://dspy.ai/>

## Summary

DSPy ("Declarative Self-improving Python") is a framework from
Stanford NLP (Omar Khattab and collaborators) for building LLM
applications as **modular Python code** instead of brittle prompt
strings. You write `Signature`s (typed input/output specs), compose
them into `Module`s, and let DSPy's **optimisers** (`BootstrapFewShot`,
`MIPROv2`, `BootstrapFinetune`, etc.) compile your program into
optimal prompts and few-shot examples — and, if you want, into
fine-tuned weights — for whichever model you're targeting. The 1.0
release landed in late 2024 and the project is now broadly stable.

## Insight

The mental shift DSPy is selling: **stop prompt-engineering, start
*programming***. You declare what you want (a `Signature` like
`question -> answer` or `context, question -> reasoning, answer`) and
the optimiser searches for the prompt + demonstrations that maximise a
metric on your dev set. Swapping `gpt-4o-mini` for `claude-haiku` or
`llama-3.1-8b` becomes a config change, not a prompt rewrite.

When to reach for DSPy:

- You have a **measurable** task (RAG QA, classification, structured
  extraction, multi-hop reasoning) where you can write a metric.
- You're tired of hand-tuning long prompt templates that break when
  you change model.
- You want few-shot example selection done for you, on data, rather
  than by gut feel.

When *not* to reach for it:

- One-off chatbot prompts with no eval signal — the optimiser has
  nothing to optimise against.
- You need fine-grained control over every token in the system
  prompt (DSPy abstracts that away on purpose).
- Your bottleneck is tool-use orchestration / long-running stateful
  agents — that's [[langgraph]] / [[../agents/README|agents]]
  territory; DSPy plays inside a node, not as the graph.

Versus the neighbours:

| Framework      | Primary axis                                              |
|----------------|-----------------------------------------------------------|
| DSPy           | **Compile** prompts/weights from a metric                 |
| LangChain/LCEL | Glue/chain components                                     |
| [[langgraph]]  | Stateful cyclic agent graphs                              |
| LlamaIndex     | RAG-shaped data ingestion + query engines                 |
| Outlines / Instructor | Constrained / structured-output decoding           |

DSPy and a structured-output library are *complementary* — DSPy
optimises the prompt, Outlines/Instructor enforces the output shape.

Gotchas:

- The optimiser runs the model many times against your dev set —
  budget API costs accordingly.
- Programs are pickled with the compiled prompts; re-compile when you
  change model family (a prompt optimised for GPT-4o is not
  necessarily good for Claude).
- The 0.x → 1.0 transition broke some import paths and module names;
  pin a version and read the migration guide.

## Similar / related topics

- LangChain / LCEL — composable chain-of-components glue.
- [[langgraph]] — stateful, cyclic, graph-shaped agent runtime.
- LlamaIndex — RAG pipelines and query engines.
- Outlines / Instructor / `guidance` — constrained decoding for
  structured output (complementary, not competing).
- TextGrad — gradient-style prompt optimisation, similar spirit.

## Internal links

- [[README]] — frameworks landing.
- [[langgraph]] — sibling framework for stateful agents.
- [[phidata]] — agent/RAG framework with built-in observability.
- [[../rag/README|rag]] — RAG hub (DSPy is a common engine inside
  RAG pipelines).
- [[../agents/README|agents]] — agent-orchestration hub.
- [[../llm/README|llm]] — LLM landing (model-side context).

## Keywords

`#dspy` `#llm` `#prompting` `#optimization` `#stanford` `#frameworks`

## References / raw notes

- Site: <https://dspy.ai/>
- Repo: <https://github.com/stanfordnlp/dspy>
- Paper: *DSPy: Compiling Declarative Language Model Calls into
  Self-Improving Pipelines*, Khattab et al., arXiv 2310.03714 —
  <https://arxiv.org/pdf/2310.03714>
- Databricks case study (one of the better real-world write-ups):
  <https://www.databricks.com/blog/optimizing-databricks-llm-pipelines-dspy>

> Original blurb (Stanford):
>
> *DSPy is a declarative framework for building modular AI software.
> It allows you to iterate fast on structured code, rather than
> brittle strings, and offers algorithms that compile AI programs
> into effective prompts and weights for your language models,
> whether you're building simple classifiers, sophisticated RAG
> pipelines, or Agent loops. Think of DSPy as a higher-level
> language for AI programming — like the shift from assembly to C,
> or from pointer arithmetic to SQL.*
