---
title: A2A — Google's Agent2Agent protocol
main_link: https://github.com/google-a2a/A2A
keywords: [a2a, agents, google, protocol, mcp, interoperability]
status: reviewed
---

# A2A — Google's Agent2Agent protocol

**Main link:** <https://github.com/google-a2a/A2A>

## Summary

**A2A (Agent2Agent)** is an open protocol announced by Google (Apr 2025, [blog](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/))
for **agent-to-agent** communication: a "client agent" formulates a task and a "remote
agent" executes it, with capability discovery via a JSON **Agent Card**, a typed **Task**
object with a lifecycle, **artifact** outputs, and **parts** (typed content fragments) for
UX negotiation. Donated to the Linux Foundation in Jun 2025. Often paired with — not
replacing — MCP: **MCP is agent-to-tool, A2A is agent-to-agent**.

## Insight

The mental model that makes A2A click: **MCP exposes resources/tools to one agent;
A2A is how two agents (potentially from different vendors, in different processes, on
different clouds) negotiate work between themselves.** Discovery is via a static
`/.well-known/agent.json` Agent Card; the actual transport is HTTP+JSON-RPC (with SSE
for streaming and webhooks for long-running tasks).

Honest 2025 framing: A2A is **still very early**. MCP has dramatically more momentum
(Claude Desktop, Cursor, Goose, Continue, Zed, Windsurf all support it; A2A adoption is
mostly Google-orbit demos). The Confluent post argues A2A's task-handoff pattern wants
a durable broker (Kafka) under it — fair point, also true of any production agent fleet.
Watch for: convergence with MCP under the LF umbrella; whether agent frameworks
(LangGraph, CrewAI, AutoGen) ship first-class A2A clients; whether Anthropic/OpenAI ever
publish A2A-shaped Agent Cards for their hosted agents.

## Similar / related topics

- **MCP** — sibling protocol; tool/resource exposure rather than peer-to-peer.
- **ACP** (Agent Communication Protocol, IBM Research) — older OSS attempt, smaller traction.
- **OpenAI Assistants API / Responses API** — proprietary single-vendor alternative.
- **Anthropic computer-use** — another way to "let an agent drive another system" (UI-level).
- **FIPA-ACL** (1990s multi-agent systems) — historical precursor; A2A is the modern HTTP-native take.

## Internal links
<!-- reviewed -->
- [[README]] — section landing.
- [[articles]] — broader MCP reading list (incl. A2A vs MCP vs ACP cheat sheet).
- [[mcp4db]] — sibling MCP-side article.
- [[../agents/README|agents]] — agent landscape this protocol slots into.
- [[../llm/runtimes/goose|goose]] — MCP-native agent CLI; A2A is what would let two Gooses talk.
- [[../frameworks/langgraph|langgraph]] — likely first-class A2A integration target.
- [[kafka]] — the Confluent argument for a durable broker under A2A task handoffs.

## Keywords

`#a2a` `#agents` `#google` `#protocol` `#mcp` `#interoperability` `#kafka`

## References / raw notes

- Announcement (Apr 2025): <https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/?utm_source=tldrdata>
- Spec & SDKs: <https://github.com/google-a2a/A2A> (was `google/A2A`; transferred to `google-a2a/` org).
- Linux Foundation transition (Jun 2025): A2A donated to LF AI & Data alongside related agent-protocol work.

### Core concepts (per the announcement)

- **Capability discovery** — agents advertise capabilities via an **Agent Card** (JSON), letting a client agent pick the best remote agent for a task.
- **Task management** — communication is task-oriented; the `Task` object has a lifecycle (immediate completion or long-running with status updates). Output = **artifact**.
- **Collaboration** — agents exchange messages with context, replies, artifacts, or user instructions.
- **UX negotiation** — each message contains **parts** (typed content fragments: text, image, web form, iframe, …); client and remote negotiate the supported subset.

### Kafka angle (Confluent)

- <https://www.confluent.io/blog/google-agent2agent-protocol-needs-kafka/?utm_source=tldrdata> — argues A2A's long-running tasks need a durable event log; Kafka is the obvious substrate.
