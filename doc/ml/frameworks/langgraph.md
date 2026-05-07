---
title: LangGraph — stateful, graph-based agent orchestration
main_link: https://langchain-ai.github.io/langgraph/
keywords: [langgraph, langchain, agents, orchestration, state-machine, frameworks]
status: reviewed
---

# LangGraph — stateful, graph-based agent orchestration

**Main link:** <https://langchain-ai.github.io/langgraph/>

## Summary

LangGraph is the LangChain team's low-level orchestration framework
for **stateful, cyclic** agent workflows. Where vanilla LangChain
(LCEL) composes components into directed acyclic chains, LangGraph
expresses the workflow as an explicit **graph** with named nodes,
typed shared state, and edges that can loop, branch, or hand off to
a human. It ships with first-class checkpointing, human-in-the-loop
interrupts, and integrates with **LangSmith** for tracing/eval and
**LangGraph Cloud / Platform** for hosted deployment.

## Insight

The reason LangGraph exists is that real agents have cycles —
"think, call a tool, observe, think again until done" — and they
need to remember things across hundreds of steps and across crashes.
Trying to express that as an LCEL chain works for one or two
iterations and then turns into a tangle. LangGraph models the same
problem as a state machine: nodes are pure functions over a shared
`State` `TypedDict`, edges encode transitions, and the runtime
takes care of persisting state to a checkpointer (SQLite / Postgres
/ Redis) so a run can be paused, resumed, or branched.

When to reach for LangGraph:

- The workflow has **loops** — ReAct-style agents, plan-execute-replan
  loops, multi-agent debate, supervisor + workers, etc.
- You need **human-in-the-loop** approvals or edits between steps.
- You need to **resume** long runs after a crash, or **time-travel**
  / fork from an earlier checkpoint to debug.
- You're already on the LangChain stack and want LangSmith tracing
  out of the box.

When *not* to reach for it:

- The workflow is a clean DAG with no loops — vanilla LangChain LCEL
  or even raw `requests + json` is simpler.
- You want a code-first agent SDK with no graph DSL — try
  `pydantic-ai`, OpenAI Agents SDK, or a lighter framework like
  CrewAI or [[phidata]].
- You only need *prompt optimisation* — that's [[dspy]]'s job; the
  two are complementary (DSPy inside a node, LangGraph around it).

LangGraph vs the neighbourhood:

| Need                                           | Reach for                |
|------------------------------------------------|--------------------------|
| Linear chain, no state                         | LangChain LCEL           |
| Cyclic agent, persistent state, checkpointing  | **LangGraph**            |
| Multi-agent role-play / crew-style             | CrewAI, AutoGen          |
| Production agent SDK with built-in obs         | [[phidata]] (agno)       |
| Optimise *prompts* via metrics                 | [[dspy]]                 |
| RAG-shaped data + query                        | LlamaIndex               |

The LangGraph **Platform / Cloud** is the hosted-runtime / state-store
side of the product; you can self-host the same components (Postgres
checkpointer + the OSS server) for free. LangSmith is the
observability/eval companion; it's separate but very tightly
integrated, and the dev loop (run → trace → annotate → eval) is one
of the more polished in the agent space today.

Gotchas:

- LangChain churns fast; pin both `langchain` and `langgraph` and
  expect to update intentionally.
- Shared `State` is mutated by reducers — `Annotated[list, operator.add]`
  to append, plain field to overwrite. Forgetting the reducer is the
  most common bug.
- Checkpointers persist *every* state transition by default; that's
  great for replay, painful for storage if your state holds large
  blobs. Trim what you put in `State`.

## Similar / related topics

- LangChain (LCEL) — the parent project, DAG-shaped composition.
- CrewAI — opinionated multi-agent role-play framework.
- AutoGen — Microsoft Research's multi-agent conversation framework.
- OpenAI Agents SDK — provider-native agents loop.
- [[phidata]] (renamed to **agno**) — agent/RAG with built-in
  observability and a playground UI.
- LangSmith — observability/eval companion to LangChain/LangGraph.
- Temporal / Restate — generic durable-execution engines (the
  same pattern, language-agnostic).

## Internal links

<!-- reviewed -->

- [[README]] — frameworks landing.
- [[dspy]] — sibling framework, complementary (prompt compilation).
- [[phidata]] — sibling agent/RAG framework with built-in
  observability.
- [[../agents/README|agents]] — agent-orchestration hub.
- [[../llm/README|llm]] — LLM landing.
- [[../rag/README|rag]] — RAG hub (LangGraph is commonly the
  control flow around a RAG retriever).

## Keywords

`#langgraph` `#langchain` `#agents` `#orchestration` `#state-machine`
`#frameworks`

## References / raw notes

- Docs: <https://langchain-ai.github.io/langgraph/>
- Repo: <https://github.com/langchain-ai/langgraph>
- LangSmith (observability/eval companion): <https://smith.langchain.com/>
- LangGraph Platform (hosted runtime):
  <https://www.langchain.com/langgraph-platform>
- UiPath × LangGraph case study (agentic automation in RPA):
  <https://www.uipath.com/blog/product-and-updates/langgraph-uipath-advancing-agentic-automation-together>

> Production users frequently cited by the project: Replit, Uber,
> LinkedIn, GitLab, Klarna, Elastic.
