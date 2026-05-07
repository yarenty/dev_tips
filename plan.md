# dev_tips — next steps

## Current vault snapshot (2026/05/03)

- **577 articles** across `doc/`, every one hand-reviewed against the standard template (Summary / Insight / Similar / Internal links / Keywords / References).
- **131 directories**, every one with a substantive `README.md` landing page.
- **0 orphans**, **0 dead-stub fossils**, **0 auto-stub residue**, **0 splitter-killer files** (every code block is properly fenced).
- **mdBook build is clean** (only the persistent benign mermaid-version-skew + large-search-index warnings).

New articles follow the conventions in the *Conventions cheat-sheet* at the bottom of this file.

---

## Open research threads 


### 1. Agent-to-Agent (A2A) protocol

- **Where it stands today** is captured in `doc/ml/mcp/a2a.md`: Google's Agent2Agent (Apr 2025), Linux Foundation transition (Jun 2025), Agent Card / Task / artifact / parts primitives, "MCP has more momentum" framing.
- **What to watch**: A2A specification version updates; concrete adopter stories beyond the announcement-tier; how A2A composes with MCP (the LLM-to-tool layer) in real production stacks; whether the "agent registry" idea around Agent Cards actually takes off; reference implementations beyond Google's Python SDK.
- **Watch list**: <https://github.com/google-a2a/A2A>, the LF AI & Data foundation announcement feed, IBM watsonx Orchestrate's A2A coverage, ServiceNow's pivot.

### 2. Orchestration & multi-agent collaboration

- **Current coverage**: `doc/ml/agents/orchestration.md` (LangGraph + the agent-control-flow landscape), `doc/ml/orchestration.md` (the data-pipeline side: Airflow / Prefect / Dagster / Argo / Flyte), `doc/ml/frameworks/langgraph.md` (the canonical Python orchestration framework).
- **What to watch**:
  - **CrewAI vs AutoGen vs LangGraph vs Burr vs Pydantic AI** — the picker matrix is still moving. Newer entrants like Mastra (Apr 2025), Letta agents, Smolagents code-agents pattern.
  - **Multi-agent debate / juries / committees** as a pattern (Microsoft AutoGen v0.4+, OpenAI Swarm / Agents SDK, the "judge" pattern from DSPy).
  - **Agent2Agent + MCP composition** — can agents from different vendors collaborate via A2A while consuming tools via MCP?
  - **Workflow vs agent distinction** — Anthropic's *Building effective agents* (already canonical reference in `doc/ml/agents/claude.md`) frames this; watch how the field converges or diverges from this taxonomy.

### 3. Agent platforms & coding-agent UX

- **Current coverage**: `doc/ml/agents/coding_agents.md` (broad landscape), `doc/ml/agents/devin.md` (Devin / OpenHands), `doc/ml/agents/openai.md` (Agents SDK), `doc/ml/llm/runtimes/goose.md` (Block's open agent CLI), `doc/ml/tools/cursor.md` (Cursor IDE), `doc/programming/rust/tooling/tauri.md` (the dev-tooling angle), and many more.
- **What to watch (per the user's list)**:
  - **Cursor** — pricing changes, Composer evolution, Tab model improvements, Composer-in-CI patterns.
  - **Gemini CLI** — Google's CLI agent (released late 2024 / early 2025); how does it compare to Claude Code / Codex CLI / Aider?
  - **Gemma CLI** — local-Gemma agent CLI variant, if/when Google releases one.
  - **Zed IDE** — the AI features (Edit Predictions, Assistant, Agent Edit) and their evolution; the Rust-native angle is interesting because the core IDE is itself Rust.
  - **Antigravity** — Google's Nov 2024 IDE/agent platform announcement (replaced Project IDX); needs a fresh look once it stabilises beyond Preview.
- **What's missing today**: a clean comparison-of-state-of-the-art coding-agents article that gets refreshed quarterly. The current `coding_agents.md` was written Q4 2025; it'll need a rewrite by mid-2026.

### 4. OpenClaw and the "open foundational agent" space

- **What it is** (per the user's note): OpenClaw appears to be an emerging open-source coding/agent framework. As of the last update, it doesn't have an article in this vault — needs investigation.
- **Investigation prompts**:
  - Is OpenClaw the Ant Group / Alibaba project? (There's an OpenClaw-related project from 2024-2025 in the Chinese open-source ecosystem.)
  - Or is this a different thing? (User memory may be slightly off on the name — could be OpenCSG, OpenManus, or something else from the same space.)
  - Where does it fit vs OpenHands (formerly OpenDevin), OpenManus, OpenAgent, AutoGen Studio, MetaGPT, AgentVerse, AutoAgents, GPT-Pilot?
- **Watch list once identified**: GitHub repo, the parent organisation's other projects, Twitter/Bluesky discussion threads.

### 5. MCP / tooling — latest advances and quality registries

- **Current coverage**: `doc/ml/mcp/` covers MCP fundamentals, A2A, MCP-for-databases (with the prompt-injection-becomes-SQL-injection security section), MCP-UI.
- **What to watch**:
  - **Spec evolution** — MCP spec versions, transport changes (stdio → HTTP+SSE → Streamable HTTP), authentication story, multi-tenant patterns.
  - **Registry / curation** — the question "where do I find a *trustworthy* MCP server for X?" is still poorly answered. Watch:
    - <https://github.com/modelcontextprotocol/servers> — official reference servers.
    - <https://github.com/punkpeye/awesome-mcp-servers> — community awesome list (large but quality is uneven).
    - <https://github.com/wong2/awesome-mcp-servers> — alternative awesome list.
    - <https://mcp.so/> — registry-style index.
    - <https://smithery.ai/> — registry + install tool.
    - Anthropic's own curation (the Claude Desktop "Connectors" gallery, when/if it opens).
    - Vendor-specific quality marks (Cloudflare's MCP server gallery, Block's Goose extensions registry).
  - **Security** — supply-chain risk for MCP servers is real; the prompt-injection / data-exfiltration vectors documented in `mcp4db.md` apply broadly. Watch for:
    - MCP-level signing / attestation specs (Sigstore-for-MCP equivalents).
    - Sandbox patterns (running MCP servers in WASM or containerised isolation).
    - Audit logs / observability for tool calls.
  - **Quality patterns**:
    - Which MCP servers are maintained by the *vendor* (Stripe, Linear, GitHub) vs community-only.
    - Test coverage and `mcp-eval` style testing.
    - Backwards-compat policy.

### 6. AI / ML / programming / Rust — general "where to look next"

#### AI/ML general

- **Models to watch**:
  - Llama 5 (rumoured Q2-Q3 2026), Llama 4.x updates.
  - DeepSeek R2 / V4 (the GRPO + reasoning-distillation recipe is now the canonical open-source approach).
  - Qwen3 series (Alibaba's response is a serious contender).
  - Mistral Large 3 / Codestral 2.
  - Anthropic Claude 4 (when it lands).
  - OpenAI o4 / GPT-5 family.
  - Google Gemini 2.5 / 3 series (Pro / Flash / Nano tiers).
- **Reading sources**:
  - **Andrej Karpathy** (X, GitHub, YouTube) — for the architectural mental models.
  - **Lilian Weng's blog** (lilianweng.github.io) — for the structured surveys.
  - **Sebastian Raschka's substack** (magazine.sebastianraschka.com) — for the practical paper-walkthroughs.
  - **Simon Willison's weblog** (simonwillison.net) — for the "what just happened in AI today" digest.
  - **The Pragmatic Engineer** (newsletter.pragmaticengineer.com) — for the engineering-organisation lens on AI tooling.
  - **Eugene Yan** (eugeneyan.com) — for production ML patterns.
- **Conferences worth following**: NeurIPS, ICML, ICLR, COLM, EMNLP, ACL — but also the AI Engineer Summit (Latent Space's conference), which has more practitioner content.

#### Rust general

- **2026 trends to watch**:
  - **Async story closing out** — async traits stabilised in 1.75 (2023); now the question is `dyn AsyncTrait` and async closures landing.
  - **`gccrs` and `rustc_codegen_gcc`** — the alternative Rust frontends; matters for embedded and for non-LLVM targets.
  - **Cranelift backend** for debug builds — Cranelift is now default for `cargo build` debug profile in nightly; faster compile times.
  - **`cargo-script`** stabilising — single-file Rust scripts.
  - **Edition 2024** rolling out — what changed, what broke.
  - **`unsafe` extern blocks** — the path to safer FFI.
  - **Generic-const-exprs** — slow march toward stabilisation.
- **Reading sources**:
  - **This Week in Rust** (this-week-in-rust.org) — weekly digest, the canonical "what's new in Rust" feed.
  - **Without.boats** (without.boats) — for the async/Future internals deep-dives.
  - **Niko Matsakis** (smallcultfollowing.com) — for the language-design rationale.
  - **Jon Gjengset** (X, YouTube "Crust of Rust") — for advanced patterns.
  - **fasterthanli.me** — for ergonomics-focused walkthroughs.
- **Vault-internal**: `doc/programming/rust/learning/_must_have.md`, `doc/programming/rust/learning/_to_learn.md` are the curated reading lists; refresh them when the canonical book set changes.

#### Programming general

- **AI-augmented dev workflow** — the watch list is itself the topic. Cursor + Claude Code + Aider + Codex CLI + Continue + Goose are all evolving fast; the "best workflow" question doesn't have a stable answer.
- **Build system trends** — Bazel, Buck2, Pants 2 (the Twitter-built monorepo build system); Nx and Turborepo for JS/TS monorepos; Mise for tool management.
- **Observability** — OpenTelemetry continues to consolidate; the metrics-vs-logs-vs-traces unification is real but slow.

### 7. The "rust + ML" intersection

This is the niche where the vault has unique coverage (`doc/programming/rust/ml/`, `doc/ml/tools/{burn,candle,tch,dfdx,tract}`, `doc/ml/compilers/cuda/`).

- **Watch**: the slow-but-real progress of Rust-native ML stacks (Burn 0.20+ trajectory, Candle's catch-up to PyTorch ergonomics, the tch-rs maintenance story now that tch has a single primary maintainer).
- **2025-2026 inflection point**: whether Burn lands LibTorch-tier ergonomics with the WGPU backend, which would make pure-Rust DL on Apple Silicon and laptops genuinely competitive.
- **`mistralrs`** is worth watching — Eric Buehler's pure-Rust LLM inference library is shaping up to be the Rust answer to vLLM/llama.cpp.

---

## Operational notes for future updates

The conventions baked into the vault are:

### Per-article template

```yaml
---
title: <Human-readable title>
main_link: <single canonical URL>
keywords: [kw1, kw2, kw3]   # 3-10 short tags
status: reviewed            # bump to reviewed when done
review_date: YYYY/MM/DD     # date of the most recent review
---

# <Title>

**Main link:** <URL>

## Summary
2-5 sentences. What is it? Who made it? What does it do?

## Insight
Why care? When/where to reach for this? Gotchas, opinions, comparisons.

## Similar / related topics
- name — 1-line description

## Internal links
- [[other_article]]

## Keywords
`#kw1` `#kw2`

## References / raw notes
<original links / snippets / curated content>
```

### Filename / wikilink conventions

- `lower_snake_case.md`, no spaces, no mixed case (except `README.md` / `SUMMARY.md`).
- Internal cross-links use Obsidian `[[wikilinks]]`. If the stem is unique vault-wide, bare `[[stem]]`. If ambiguous (e.g. `papers` exists in `ml/time_series/`, `ml/knowledge_graph/`, etc.), use `[[path/stem|alias]]`.
- mdBook ToC (`SUMMARY.md`) uses standard `[text](path)` Markdown links.

### When add a new article

1. Drop it under the correct topic directory.
2. Fill in the frontmatter, including `status: reviewed` and today's `review_date`.
3. Add it to `SUMMARY.md` (under the right `##` section).
4. Add it to the parent directory's `README.md`.
5. Run `mdbook build` from the repo root to verify no broken links.

### When delete or rename an article

1. `grep -rln '\[\[old_stem\]\]\|\[\[old_stem|' doc/` to find references.
2. `sed -i '' 's#\[\[old_stem\]\]#\[\[new_stem\]\]#g; s#\[\[old_stem|#\[\[new_stem|#g'` to patch them.
3. Update `SUMMARY.md` and the parent README.
4. `mdbook build` to verify.

### Snapshot stats

Re-audit:

```sh
find doc -name '*.md' | wc -l                     # total articles
grep -rln '^status: draft' doc/ | wc -l           # should be 0
grep -rln '^status: reviewed' doc/ | wc -l        # should be ~575
grep -rln '^review_date:' doc/ | wc -l            # should be ~575
mdbook build                                      # should be warning-clean
```

---
