---
title: n8n — fair-source workflow automation with native AI nodes
main_link: https://n8n.io/
keywords: [n8n, workflow-automation, langchain, ai-agents, integromat, zapier, fair-source, self-hosted]
status: reviewed
---

# n8n — fair-source workflow automation with native AI nodes

**Main link:** <https://n8n.io/>

Repo: <https://github.com/n8n-io/n8n>

## Summary
n8n (pronounced "n-eight-n", short for "nodemation") is a fair-source workflow automation platform aimed at technical teams. It combines a visual node-based editor with the option to drop into JavaScript or Python, ships 400+ integrations and 900+ template workflows, has first-class **AI-agent nodes** (LangChain-based, with OpenAI / Anthropic / Ollama / vector-store / tool nodes), and is **self-hostable** under the [Sustainable Use License](https://docs.n8n.io/sustainable-use-license/) (fair-source — free for internal business use; restricted for hosted resale). Enterprise features (SSO, advanced permissions, air-gapped deploys) are available via a paid licence. This is the canonical home for n8n in this vault — `www/n8n_io.md` was deleted as a duplicate during P5.G.

## Insight
Reach for n8n when you need **Zapier-style integrations *and* the ability to self-host *and* native AI-agent steps in the same workflow** — the combination is genuinely rare. The 2023-2024 LangChain integration turned n8n from "yet another iPaaS" into a credible no-code/low-code AI-agent runtime: a workflow can chain a webhook → vector-store retrieval → LLM tool-calling agent → Slack post → DB write, with the LangChain nodes handling tool routing inside one step.

Vs the alternatives:
- **vs Zapier** — Zapier is closed, smoother UX, more polished integrations, but you can't self-host and AI features lag. Pick Zapier if you're a non-technical team that wants the path of least resistance.
- **vs Make.com (formerly Integromat)** — Make has a cleaner visual editor and good AI features, also closed-source. Pick Make if visual ergonomics matter and you don't need to self-host.
- **vs Apache Airflow** — Airflow is code-first, data-pipeline-shaped (DAGs, scheduling, backfills), Python-native. Different problem class — Airflow for ETL, n8n for SaaS-glue + AI.
- **vs Temporal** — code-first durable workflows for engineers; n8n is the no-code/low-code counterpart with much better SaaS-integration coverage but weaker durability semantics.
- **vs Pipedream** — closer competitor in the "code-when-you-need-it" niche; Pipedream is closed and serverless-first.
- **vs IFTTT** — consumer-grade; not in the same league for technical teams.

Honest caveats: the "fair-source" Sustainable Use License is **not OSI-approved open source** — internal business use is fine, but you can't build a hosted competitor on n8n. The execution model is single-process per workflow run (with queue mode for scale-out), so it's not ideal for very-high-throughput event processing — reach for Kafka/Flink/Temporal there. The visual editor is great until you have 50+ nodes, at which point version control of workflows becomes the bottleneck (the JSON-export-to-git workflow helps but is fiddly).

## Similar / related topics
- [Zapier](https://zapier.com/) — closed, polished, no self-host; the original.
- [Make.com](https://www.make.com/) — closed, visual; closer feature parity than Zapier.
- [Apache Airflow](https://airflow.apache.org/) — code-first DAGs for data pipelines.
- [Temporal](https://temporal.io/) — code-first durable workflows for engineers.
- [Pipedream](https://pipedream.com/) — closed, serverless-first, code-when-you-need-it neighbour.
- [[../frameworks/langgraph|langgraph]] — code-first stateful agent graphs; the LangChain ecosystem n8n's AI nodes share lineage with.

## Internal links
- [[../frameworks/langgraph|langgraph]]
- [[../frameworks/README|frameworks]]
- [[../agents/orchestration|agents/orchestration]]

## Keywords
`#n8n` `#workflow-automation` `#fair-source` `#self-hosted` `#langchain` `#ai-agents` `#zapier-alternative`

## References / raw notes

Secure, AI-native workflow automation. The world's most popular workflow automation platform for technical teams (per the marketing copy).

### Key capabilities

- **Code when you need it**: write JavaScript / Python, add npm packages, or use the visual interface.
- **AI-native platform**: build AI-agent workflows based on LangChain with your own data and models.
- **Full control**: self-host under the fair-code Sustainable Use License, or use the n8n Cloud offering.
- **Enterprise-ready**: advanced permissions, SSO, air-gapped deployments.
- **Active community**: 400+ integrations and 900+ ready-to-use templates.
