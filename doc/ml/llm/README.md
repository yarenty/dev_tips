<!-- initial content from doc/ml/llm.md -->

# LLMs



## WOLF


https://wolfv0.github.io/


WOrLd summarization Framework for accurate video captioning. Wolf is an automated captioning framework that adopts a mixture-of-experts approach, leveraging complementary strengths of Vision Language Models (VLMs). By utilizing both image and video models, our framework captures different levels of information and summarizes them efficiently. Our approach can be applied to enhance video understanding, auto-labeling, and captioning. To evaluate caption quality, we introduce CapScore, an LLM-based metric to assess the similarity and quality of generated captions compared to the ground truth captions. We further build four human-annotated datasets in three domains: autonomous driving, general scenes, and robotics, to facilitate comprehensive comparisons. We show that Wolf achieves superior captioning performance compared to state-of-the-art approaches from the research community (VILA1.5, CogAgent) and commercial solutions (Gemini-Pro-1.5, GPT-4V). For instance, in comparison with GPT-4V, Wolf improves CapScore (caption quality) by 55.6% and CapScore (caption similarity) by 77.4% on challenging driving videos. Finally, we establish a benchmark for video captioning and introduce a leaderboard, aiming to accelerate advancements in video understanding, captioning, and data alignment.

1[](https://wolfv0.github.io/static/images/main.png)

<!-- merged from doc/ml/ai/llm/README.md -->

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

<!-- merged from doc/ml/ai/llm/llm.md -->

# LLMs

![](https://x.com/i/status/1923611595852100085)

![img.png](../../../assets/img/llms.png)


# Awsome Rust LLM

https://github.com/jondot/awesome-rust-llm


```

                                                                      __       __ __          
.---.-.--.--.--.-----.-----.-----.--------.-----.___.----.--.--.-----|  |_ ___|  |  .--------.
|  _  |  |  |  |  -__|__ --|  _  |        |  -__|___|   _|  |  |__ --|   _|___|  |  |        |
|___._|________|_____|_____|_____|__|__|__|_____|   |__| |_____|_____|____|   |__|__|__|__|__|

https://github.com/jondot/awesome-rust-llm
```
**Awesome Rust LLM** is an awesome style list that keeps track and curates the best Rust based LLM frameworks, libraries, tools, tutorials, articles and more. PRs are welcome!

## Models & Inference

* [llm](https://github.com/rustformers/llm) - a Rust library for running inference from a number of supported LLMs, loads [ggml](https://github.com/ggerganov/ggml) based models
* [rust-bert](https://github.com/guillaume-be/rust-bert) - all in one pipelines for for transformer-based models (BERT, DistilBERT, GPT2,...). Good for local embedding, port of `transformers` (python)
* [llm-chain](https://github.com/sobelio/llm-chain) - chaining LLMs in Rust
* [smartgpt](https://github.com/Cormanz/smartgpt) ([how it works](https://twitter.com/jondot/status/1660576729549664261))- use LLMs with the ability to complete complex tasks using plugins
* [diffusers](https://github.com/pykeio/diffusers) - Stable Diffusion using Rust, 45% faster than Pytorch
* [postgresml](https://github.com/postgresml/postgresml) - an amazing Postgres extension to do model fetching, inference all with SQL as part of your Postgres instance


## Projects

* [aichat](https://github.com/sigoden/aichat) - a pure Rust CLI implementing AI chat, with advanced features such as real-time streaming, text highlighting and more

* [browser-agent](https://github.com/m1guelpf/browser-agent/) - a headless browser driven by GPT-4. Sends off a simplified page representation and receives & executes instructions from GPT using a custom message format
* [tenere](https://github.com/pythops/tenere) - TUI interface for LLMs

How it works? here's a prompt snippet:

```
You must respond with ONLY one of the following commands AND NOTHING ELSE:
    - CLICK X - click on a given element. You can only click on links, buttons, and inputs!
    - TYPE X \"TEXT\" - type the specified text into the input with id X and press ENTER
    - ANSWER \"TEXT\" - Respond to the user with the specified text once you have completed the objective
```
* [ajeto](https://github.com/bausano/ajeto) - LLM personal assistant
* [shortgpt](https://github.com/rupeshs/shortgpt) - Ask shortgpt for instant and concise answers
* [autorust](https://github.com/minskylab/auto-rust) - macros that generate AI driven rust code in compile time
* [clerk](https://github.com/blankenshipz/clerk) - LLM based file organizer
* [gptcommit](https://github.com/zurawiki/gptcommit) - prepare commit message with GPT


## LLM Memory

* [indexify](https://github.com/diptanu/indexify) - A retrieval and long term memory service for LLMs
* [memex](https://github.com/spyglass-search/memex) - Super-simple, fully Rust powered "memory" (doc store + semantic search) for LLM projects, semantic search, etc.
* [motorhead](https://github.com/getmetal/motorhead) -  a memory and information retrieval server for LLMs.
    * Uses Redis as vector store for long term memory,
    * Works with OpenAI API
    * [js example](https://github.com/getmetal/motorhead/blob/main/examples/chat-js), [python example](https://github.com/getmetal/motorhead/blob/main/examples/chat-py)

## Core Libraries

* [tiktoken](https://github.com/openai/tiktoken) - tiktoken is a Python library with a Rust core implementing a fast [BPE](https://en.wikipedia.org/wiki/Byte_pair_encoding) tokeniser for use with OpenAI's models
    * BPE is done in Rust
    * Made by OpenAI

* [tiktoken-rs](https://github.com/zurawiki/tiktoken-rs) - a Rust focused library based on the `tiktoken` core with additional enhancements for use in Rust code. (Pyton parts in `tiktoken` done in pure Rust)

```rust
// Rust
use tiktoken_rs::p50k_base;

let bpe = p50k_base().unwrap();
let tokens = bpe.encode_with_special_tokens(
  "This is a sentence   with spaces"
);
println!("Token count: {}", tokens.len());
```

* [polars](https://github.com/pola-rs/polars) - a faster, pure Rust pandas alternative

* [rllama](https://github.com/Noeda/rllama) - a pure Rust implemenation of LLaMa inference. Great for embedding into other apps or wrapping for a scripting language.
* [whatlang](https://github.com/quickwit-oss/whichlang) - Rust library using a multiclass logistic regression model to detect languages

* [OpenAI API](https://github.com/uiuifree/rust-openai-chatgpt-api)  - a strongly typed Rust client for the OpenAI API

## Tools

* [spider](https://github.com/spider-rs/spider) - crawler / spider written in Rust for when you need a whole-website dump. Unlike Scrapy, focuses on dumping data but the post-processing is done later.

## Services

* [dust](https://github.com/dust-tt/dust) - a full service for workflow running with composable blocks. Core is in Rust, various frontends in Typescript.


## Vector Stores

* [pgvecto.rs](https://github.com/tensorchord/pgvecto.rs) - Vector database plugin for Postgres, written in Rust, specifically designed for LLM. 20x faster than pgvector
* [qdrant](https://github.com/qdrant/qdrant) - Qdrant - Vector Database for the next generation of AI applications



## LLM

https://github.com/rustformers/llm

llm - Large Language Models for Everyone, in Rust
llm is an ecosystem of Rust libraries for working with large language models - it's built on top of the fast, efficient GGML library for machine learning.


