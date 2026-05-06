---
title: "Fauna — the serverless transactional DB (sunset 2025)"
main_link: https://fauna.com/
keywords: [fauna, serverless, distributed, calvin, fql, graphql, sunset]
status: reviewed
---

# Fauna — the serverless transactional DB (sunset 2025)

**Main link:** <https://fauna.com/>

Shutdown notice: <https://fauna.com/blog/sunsetting-saas> · Open-source successor: <https://github.com/fauna/fauna-core>

## Summary

Fauna was a serverless, multi-region, distributed transactional database that pitched itself as "the operational database that takes consistency seriously". Internally it implemented Daniel Abadi's Calvin protocol for deterministic transactions, exposing a JSON document data model with relational features (FK-like refs, joins, indexes, ACID multi-document transactions) and its own functional query language **FQL** (plus a GraphQL layer).

**As of April 2025, Fauna sunset its hosted SaaS.** The company announced the shutdown in late 2024; existing customers were given a migration window. Some of the core engine was open-sourced.

## Insight

Historical interest only as a managed product. If you're considering it today:

- **Don't start a new project on Fauna's hosted service.** It's gone.
- **The open-sourced core** is interesting if you want to study Calvin-style deterministic transaction protocols, but operating it yourself defeats the original "serverless, no ops" pitch.
- **What to use instead** depends on which Fauna feature pulled you in:
  - "Serverless multi-region with HTTP API" → CockroachDB Serverless, Neon, PlanetScale, Turso, Cloudflare D1.
  - "Document model + ACID transactions" → MongoDB Atlas (since 4.0), [[postgresql]] with `jsonb`, [[surrealdb]].
  - "GraphQL out of the box" → Hasura on Postgres, Supabase, Grafbase.
  - "FQL-style functional query language" → arguably [[edgedb]]/Gel's EdgeQL fills a similar niche.

The Fauna shutdown is a useful cautionary tale: when the data layer is a single-vendor SaaS with a proprietary query language and no easy export path, vendor risk is concentrated. Postgres-on-anything has fewer fireworks.

## References / raw notes

Marketing copy from the original Fauna site, archived for reference:

> The distributed serverless database. Fauna combines the flexibility of NoSQL with the relational querying capabilities and ACID consistency of SQL systems — with native GraphQL and delivered as a cloud API so you don't have to worry about operations.

Headline features as advertised:

- Document-relational data model: JSON documents with FKs, views, joins.
- User-defined functions (UDFs) written in FQL, callable from GraphQL via custom resolvers.
- Connectionless access via HTTPS, attribute-driven security down to the document.
- Drivers for major languages, plus direct GraphQL from the client.

## Similar / related topics

- [[postgresql]] — Postgres + a managed provider (Neon, Supabase, RDS) covers most "serverless" needs without the lock-in.
- [[edgedb]] — typed query language pitch, alive and well.
- [[surrealdb]] — multi-model alternative aiming at a similar "developer-friendly DB" niche.
- [CockroachDB](https://www.cockroachlabs.com/) — distributed SQL, also serverless tier.
- [PlanetScale](https://planetscale.com/) — Vitess-on-MySQL, serverless tier.

## Internal links

<!-- reviewed -->

- [[postgresql]]
- [[edgedb]]
- [[surrealdb]]

## Keywords

`#fauna` `#serverless` `#distributed-sql` `#calvin` `#fql` `#sunset` `#vendor-lockin` `#db`
