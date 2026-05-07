---
title: phidata / agno — agent + RAG framework with built-in observability
main_link: https://github.com/agno-agi/agno
keywords: [phidata, agno, agents, rag, llm, observability, frameworks]
status: reviewed
---

# phidata / agno — agent + RAG framework with built-in observability

**Main link:** <https://github.com/agno-agi/agno>

## Summary

phidata is a Python framework for building LLM-backed **agents** and
**RAG** apps with first-class memory, knowledge (vector store) and
tool integrations, plus a built-in **playground UI** and observability
that the project markets as "ship to production fast". The project
**rebranded to `agno` in late 2024** (org `agno-agi/agno`) and the old
`phidata` package now redirects there. The Agent abstraction is the
core: an agent has a model, optional memory (DB-backed chat history),
optional knowledge (vector DB), and a list of `Tools` it can call.

## Insight

The pitch is a single coherent surface for the whole agent stack —
model + memory + knowledge + tools + monitoring + serving — instead
of stitching together LangChain + a vector DB + LangSmith + FastAPI
yourself. You write a `Agent(model=..., tools=[...], knowledge=..., storage=...)`,
hit `print_response("…")` for the dev loop, and serve the same agent
through Streamlit / FastAPI / Django when you're ready.

When to reach for phidata/agno:

- You want a **batteries-included** way to build an agent / RAG app
  without spending a week wiring components.
- You value a built-in **playground / monitoring UI** for the dev
  loop more than maximum flexibility.
- You're building "team of agents" patterns and want them as plain
  Python classes.

When *not* to reach for it:

- You need **stateful, cyclic, checkpointable** workflows with
  human-in-the-loop interrupts — that's [[langgraph]]'s strength.
- You want to **optimise prompts via metrics** — use [[dspy]];
  agno doesn't compile prompts.
- You need a fully provider-agnostic, vendor-neutral stack with
  the largest integration surface — LangChain still wins on raw
  breadth (at the cost of complexity).

Versus the agent neighbourhood:

| Framework         | Sweet spot                                          |
|-------------------|-----------------------------------------------------|
| phidata / agno    | All-in-one agent+RAG with playground & monitoring   |
| LangChain         | Largest integration surface, most documentation     |
| [[langgraph]]     | Stateful cyclic graphs, checkpointing, HITL         |
| LlamaIndex        | RAG-first ingest + query engines                    |
| CrewAI / AutoGen  | Multi-agent role-play / debate                      |
| [[dspy]]          | Prompt + weight compilation from metrics            |
| pydantic-ai       | Typed, code-first agents with validation focus      |

Gotchas:

- The **rebrand** is real — code on the public web that imports
  `from phi.assistant import Assistant` is from the old
  `phidata`/`phi` API; current docs use `from agno.agent import
  Agent`. Watch the date on tutorials.
- The "playground / monitoring" features push you toward the hosted
  agno service for the full experience; the OSS path works but is
  less polished.
- Like every JS-fast-moving Python agent framework, expect API
  churn — pin versions in production.

## Similar / related topics

- [[langgraph]] — sibling framework for stateful, cyclic agent
  graphs with checkpointing.
- [[dspy]] — prompt/weight compiler that fits *inside* an agent
  step.
- LangChain / LlamaIndex — the broader-scope siblings.
- CrewAI — multi-agent role-play with declarative crews.
- pydantic-ai — typed code-first agent SDK.

## Internal links

<!-- reviewed -->

- [[README]] — frameworks landing.
- [[langgraph]] — sibling agent framework.
- [[dspy]] — sibling prompt-compilation framework.
- [[../agents/README|agents]] — agent-orchestration hub.
- [[../rag/README|rag]] — RAG hub (phidata/agno is a common engine).
- [[../llm/README|llm]] — LLM landing.

## Keywords

`#phidata` `#agno` `#agents` `#rag` `#llm` `#observability`
`#frameworks`

## References / raw notes

- Current repo (post-rebrand): <https://github.com/agno-agi/agno>
- Docs: <https://docs.agno.com/>
- Original phidata repo (now redirects):
  <https://github.com/phidatahq/phidata>
- Discord, examples and tutorials are linked from the docs.

### Original (phidata-era) project pitch

> *Phidata is a framework for building Autonomous Assistants
> (Agents) using LLMs. Problem: LLMs have limited context and cannot
> take actions. Solution: add Memory (chat history in a DB),
> Knowledge (info in a vector DB) and Tools (functions the LLM can
> invoke).*
>
> Workflow:
>
> 1. Create an `Assistant` (now `Agent`).
> 2. Add Tools (functions), Knowledge (vectordb) and Storage (db).
> 3. Serve via Streamlit / FastAPI / Django.

### Quick-start (phidata-era API; check current agno docs)

```sh
pip install -U phidata
pip install openai duckduckgo-search
export OPENAI_API_KEY=sk-xxxx
python assistant.py
```

```python
from phi.assistant import Assistant
from phi.tools.duckduckgo import DuckDuckGo

assistant = Assistant(tools=[DuckDuckGo()], show_tool_calls=True)
assistant.print_response("What's happening in France?", markdown=True)
```

### Example projects (from the original README)

- **LLM OS** — using LLMs as the CPU for an emerging operating
  system.
- **Autonomous RAG** — agent with tools to search its knowledge,
  the web or chat history.
- **Local RAG** — fully local with Ollama + PgVector.
- **Investment researcher / news writer / video summariser /
  research assistant** — Llama-3 + Groq examples.
