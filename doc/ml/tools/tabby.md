---
title: TabbyML — self-hosted open-source code-completion server
main_link: https://github.com/TabbyML/tabby
keywords: [tabby, tabbyml, copilot-alternative, self-hosted, code-completion, starcoder, deepseek-coder, qwen-coder, openapi]
status: reviewed
---

# TabbyML — self-hosted open-source code-completion server

**Main link:** <https://github.com/TabbyML/tabby>

## Summary
Tabby (TabbyML) is a self-hosted, Apache-2.0-licensed AI coding assistant — an open, on-premises alternative to GitHub Copilot. The Rust-based server is **self-contained** (no DBMS / no cloud dependency), exposes an **OpenAPI-compatible HTTP interface** for IDE integration, and runs your choice of open-weights coding model (StarCoder / StarCoder2, Code Llama, DeepSeek-Coder, Qwen2.5-Coder, CodeGemma, CodeQwen, …) on consumer-grade GPUs. IDE/editor extensions exist for VS Code, JetBrains, Vim/Neovim, IntelliJ, and others; team-wide analytics ("Reports tab" since v0.10.0, April 2024) and locally-relevant snippet retrieval (LSP declarations + recently-modified code) round out the offering.

## Insight
Reach for Tabby when **privacy, vendor independence, or air-gapped operation** are real constraints — regulated industries (finance, defence, healthcare), source code that can't legally leave the building, or simply teams that don't want to ship every keystroke to an external API. The "self-contained, OpenAPI-compatible" design choice is what makes Tabby easy to drop into existing infrastructure: spin up a Docker container on a GPU box, point your IDE plugin at it, done. The project also publishes an OpenAI-compatible chat endpoint for chat-style use beyond raw completion.

Vs the neighbours:
- **vs GitHub Copilot** — Copilot is closed, hosted, and the polished default; Tabby is the self-hosted answer when you can't or don't want to use it.
- **vs [[cursor]]** — Cursor is an IDE, not a server; orthogonal. Tabby could in principle back a Cursor-style frontend (and the Tabby team's "Tabby Chat" is moving in that direction).
- **vs [Continue.dev](https://www.continue.dev/)** — Continue is more flexible (BYO any model API) and lives entirely as IDE extensions; Tabby is the *server*-side counterpart that Continue can talk to. Many teams run both: Continue as the IDE plugin, Tabby as the on-prem inference server.
- **vs [Sourcegraph Cody](https://sourcegraph.com/cody)** — Cody combines code search + AI with strong codebase context (via Sourcegraph indexing); heavier, more enterprise-shaped.
- **vs [TabbyML's own competitors] [Refact](https://refact.ai/)** — Refact is the closest direct competitor on "self-hosted FIM completion server with open models"; pick on model-zoo support and IDE-extension polish.

The privacy story is the differentiator that doesn't bit-rot. Run-in-1-minute via Docker:

```sh
docker run -it \
  --gpus all -p 8080:8080 -v $HOME/.tabby:/data \
  tabbyml/tabby \
  serve --model TabbyML/StarCoder-1B --device cuda
```

Honest caveats: open coding models still trail the closed frontier on raw quality (Claude / GPT-5 are stronger at multi-file reasoning); GPU sizing matters (a 1B–7B model fits a single consumer GPU; bigger needs more); the Reports analytics dashboard is useful but the multi-team / RBAC story has historically lagged Copilot Business.

## Similar / related topics
- [Continue.dev](https://www.continue.dev/) — open IDE extension; pairs naturally with Tabby as the inference backend.
- [Sourcegraph Cody](https://sourcegraph.com/cody) — code-search-anchored AI assistant; stronger on multi-repo context.
- [Refact](https://refact.ai/) — direct self-hosted competitor.
- [[cursor]] — closed AI-IDE; opposite end of the privacy / convenience trade-off.
- [[../llm/runtimes/ollama|ollama]] — local LLM runtime; complementary if you want to run Tabby's chat backend off an Ollama endpoint.

## Internal links
- [[cursor]]
- [[../agents/coding_agents|coding_agents]]
- [[../llm/runtimes/ollama|ollama]]
- [[../llm/models/jetbrains_mellum|jetbrains_mellum]]

## Keywords
`#tabby` `#tabbyml` `#self-hosted` `#code-completion` `#openapi` `#copilot-alternative` `#privacy`

## References / raw notes

Tabby is a self-hosted AI coding assistant — an open-source, on-premises alternative to GitHub Copilot.

### Key features

- Self-contained: no external DBMS or cloud service required.
- OpenAPI interface; easy to integrate into existing infrastructure (e.g. cloud IDEs).
- Supports consumer-grade GPUs.
- Chat playground built in.

### Notable recent updates (excerpted from the upstream changelog)

- **v0.10.0 (2024-04-22)** — new Reports tab with team-wide Tabby usage analytics.
- **2024-04-19** — code completion now incorporates locally relevant snippets (declarations from local LSP and recently-modified code).
- **2024-04-17** — CodeGemma and CodeQwen model series added to the official registry.

### Getting started

Documentation: <https://tabby.tabbyml.com/docs/welcome/>

Run Tabby in 1 minute via Docker:

```sh
docker run -it \
  --gpus all -p 8080:8080 -v $HOME/.tabby:/data \
  tabbyml/tabby \
  serve --model TabbyML/StarCoder-1B --device cuda
```

For inference type, parallelism, and other options, refer to the documentation. IDE/editor extensions are available for the major editors.
