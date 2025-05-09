# Orchestration

https://page.camunda.com/wp-why-agentic-process-orchestration-belongs-in-your-automation-strategy?utm_medium=paid_leadgen&utm_source=tldr&utm_content=may_newsletter&utm_campaign=Guide.WhyAgenticProcessOrchestrationBelongsInYourAutomationStrategy.25Q1.EN


## Langgraph


https://langchain-ai.github.io/langgraph/


LangGraph — used by Replit, Uber, LinkedIn, GitLab and more — is a low-level orchestration framework for building controllable agents. While langchain provides integrations and composable components to streamline LLM application development, the LangGraph library enables agent orchestration — offering customizable architectures, long-term memory, and human-in-the-loop to reliably handle complex tasks.

https://langchain-ai.github.io/langgraph/tutorials/workflows/


![](https://langchain-ai.github.io/langgraph/concepts/img/agent_workflow.png)



![](https://langchain-ai.github.io/langgraph/tutorials/workflows/img/augmented_llm.png)



## Good example why is it graph


### Parallelization¶
With parallelization, LLMs work simultaneously on a task:

LLMs can sometimes work simultaneously on a task and have their outputs aggregated programmatically. This workflow, parallelization, manifests in two key variations: Sectioning: Breaking a task into independent subtasks run in parallel. Voting: Running the same task multiple times to get diverse outputs.

When to use this workflow: Parallelization is effective when the divided subtasks can be parallelized for speed, or when multiple perspectives or attempts are needed for higher confidence results. For complex tasks with multiple considerations, LLMs generally perform better when each consideration is handled by a separate LLM call, allowing focused attention on each specific aspect.

![parallelization.png](https://langchain-ai.github.io/langgraph/tutorials/workflows/img/parallelization.png)


```python

# Graph state
class State(TypedDict):
topic: str
joke: str
story: str
poem: str
combined_output: str


# Nodes
def call_llm_1(state: State):
"""First LLM call to generate initial joke"""

    msg = llm.invoke(f"Write a joke about {state['topic']}")
    return {"joke": msg.content}


def call_llm_2(state: State):
"""Second LLM call to generate story"""

    msg = llm.invoke(f"Write a story about {state['topic']}")
    return {"story": msg.content}


def call_llm_3(state: State):
"""Third LLM call to generate poem"""

    msg = llm.invoke(f"Write a poem about {state['topic']}")
    return {"poem": msg.content}


def aggregator(state: State):
"""Combine the joke and story into a single output"""

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

#### Show workflow
display(Image(parallel_workflow.get_graph().draw_mermaid_png()))

##### Invoke
state = parallel_workflow.invoke({"topic": "cats"})
print(state["combined_output"])
LangSmith Trace

https://smith.langchain.com/public/3be2e53c-ca94-40dd-934f-82ff87fac277/r


