---
title: Agent2Agent
main_link: https://github.com/google/A2A
keywords: [a2a, kafka]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Agent2Agent

**Main link:** <https://github.com/google/A2A>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[task]] — Task _(score 26.9)_
- [[articles]] — Articles _(score 26.1)_
- [[co_scientist]] — Google _(score 19.5)_
- [[ml/bigquery/links|links]] — Links _(score 14.4)_
- [[agentic_ai_memory]] — Task _(score 14.4)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#a2a` `#mcp` `#ml` `#agent` `#agents` `#task` `#each`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Agent2Agent


https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/?utm_source=tldrdata


A2A facilitates communication between a "client" agent and a “remote” agent. A client agent is responsible for formulating and communicating tasks, while the remote agent is responsible for acting on those tasks in an attempt to provide the correct information or take the correct action. This interaction involves several key capabilities:

Capability discovery: Agents can advertise their capabilities using an “Agent Card” in JSON format, allowing the client agent to identify the best agent that can perform a task and leverage A2A to communicate with the remote agent.
Task management: The communication between a client and remote agent is oriented towards task completion, in which agents work to fulfill end-user requests. This “task” object is defined by the protocol and has a lifecycle. It can be completed immediately or, for long-running tasks, each of the agents can communicate to stay in sync with each other on the latest status of completing a task. The output of a task is known as an “artifact.”
Collaboration: Agents can send each other messages to communicate context, replies, artifacts, or user instructions.
User experience negotiation: Each message includes “parts,” which is a fully formed piece of content, like a generated image. Each part has a specified content type, allowing client and remote agents to negotiate the correct format needed and explicitly include negotiations of the user’s UI capabilities–e.g., iframes, video, web forms, and more.

https://github.com/google/A2A




https://github.com/google/A2A


## Kafka

https://www.confluent.io/blog/google-agent2agent-protocol-needs-kafka/?utm_source=tldrdata
