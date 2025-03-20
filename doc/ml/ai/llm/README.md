# Large Language Models (LLMs)

This directory contains notes and resources related to Large Language Models (LLMs), their applications, and the tools and frameworks used to work with them.

## Overview

This section covers various aspects of LLMs, including:

*   **Models:** Specific LLM architectures and implementations.
*   **Tools:** Frameworks and libraries for building and deploying LLM-powered applications.
*   **Techniques:** Methods for improving LLM performance, such as Retrieval-Augmented Generation (RAG).
*   **Ecosystem:** The broader landscape of LLMs, including open-source projects and commercial offerings.

## Key Topics

### LLM Models

*   **DeepSeek:**
    *   [deepseek.md](deepseek.md)
    *   Notes on the DeepSeek family of models, including DeepSeek-R1.
    *   Information on how to reproduce the results from the DeepSeek paper.
    *   Links to relevant resources on Hugging Face and YouTube.
*   **Llama:**
    *   [llama.md](llama.md)
    *   information about Meta's Llama models
*   **MPLUG:**
    *   [mplug.md](mplug.md)
    Multi-modal PLUG models.
*   **Ollama:**
    *   [ollama.md](ollama.md)
    *   information about Ollama, a tool for running LLMs locally.

### LLM Tools and Frameworks

*   **Goose:**
    *   [goose.md](goose.md)
    *   An open-source, extensible AI agent that goes beyond code suggestions.
    *   Capabilities include installing, executing, editing, and testing with any LLM.
    *   Runs locally and can be customized with preferred LLMs and external APIs.
    *   Supports providers like Ollama for running open-source models.
    *   Provides extensions for managing Goose sessions.
*   **Rig:**
    *   [rig.md](rig.md)
    *   A Rust library for building scalable, modular, and ergonomic LLM-powered applications.
    *   Provides core functionalities and an API reference.
*   **rsLLM:**
    *   [rsllm.md](rsllm.md)
    *   Rust AI Stream Analyzer Twitch Bot
* **Inspectus:**
    * [inspectus.md](inspectus.md)
    * Inspectus is a versatile visualization tool for large language models

### LLM Techniques

*   **Retrieval-Augmented Generation (RAG):**
    *   [rag.md](rag.md)
    *   contains information about RAG, a technique for improving LLM responses by retrieving relevant information.
*   **Graph RAG:**
    *   [graph_rag.md](graph_rag.md)
    *  information about using graphs in RAG systems
*   **Neo4j RAG:**
    *   [neo4j_rag.md](neo4j_rag.md)
    *    information about using Neo4j graph database in RAG systems.

### Other

*   **KAG:**
    *   [kag.md](kag.md)
    *   Knowledge based RAG
*   **LLM:**
    *   [llm.md](llm.md)
    *   general information about LLMs.

## Further Exploration

*   **AI Landscape:**
    *   Refer to the [index.md](../index.md) file in the parent directory for a broader overview of the AI landscape.
    *   Includes information about tools like `dfdx`, `Burn`, `tch-rs`, `tract`, and `Candle`.

## Related Resources

*   **Goose:** https://block.github.io/goose/
*   **Goose GitHub:** https://github.com/block/goose
*   **Rig:** https://docs.rig.rs/
*   **Rig API Reference:** https://docs.rs/rig-core/latest/rig/
*   **Deepseek:** https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-14B
* **Open R1:** https://huggingface.co/blog/open-r1
* **Open R1 GitHub:** https://github.com/huggingface/open-r1