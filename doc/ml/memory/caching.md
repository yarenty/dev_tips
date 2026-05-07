---
title: Semantic caching for LLMs and AI agents
main_link: https://thenewstack.io/what-is-semantic-caching/
keywords: [caching, semantic-cache, prompt-cache, kv-cache, gptcache, langcache, llm]
status: reviewed
review_date: 2026/05/03
---

# Semantic caching for LLMs and AI agents

**Main link:** <https://thenewstack.io/what-is-semantic-caching/>

## Summary

Semantic caching is the LLM-era variant of HTTP/CDN caching: instead of matching requests by exact URL/key, you embed the prompt as a vector and serve a cached response if a previous prompt is *semantically close enough* (vector-similarity above some threshold). Used as a drop-in proxy in front of OpenAI/Anthropic/Gemini APIs, it reduces token cost and tail latency dramatically — Fastly's published benchmark shows ~9× faster server response and the canonical 2024 study reports up to ~68.8% reduction in API calls. Three layers in the cache stack are worth distinguishing: **semantic cache** (cache the *answer*; what this article is about), **prompt cache** (cache the *prefix tokenisation/KV*; an OpenAI/Anthropic 2024 feature), and **KV cache** (the model-internal one that paged-attention runtimes like vLLM manage).

## Insight

When to reach for it: **conversational interfaces in narrow domains** (customer service, retail, repeated FAQ-shaped agent queries) where users phrase the same intent dozens of different ways. The semantic-cache hit rate climbs as the *intent space narrows*. When **not** to reach for it: open-ended creative chat, reasoning-heavy tasks where small prompt differences should produce different outputs, anything safety-critical where a near-miss false positive could matter (the cache returns *someone else's* answer, with their context).

The three failure modes to watch:

1. **Threshold tuning is hard.** Too tight → low hit rate, no benefit. Too loose → semantically-similar-but-actually-different prompts return wrong answers. Most teams pick a threshold per domain experimentally.
2. **Privacy/multi-tenancy boundary.** A naive shared cache leaks information across users — *every* deployment needs partitioning by tenant/user/scope.
3. **Invalidation.** When the underlying knowledge changes (model update, RAG corpus update, business rule change), you need to invalidate the cache or it serves stale answers — same lesson the CDN industry learned 25 years ago.

The picker (2025 landscape):

- **GPTCache** (Zilliz) — the original OSS project; pluggable similarity backends (Faiss/Milvus/Redis).
- **Redis LangCache** — Redis's managed semantic cache, launched 2024, integrated with their vector type.
- **Fastly AI Accelerator** — edge-deployed semantic cache as a managed proxy (the article above).
- **LangChain `langchain.cache.RedisSemanticCache` / `MomentoCache`** — framework-integrated.
- **Anthropic prompt caching** (Aug 2024) and **OpenAI prompt caching** (Oct 2024) — *prompt* (prefix) caching not semantic; orthogonal mechanism, often used together. These don't need any infrastructure on your side.
- **vLLM PagedAttention** — KV-cache management for inference servers; orthogonal but lives in the same mental model.

Note: the filename `caching.md` is shared with `programming/rust/data/cache.md` (Moka / quick_cache — *in-process* caching crates) and `programming/rust/data/compilation_cache.md` (sccache for Rust builds). Cross-link with disambiguated stems.

## Similar / related topics

- **GPTCache** — Zilliz's OSS semantic cache; pluggable vector backend.
- **Redis LangCache** — Redis-managed semantic-cache product (2024).
- **Anthropic / OpenAI prompt caching** — provider-side prefix caching (orthogonal optimisation).
- **vLLM PagedAttention** — KV-cache management at the inference layer.
- **Fastly AI Accelerator** — managed semantic-cache proxy at the edge.

## Internal links

- [[agentic_ai_memory]] — sibling: agent-memory landscape (caching is the *adjacent* memory-shaped optimisation).
- [[response]] — sibling: 2024-2025 eidetic-memory survey.
- [[episodic_memory]] — sibling: episodic recall (different problem, different solution).
- [[202505_recents]] — sibling: May 2025 memory papers digest.
- [[../data/qdrant_vector_search|qdrant_vector_search]] — vector search underlies most semantic-cache backends.
- [[../rag/README|rag]] — RAG section landing (semantic cache often sits in front of a RAG pipeline).
- [[../llm/README|llm]] — LLM section landing.
- [[programming/rust/data/cache|rust/data/cache]] — *different* topic: Rust in-process caches (Moka / quick_cache).

## Keywords

`#caching` `#semantic-cache` `#prompt-cache` `#kv-cache` `#gptcache` `#langcache` `#llm`

## References / raw notes

- **Headline article**: Bill Doerrfeld, *What Is Semantic Caching?*, The New Stack, May 2025 — <https://thenewstack.io/what-is-semantic-caching/>. Interviews Randy Reddig (Fastly), Andre Zayarni (Qdrant), Archana Kannan (Salesforce/Slack). Source for the "9× faster" Fastly benchmark and the 68.8% study citation.

### Quoted observations from the source article

- Reddig (Fastly): *"Many requests are phrased slightly differently, but their responses should be relatively the same."* The cache value scales with how *narrow* the semantic space is — retail / customer service / online shopping are sweet spots.
- Reddig (Fastly): semantic caching is *not much different than how a CDN or gateway already works* — the secret sauce is the engine that determines semantic sameness.
- Zayarni (Qdrant): *"Agentic AI demands better context integration, faster feedback loops and scalable orchestration. AI agents hit bottlenecks when retrieval is slow or context updates lag behind real-time needs."*
- Kannan (Salesforce): *"The most critical advancements will be centered around improvements in data integration, metadata management and multiagent collaboration."*

### Adjacent / orthogonal mechanisms

- **Prompt caching** — Anthropic (Aug 2024) and OpenAI (Oct 2024) ship provider-side caching of prompt prefixes (system prompt + repeated context). Reduces token cost without any infra on your side. Different layer from semantic caching; often combined.
- **KV caching** — the model-internal key/value cache. vLLM PagedAttention is the canonical optimised implementation; SGLang / LMDeploy / TGI ship comparable mechanisms. Not user-facing but determines per-token cost.
- **Multi-agent semantic routing** — Kannan's argument: a dispatcher agent reviews queries, finds semantically-similar prior queries, and hands off deeper reasoning to specialised downstream agents. Combines well with semantic caching since the dispatcher is doing semantic matching anyway.

### Picker quick-look

| Tool | Type | Hosting | Best for |
|------|------|---------|----------|
| GPTCache | OSS lib | self-hosted | Greenfield projects, max flexibility |
| Redis LangCache | Managed | Redis Cloud | Already on Redis |
| Fastly AI Accelerator | Managed | Fastly edge | Add-a-proxy migration; global low latency |
| LangChain semantic cache | OSS integration | self-hosted | Already in LangChain |
| Anthropic / OpenAI prompt cache | Provider | provider-side | Free win on long system prompts |
| vLLM PagedAttention | OSS runtime | self-hosted | Self-hosted inference; KV-cache layer |

### Original framing notes (preserved)

> Caching layers between the client and LLM API will become the default for new AI development — like CDNs became the standard for optimising web performance.

> Lastly, there is an argument to be made that LLMs will become cheaper with future model advances and GPU efficiencies, decreasing the need for efficiency savings. But, as Moore's Law plateaus and inference costs climb, semantic caching is emerging as a helpful method to curtail redundancy.
