# dev_tips

> Personal knowledge vault: notes, tips, tricks, and short investigations across programming, databases, machine learning, infrastructure, and tools.
>
> Built as an Obsidian vault and published as a mdBook site.

This is the canonical landing page for the `doc/` tree. Every sub-directory has its own `README.md` describing that area, linking to its articles, and connecting to neighbouring topics.

## Top-level sections

### Code & engineering
- [Programming](programming/README.md) — languages, language ecosystems, runtimes, build & packaging.
- [Databases](db/README.md) — relational, time-series, NoSQL, vector, analytics, streaming, distributed, catalogs, formats.
- [Tools](tools/README.md) — developer tooling: shell, OS, security, editors, design, networking, demos.
- [Visualization](visualization/README.md) — diagrams, charts, PDFs, Latex, web-templating, fonts.

### Data & AI
- [Machine Learning](ml/README.md) — foundations, frameworks, compilers, time-series, knowledge graphs, genetics, quantum.
- [LLMs](ml/llm/README.md) — models, runtimes, tokenizers, inspection, system prompts, papers.
- [RAG](ml/rag/README.md) — retrieval-augmented generation: vanilla, graph-RAG, KAG, agentic.
- [Agents](ml/agents/README.md) — agent frameworks, autonomous coders, multi-agent orchestration.
- [MCP](ml/mcp/README.md) — Model Context Protocol notes.

### Infrastructure
- [Cloud](cloud/README.md) — cloud platforms and free tiers.
- [Kubernetes](kubernetes/README.md) — container orchestration, K3s, AKRI, Balena.
- [IoT](iot/README.md) — MQTT, Drogue, Shodan, edge devices.
- [Messaging](messaging/README.md) — Kafka, Pulsar, Kinesis, NATS, Redpanda.
- [Observability](observability/README.md) — Prometheus, Grafana, OpenObserve, exporters.
- [DPU](dpu/README.md) — data processing units, Xilinx, XPU.

### Domains
- [Trading](trading/README.md) — algo trading, MQL5, Hummingbot, Rust trading bots.
- [Robotics](robot/README.md) — robotics tools and platforms.
- [Digital Twin](digital_twin/README.md) — simulation, NVIDIA Omniverse, BMW.
- [Games](games/README.md) — engines, Core Wars, retro.
- [Web](www/README.md) — web platform, SSL, payments, signing, SSG.

### Meta
- [Git](git/README.md) — workflow, configuration, delta.
- [Live Demos](tools/live_demos/README.md) — recorded shell sessions.
- [Funny](funny/README.md) — esoteric languages and joke programs.
- [Published](published/README.md) — papers and slides shipped externally.

## Conventions

- Every directory has exactly one `README.md` as its landing page.
- Every article carries YAML frontmatter (`title`, `main_link`, `keywords`, `status`).
- Internal cross-links use Obsidian wikilinks `[[…]]`; `SUMMARY.md` keeps standard `[text](path)` for mdBook compatibility.
- File names: `lower_snake_case.md`, no spaces, no mixed case (except literal `README.md`).
- See `plan.md` (repo root) for the full reorganization history and the article/landing templates.

## Building the mdBook site

```shell
mdbook serve --open
```

mdBook is a command-line tool to create books with Markdown. Output goes to `book/`. PDF rendering can be enabled in `book.toml`.

The site supports:

- MathJax (inline `\( … \)` and display `\[ … \]`)
- Mermaid diagrams (use ```` ```mermaid ```` fenced blocks)
- Admonish callouts (warning / bug / note)
- Editable runnable Rust snippets

For full mdBook capabilities see <https://rust-lang.github.io/mdBook/>.

## Investigation procedure

When I add a new tool or technology to this vault I follow a fixed loop:

1. **Investigate** — 5 minutes copying source material into a new file.
2. **Summarise** — 5 minutes writing summary + insight + keywords following the article template.
3. **Cross-link** — add Obsidian wikilinks to related articles already in the vault.
4. **Close the tab** — the whole point of the vault is to *empty the browser*.

For tools specifically: install instructions, plugin/extension setup, and a tip section live alongside.

## See also

- `plan.md` (root) — the master reorganization plan and live progress tracker.
- `inventory/` (root) — machine-readable audit data feeding the plan.
- `published/` — articles previously published on yarenty.blogspot.com.
