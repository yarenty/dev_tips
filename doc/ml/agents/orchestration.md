---
title: Agent orchestration — LangGraph, CrewAI, AutoGen, Burr, Parlant, Camunda
main_link: https://langchain-ai.github.io/langgraph/
keywords: [orchestration, langgraph, crewai, autogen, burr, parlant, camunda, agentic-workflow]
status: reviewed
review_date: 2026/05/03
---

# Agent orchestration — LangGraph, CrewAI, AutoGen, Burr, Parlant, Camunda

**Main link:** <https://langchain-ai.github.io/langgraph/>

## Summary

Once an agent system has more than one LLM call, you've got an **orchestration** problem: how do the calls connect, what's the state, how do you handle errors / retries / human-in-the-loop, and how do you observe the whole thing? **LangGraph** (LangChain's spinout, used by Replit, Uber, LinkedIn, GitLab) is the most-popular Python answer in late 2025; it models the agent as a graph of nodes communicating through shared typed state. Other contenders cover the same territory with different abstractions: **CrewAI** (role-based crews), **AutoGen** (Microsoft, multi-agent conversations), **Burr** (state machines), **Parlant** (rule-based control), **Camunda** (BPMN-based, enterprise-leaning). All map cleanly onto the workflow patterns in [[claude]] (prompt chaining / routing / parallelisation / orchestrator-workers / evaluator-optimiser).

## Insight

The decision-shortcut question is **what's your control-flow shape**:

| Shape | Pick |
|---|---|
| Linear chain of LLM calls | LangChain (LCEL), or just write the function |
| Graph with cycles, checkpoints, HITL | **LangGraph** |
| Several agents talking to each other in roles | **CrewAI** or **AutoGen** |
| Strict state machine with explicit transitions | **Burr** |
| Conversational agent with hard rule-based guardrails | **Parlant** |
| Enterprise process orchestration (auditable, BPMN) | **Camunda** with agentic extension |
| You need full provider neutrality + observability | LangGraph + LangSmith / Arize / Langfuse |

**LangGraph specifics.** LangGraph's killer feature isn't graphs (every framework has those); it's the **checkpointer** + **interrupt** machinery for human-in-the-loop, the **`Annotated[..., reducer]`** state-merging pattern, and the **time-travel debugging** in LangSmith. The graph compiles to an `app.invoke()` / `app.stream()` callable; the parallelisation primitive is "send several edges from the same node, the runtime fans out".

**Vs picking nothing.** A surprising amount of agentic work doesn't need a framework. If your "agent" is a single LLM call with tool use, write the loop. The frameworks earn their weight when you have ≥3 LLM calls, branching control flow, persistent state, and a human reviewer somewhere.

**Common gotchas.**

- **State explosion.** Putting everything in shared state makes graphs unreadable. Promote to durable storage (DB, vector store) early.
- **The `Annotated[..., operator.add]` reducer footgun.** Without an explicit reducer, parallel-branch updates clobber each other. Always declare reducers for fields multiple nodes write.
- **Provider lock-in by accident.** LangGraph is provider-neutral, but `langchain_openai`-flavoured nodes will drag you to OpenAI. Use the universal `init_chat_model` helper.
- **Observability is non-optional.** Once you have cycles + retries + HITL, an unobserved agent is a debugging nightmare. LangSmith, Langfuse, Arize Phoenix, OpenLLMetry all integrate.

**Note on `ml/orchestration/`.** There's a separate `ml/orchestration/` section (P5.AG upcoming) that covers a different angle (likely workflow systems / Airflow-shaped). This article is *agent-side* orchestration; cross-link both when relevant.

## Similar / related topics

- **LangGraph** — graph + state for stateful agents; the de facto Python choice.
- **CrewAI** — role-based multi-agent (Researcher / Writer / Reviewer crews).
- **AutoGen** — Microsoft's multi-agent-conversation framework.
- **Burr** — explicit state machines (DAGsHub).
- **Parlant** — rule-based conversational agent control (better safety guarantees).
- **Camunda 8** — enterprise BPMN process engine with new agentic features (the original tldr-link source).
- **n8n / Zapier / Make** — low-code automation that's adding agent nodes; different audience.
- **DSPy** — pushes prompts and weights into a compiler step; less orchestrator, more *generator*.

## Internal links

- [[README]] — agents section landing.
- [[claude]] — Anthropic's *Building effective agents* (the workflow-patterns vocabulary).
- [[memory]] — agent-memory side; the state inside the orchestrator.
- [[task]] — task-framing side.
- [[../frameworks/langgraph|langgraph]] (P5.AB) — the canonical LangGraph article.
- [[../frameworks/dspy|dspy]] (P5.AB) — the prompt-and-weight compiler complement.
- [[../frameworks/phidata|phidata]] (P5.AB) — agent + RAG framework with playground/observability.
- [[openai]] — OpenAI Agents SDK (the orchestration pattern OpenAI ships).
- [[../mcp/README|mcp]] — Model Context Protocol; the tool-call wire format orchestrators speak.

## Keywords

`#orchestration` `#langgraph` `#crewai` `#autogen` `#burr` `#parlant` `#agentic-workflow`

## References / raw notes

- LangGraph docs: <https://langchain-ai.github.io/langgraph/>
- LangGraph workflow tutorials (the parallelisation snippet below): <https://langchain-ai.github.io/langgraph/tutorials/workflows/>
- CrewAI: <https://www.crewai.com/>
- AutoGen: <https://microsoft.github.io/autogen/>
- Burr: <https://burr.dagworks.io/>
- Parlant: <https://parlant.io/>
- Camunda agentic process orchestration: <https://camunda.com/blog/2024/04/agentic-process-orchestration/>
- Original tldr-source (Camunda whitepaper): <https://page.camunda.com/wp-why-agentic-process-orchestration-belongs-in-your-automation-strategy>

### LangGraph parallelisation worked example

This is the canonical *parallelisation* workflow: three LLM calls fan out from `START`, each writes a different field of shared state, and an `aggregator` node merges them.

```python
from typing import TypedDict
from langgraph.graph import StateGraph, START, END

# Graph state
class State(TypedDict):
    topic: str
    joke: str
    story: str
    poem: str
    combined_output: str

# Nodes
def call_llm_1(state: State):
    """First LLM call to generate initial joke."""
    msg = llm.invoke(f"Write a joke about {state['topic']}")
    return {"joke": msg.content}

def call_llm_2(state: State):
    """Second LLM call to generate story."""
    msg = llm.invoke(f"Write a story about {state['topic']}")
    return {"story": msg.content}

def call_llm_3(state: State):
    """Third LLM call to generate poem."""
    msg = llm.invoke(f"Write a poem about {state['topic']}")
    return {"poem": msg.content}

def aggregator(state: State):
    """Combine the joke, story, and poem into a single output."""
    combined = f"Here's a story, joke, and poem about {state['topic']}!\n\n"
    combined += f"STORY:\n{state['story']}\n\n"
    combined += f"JOKE:\n{state['joke']}\n\n"
    combined += f"POEM:\n{state['poem']}"
    return {"combined_output": combined}

# Build workflow
parallel_builder = StateGraph(State)

# Add nodes
parallel_builder.add_node("call_llm_1", call_llm_1)
parallel_builder.add_node("call_llm_2", call_llm_2)
parallel_builder.add_node("call_llm_3", call_llm_3)
parallel_builder.add_node("aggregator", aggregator)

# Add edges to connect nodes
parallel_builder.add_edge(START, "call_llm_1")
parallel_builder.add_edge(START, "call_llm_2")
parallel_builder.add_edge(START, "call_llm_3")
parallel_builder.add_edge("call_llm_1", "aggregator")
parallel_builder.add_edge("call_llm_2", "aggregator")
parallel_builder.add_edge("call_llm_3", "aggregator")
parallel_builder.add_edge("aggregator", END)
parallel_workflow = parallel_builder.compile()
```

Visualise the graph (Jupyter):

```python
from IPython.display import Image, display
display(Image(parallel_workflow.get_graph().draw_mermaid_png()))
```

Invoke:

```python
state = parallel_workflow.invoke({"topic": "cats"})
print(state["combined_output"])
```

LangSmith trace example: <https://smith.langchain.com/public/3be2e53c-ca94-40dd-934f-82ff87fac277/r>

### Why "graph" rather than "chain"

> With parallelisation, LLMs work simultaneously on a task and have their outputs aggregated programmatically. This workflow manifests in two key variations: **sectioning** (breaking a task into independent subtasks run in parallel) and **voting** (running the same task multiple times for diverse outputs). Use it when divided subtasks can be parallelised for speed, or when multiple perspectives or attempts are needed for higher confidence.
