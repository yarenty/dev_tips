---
title: Machine Learning — vault hub
keywords: [ml, ai, llm, agents, rag, hub]
status: reviewed
review_date: 2026/05/03
---

# Machine Learning — vault hub

This is the largest section of the vault by a wide margin. It covers the **modern AI / ML / LLM stack** end-to-end, organised by *what you do* rather than by tool name.

For the **Rust-side** of the same story (linfa, candle, burn, tch, dfdx, cudarc), see [[../programming/rust/ml/README|programming/rust/ml]] (curated separately so the Rust ecosystem stays in one place).

## Where to start

| You want to… | Look at |
|---|---|
| Understand classical ML concepts (clustering, classification, GNNs, KAN) | [[fundamentals/README|fundamentals]] |
| Pick a Python ML/agent framework (DSPy, LangGraph, H2O AutoML) | [[frameworks/README|frameworks]] |
| Use ML compilers (XLA, MLGO, Tiramisu) or the CUDA hub | [[compilers/README|compilers]] |
| Train models on Google's data warehouse | [[bigquery/README|bigquery]] |
| Find pre-training data, public datasets, or vector-search-for-ML | [[data/README|data]] |
| Pick an LLM (Llama, DeepSeek, OLMo) or a runtime (Ollama, vLLM, llama.cpp) | [[llm/README|llm]] |
| Build agents (Devin, Claude Code, OpenAI Agents SDK, smolagents) | [[agents/README|agents]] |
| Expose tools to LLMs over the Model Context Protocol | [[mcp/README|mcp]] |
| Build a RAG system (chunking, embeddings, GraphRAG, agentic RAG) | [[rag/README|rag]] |
| Fine-tune (LoRA / QLoRA / DPO / GRPO via Unsloth or TRL) | [[finetuning/README|finetuning]] |
| Manage agent memory (Mem0, Letta, Zep, episodic/semantic) | [[memory/README|memory]] |
| Build voice AI (STT, TTS, Vapi/Retell/LiveKit) | [[voice/README|voice]] |
| Pick an end-user ML tool (Burn, Candle, Cursor, Tabby, n8n, LeRobot) | [[tools/README|tools]] |
| Forecast time series (statistical / ML / DL / foundation models) | [[time_series/README|time_series]] |
| Work with knowledge graphs (KG embeddings, GraphRAG, petgraph) | [[knowledge_graph/README|knowledge_graph]] |
| Run evolutionary / multi-objective optimisation | [[genetics/README|genetics]] |
| Read the quantum-ML landscape (PennyLane, Qiskit ML, hybrid) | [[quantum]] |
| Orchestrate ML/data pipelines (Airflow, Prefect, Dagster, Argo) | [[orchestration]] |
| Package agent skills (`.cursorrules`, `AGENTS.md`, Claude Skills) | [[skills/README|skills]] |

## Subsections

- [[agents/README|agents]] — autonomous LLM-driven systems (Devin / OpenHands, Claude as canonical reference, OpenAI Agents SDK, smolagents, FutureHouse, co-scientist, coding-agent landscape).
- [[bigquery/README|BigQuery & BigQuery ML]] — Google's warehouse-side `CREATE MODEL` SQL story.
- [[compilers/README|ML compilers]] — XLA / MLGO / Tiramisu / tiny-gpu, plus the substantive [[compilers/cuda/README|CUDA hub]].
- [[data/README|data]] — public datasets, dataset APIs, test fixtures, vector-search-for-ML.
- [[finetuning/README|fine-tuning]] — PEFT (LoRA/QLoRA/DoRA), alignment (SFT/RLHF/DPO/GRPO), Unsloth/TRL/Axolotl/torchtune.
- [[frameworks/README|frameworks]] — Python ML/agent orchestration: DSPy, LangGraph, H2O AutoML, MLCube, MindsDB, phidata→agno.
- [[fundamentals/README|fundamentals]] — classification, k-means, GNNs, KAN, zero-shot, unlearning, no-deep-learning-needed.
- [[genetics/README|evolutionary computation]] — MOEA Framework + landscape.
- [[knowledge_graph/README|knowledge graphs]] — KG embeddings (TransE/RotatE), KG-GNNs, KG-augmented LLMs, petgraph.
- [[llm/README|LLM hub]] — models (Llama / DeepSeek / OLMo / mPLUG / Mellum), runtimes (Ollama / Goose / providers / Rig / rsllm), inspection (leaderboards / SEAL / abliteration), system prompts, tokenizers, the `llm_os` mental model.
- [[mcp/README|MCP — Model Context Protocol]] — Anthropic's tool/resource/prompt protocol; A2A; MCP for databases; MCP-UI.
- [[memory/README|LLM/agent memory]] — agentic memory landscape, episodic memory, semantic caching.
- [[orchestration]] — ML/data pipeline orchestration (Airflow / Prefect / Dagster / Argo / Flyte / Metaflow). **Distinct** from [[agents/orchestration|agents/orchestration]] which is LangGraph-style agent control flow.
- [[quantum]] — quantum-ML landscape with the honest "no quantum advantage demonstrated yet" framing.
- [[rag/README|RAG — retrieval-augmented generation]] — RAG production best practices, query preprocessing (HyDE / multi-query), agentic RAG, GraphRAG, KAG, Neo4j RAG.
- [[skills/README|agent skills]] — packaging agent capabilities (`.cursorrules` / `AGENTS.md` / Claude Skills / `skills.sh`).
- [[time_series/README|ML time series]] — forecasting (statistical / ML / DL / foundation models), Transformers-for-TS lineage, KAN for TS, missing-data handling.
- [[tools/README|ML tools]] — Rust DL frameworks (Burn / Candle / tch / dfdx / tract), AI-IDEs (Cursor / Tabby), robotics (LeRobot / DeepLake), workflow (n8n), web research (Webthinker).
- [[voice/README|voice AI]] — STT (AssemblyAI / Whisper / Deepgram), TTS (ElevenLabs / Cartesia / Kokoro), voice-agent platforms (Vapi / Retell / LiveKit Agents / Pipecat).

## See also (cross-section)

- [[../programming/rust/ml/README|programming/rust/ml]] — Rust ML libraries (linfa, candle/burn from the Rust angle, cudarc).
- [[../programming/rust/data/datafusion/README|programming/rust/data/datafusion]] — DataFusion ecosystem (often relevant for ML data prep).
- [[../db/vector/README|db/vector]] — vector databases (Qdrant, Chroma) — the substrate for RAG and semantic search.
- [[../db/quality/data_quality|db/quality/data_quality]] — data-quality discipline; the upstream concern for any ML training pipeline.
- [[../research_papers/README|research papers]] — paper summaries (Data Interpreter, DataMan).

## Keywords

`#ml` `#ai` `#llm` `#agents` `#rag` `#mcp` `#finetuning` `#voice` `#orchestration` `#hub`
