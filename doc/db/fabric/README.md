---
title: Data fabric
keywords: [data-fabric, governance, integration, microsoft-fabric, talend, informatica, atlan, lakehouse, data-mesh]
status: reviewed
review_date: 2026/05/03
---

# Data fabric

"Data fabric" is a vendor-coined umbrella term — primarily pushed by
Microsoft (Microsoft Fabric), Talend, Informatica, IBM, and the analyst-shop
ecosystem (Gartner) — that broadly means **"a unified layer covering metadata,
integration, governance, and access across all your data systems."** In
practice it overlaps so heavily with **data mesh** and **lakehouse** that the
boundaries are mostly a matter of which vendor's slide deck you're reading.

## Honest take

Be skeptical of the label, but don't dismiss the underlying problems. The
real, useful capabilities clustered under "data fabric" are:

- **A central catalog** with cross-system metadata, lineage, and search.
- **Integration / pipeline orchestration** that doesn't reinvent itself per
  source.
- **Unified access control and audit** across heterogeneous engines.
- **Self-service discovery** so analysts can find tables without asking on
  Slack.

Each of those is a real problem with real open-source and commercial
solutions — they just predate the "fabric" branding. If a vendor is selling
you a "data fabric", ask which of those four they actually solve, and how
much of it is rebadged catalog + ETL + IAM.

## Vendors and where they fit

- **Microsoft Fabric** — SaaS bundle around OneLake (Delta-flavoured),
  Synapse, Power BI; the most prominent "fabric" branded product.
- **Talend** / **Informatica** / **IBM Cloud Pak for Data** — legacy ETL
  vendors who repositioned existing suites under the term.
- **Atlan** / **Alation** / **Collibra** — catalog + governance products
  often called "fabric" in marketing, "catalog" by everyone else.
- **OpenMetadata** / **DataHub** — open-source catalog-shaped systems that
  cover most "fabric" capabilities without the branding.

## Adjacent terms

- **Data mesh** — *organisational* pattern (domain-owned data products) more
  than a *technology* layer. Data fabric is closer to "platform"; data mesh
  is closer to "team topology".
- **Lakehouse** — a *storage* pattern (table formats over object storage with
  ACID semantics). A fabric typically *includes* a lakehouse but is broader.
- **Data platform** — generic term that mostly means the same thing without
  the marketing.

## See also

- [[db/catalogs/README|catalogs]] — the closest-shaped subsection in this
  vault; most "fabric" capabilities are catalog-shaped.
- [[db/quality/README|quality]] — governance and quality usually ride on the
  same metadata layer.
- [[db/formats/README|formats]] — Iceberg/Delta tables are the storage layer
  most fabrics standardise on.
- [[db/distributed/README|distributed]] — the engines that fabrics broker
  access to.

## Keywords

`#data-fabric` `#governance` `#integration` `#microsoft-fabric` `#talend` `#informatica` `#atlan` `#lakehouse` `#data-mesh`
