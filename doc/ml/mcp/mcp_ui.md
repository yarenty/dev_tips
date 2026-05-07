---
title: mcp-ui — interactive UI resources from MCP servers
main_link: https://github.com/idosal/mcp-ui
keywords: [mcp, ui, agent-ui, generative-ui, react, web-components]
status: reviewed
---

# mcp-ui — interactive UI resources from MCP servers

**Main link:** <https://github.com/idosal/mcp-ui>

## Summary

**mcp-ui** is a community-driven extension to MCP (by Ido Salomon, [`idosal`](https://github.com/idosal))
that lets an MCP server return **interactive web UI resources** — small HTML/React/web-component
fragments — instead of plain text or images. The host (the MCP client, e.g. Claude
Desktop / Cursor / a custom chat UI) renders them inline. The pitch: "raise Human↔AI
interaction beyond chat-only" — let an agent draw a form, a chart, a confirmation dialog,
a domain widget, and let the user interact with it directly.

## Insight

Where this fits in the broader **agent-generated UI** trend (V0, Bolt.new, Lovable,
Vercel AI SDK Generative UI, OpenAI Canvas, Claude Artifacts): mcp-ui is the *protocol*
side rather than the *frontend builder* side. **V0/Bolt/Lovable generate code you ship**;
**Vercel AI SDK and Artifacts render rich content from a single LLM session**; **mcp-ui
lets any third-party MCP server contribute interactive UI** to whichever host the user
already trusts.

Honest 2025 framing: **adoption is early**. The MCP spec itself doesn't standardise
interactive UI yet; mcp-ui is a *proposal-by-implementation*. Hosts must opt in to render
its `ui://` resource type, and most MCP clients still only handle text/image. Sandbox
story (iframe / web-component shadow DOM / CSP) and the prompt-injection attack surface
("malicious MCP server returns a UI that nags the user into clicking 'approve'") are open
questions. Watch for: official spec absorption; Cursor / Goose / Claude Desktop shipping
host-side support.

## Similar / related topics

- **A2A `parts`** — Google's A2A also has typed UI fragments (forms, iframes, video) for the same goal.
- **Vercel AI SDK Generative UI** — same idea, in-process rather than over MCP.
- **Anthropic Claude Artifacts / OpenAI Canvas** — single-vendor inline interactive panes.
- **V0 / Bolt.new / Lovable** — agent-generates-frontend-code (different stage of the pipeline).
- **MCP elicitation** (in-spec) — the lighter-weight "ask the user a structured question" primitive in the MCP spec itself.

## Internal links
<!-- reviewed -->
- [[README]] — section landing.
- [[articles]] — broader MCP reading list.
- [[a2a]] — sibling protocol; A2A's `parts` overlap conceptually.
- [[mcp4db]] — the data-access counterpart.
- [[../llm/runtimes/goose|goose]] — host that may ship mcp-ui support.

## Keywords

`#mcp` `#ui` `#agent-ui` `#generative-ui` `#react` `#web-components`

## References / raw notes

- Repo: <https://github.com/idosal/mcp-ui>
- Pitch (verbatim): "mcp-ui brings interactive web components to Model Context Protocol (MCP). Deliver rich, dynamic UI resources directly from your MCP server to be rendered by the client. Take Human↔AI interaction to the next level."
- Resource type: `ui://...` URIs returned from MCP server responses; the client interprets the payload as renderable HTML/React/web-component content.
- Adjacent ideas in the official spec to watch: [elicitation](https://modelcontextprotocol.io/specification) (structured user-input prompts), sampling, and roots.
