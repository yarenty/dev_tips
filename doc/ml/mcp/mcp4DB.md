---
title: MCP for databases — Toolbox, postgres-mcp, and friends
main_link: https://github.com/googleapis/genai-toolbox
keywords: [mcp, databases, postgres, sqlite, security, prompt-injection, tools]
status: reviewed
---

# MCP for databases — Toolbox, postgres-mcp, and friends

**Main link:** <https://github.com/googleapis/genai-toolbox>

## Summary

The **canonical use case** for MCP: give an LLM read (and sometimes write) access to a
database without writing a custom integration per model and per DB. The headline project
is Google's **MCP Toolbox for Databases** (formerly *GenAI Toolbox*) — an open-source
MCP server that wraps connection pooling, authn/authz, and config-driven tool definitions
across Postgres / MySQL / SQL Server / AlloyDB / Spanner / BigQuery / SQLite / Neo4j.
Many single-DB peers exist (`postgres-mcp`, `sqlite-mcp`, `mongodb-mcp`, snowflake's
`mcp-server-snowflake`, etc.) and end-user products (DataLine, AskYourDatabase, Hex's
Magic, Vanna.AI) often expose the same shape.

## Insight

**When to reach for it:** you want a chat-with-your-data UX without building a bespoke
RAG-over-schema pipeline; tools-over-SQL gives the LLM real, fresh data with no embedding
drift. Pair with a **read-only role**, **row-level security**, and a **statement timeout**
on day one.

**Security is the hard part, not the protocol.** Once the LLM has DB credentials,
*prompt injection becomes SQL injection*: a malicious row containing `"; DROP TABLE..."`
inside a column the LLM is asked to summarise can flow back as a tool call. Standard
mitigations:

- Read-only DB user (separate from your app user).
- Allow-list specific tools (`get_employee_by_id`) over generic `execute_sql`.
- Prepared-statement / parameter-only tool signatures (Toolbox's config style).
- Statement timeouts + result-row caps.
- Audit-log every executed query.
- Treat the schema itself as untrusted input if it includes user-controlled identifiers.

**Toolbox vs single-DB MCP servers:** Toolbox shines when you need multi-DB / Google
Cloud auth / declarative tool definitions in YAML; the single-DB OSS servers (e.g.
[`crystaldba/postgres-mcp`](https://github.com/crystaldba/postgres-mcp)) are simpler if
you only have one Postgres and want generic SQL access. The image referenced in the
original notes (the Toolbox architecture diagram) lives in the upstream repo.

## Similar / related topics

- **`postgres-mcp`** ([crystaldba](https://github.com/crystaldba/postgres-mcp), [henkdz](https://github.com/HenkDz/postgresql-mcp-server)) — single-DB Postgres servers.
- **Reference servers** ([`modelcontextprotocol/servers`](https://github.com/modelcontextprotocol/servers)) — official sqlite/postgres examples.
- **`mongodb-mcp`** ([mongodb-js/mongodb-mcp-server](https://github.com/mongodb-js/mongodb-mcp-server)) — official MongoDB MCP.
- **Snowflake** — official `mcp-server-snowflake`; also Cortex Search / Cortex Analyst as native LLM-on-DB.
- **DataLine, AskYourDatabase, Vanna.AI** — end-user products in the same lane.
- **MindsDB** — older "ML-as-virtual-tables" angle; pre-MCP.

## Internal links
<!-- reviewed -->
- [[README]] — section landing.
- [[articles]] — broader MCP reading list.
- [[a2a]] — sibling protocol article.
- [[mcp_ui]] — MCP for interactive UI.
- [[../../db/README|db]] — relational/nosql/vector overview.
- [[../../db/relational/postgresql|postgresql]] — the canonical server target.
- [[../data/qdrant_vector_search|qdrant for ML]] — vector-DB sibling pattern.
- [[../frameworks/mindsdb|mindsdb]] — pre-MCP "ML as virtual tables".

## Keywords

`#mcp` `#databases` `#postgres` `#sqlite` `#security` `#prompt-injection` `#tools`

## References / raw notes

### Headline: Google MCP Toolbox for Databases

- Repo: <https://github.com/googleapis/genai-toolbox> (renamed in 2025: `genai-toolbox` → MCP Toolbox for Databases).
- Pitch: open-source MCP server for databases; handles connection pooling, authn/authz, declarative YAML tool definitions.
- Supported sources: Postgres, MySQL, SQL Server, AlloyDB, Spanner, BigQuery, SQLite, Neo4j (and more on the [Sources docs](https://googleapis.github.io/genai-toolbox/resources/sources/)).
- Architecture diagram: <https://github.com/googleapis/genai-toolbox/raw/main/docs/en/getting-started/introduction/architecture.png>

### Other DB MCP servers worth knowing

- `postgres-mcp` (crystaldba): <https://github.com/crystaldba/postgres-mcp>
- `mongodb-mcp-server`: <https://github.com/mongodb-js/mongodb-mcp-server>
- Snowflake: <https://github.com/Snowflake-Labs/mcp>
- SQLite reference: <https://github.com/modelcontextprotocol/servers/tree/main/src/sqlite>
- Awesome list: <https://github.com/punkpeye/awesome-mcp-servers#-databases>

### End-user products / adjacent

- DataLine (`ramiawar/dataline`) — chat-with-your-DB, BYO key.
- AskYourDatabase — hosted SaaS.
- Vanna.AI — text-to-SQL with RAG over schema; can expose as MCP.
