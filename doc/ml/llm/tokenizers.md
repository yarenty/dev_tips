---
title: Tokenizers — the silent layer that shapes LLM behaviour
main_link: https://github.com/huggingface/tokenizers
keywords: [tokenizers, bpe, sentencepiece, tiktoken, wordpiece, unigram, byte-latent, mamba]
status: reviewed
review_date: 2026/05/03
---

# Tokenizers — the silent layer that shapes LLM behaviour

**Main link:** <https://github.com/huggingface/tokenizers>

## Summary

A tokenizer is the function that converts strings to and from the integer IDs an LLM actually consumes. It is one of the most under-discussed components of an LLM: choice of algorithm (BPE / WordPiece / Unigram / SentencePiece / byte-level BPE), vocabulary size (32k → 256k+), training corpus mix, and special-token scheme all leak into model behaviour in ways that are hard to undo by fine-tuning. Every modern LLM ships a paired tokenizer, and three implementations dominate in production: **Hugging Face `tokenizers`** (Rust, the default for HF Transformers), **`tiktoken`** (Rust + Python, OpenAI's BPE), and **SentencePiece** (Google C++, the unigram/SP-BPE workhorse used by Llama, T5, mT5, Gemma, Mistral). The 2024-2025 frontier is **tokenizer-free** alternatives (byte-level state-space models like Mamba, Meta's Byte Latent Transformer) that bypass the tokenizer entirely.

## Insight

Tokenization is a deep, silent influence on three things you actually care about: **multilingual cost** (a Thai or Hindi sentence can be 5-10× more tokens than its English equivalent because the vocabulary was trained mostly on English — directly inflating per-call price and shrinking effective context), **code performance** (tokenizers that don't reserve indentation/operator merges produce garbage code completions; Llama 3 doubled its vocab partly to fix this), and **character-level reasoning bugs** (the canonical "GPT-4 doesn't know how many R's are in **strawberry**" gotcha — the model sees `["str", "aw", "berry"]` and has no per-character handle on the string). Counting characters, reversing text, sorting alphabetically, and Caesar-cipher style tasks are pathologically hard for tokenized LLMs.

Choose the implementation by the workflow: **HF `tokenizers`** for anything HF-Hub-shaped (training a new tokenizer, fast batch encoding in Rust under a Python facade), **`tiktoken`** when you're token-counting against an OpenAI / Azure-OpenAI API and need exact parity, **SentencePiece** if you're training a model that wants language-agnostic Unicode handling and offline tokenization for reproducibility. Watch for two real footguns: (1) **chat templates** are increasingly fused into the tokenizer (`apply_chat_template`) — older code that hand-builds prompts will silently mis-tokenize the system/user/assistant turn boundaries; (2) **special tokens** drift between checkpoints (a Llama 3.1 vs 3.2 vocab swap, or an instruct vs base variant), so always re-load the tokenizer from the same revision as the model.

## Similar / related topics

- Byte Pair Encoding (BPE) — Sennrich 2015 paper; the dominant family. Variants: byte-level BPE (GPT-2/3/4), SP-BPE.
- WordPiece — Schuster & Nakajima 2012; original BERT tokenizer; greedy longest-match decoding.
- Unigram Language Model — Kudo 2018; probabilistic alternative to BPE; default in SentencePiece.
- Tokenizer-free / byte-level models — Mamba, ByT5, CANINE, Meta's Byte Latent Transformer (Dec 2024).
- Tokenizer visualisation tools — OpenAI's [tokenizer playground](https://platform.openai.com/tokenizer), `tiktokenizer.vercel.app`.

## Internal links
- [[README|llm]]
- [[llm_os]]
- [[models/llama]] — paired with a SentencePiece BPE tokenizer (32k → 128k for Llama 3).
- [[models/deepseek]]
- [[../finetuning/README|finetuning]] — fine-tuning preserves the tokenizer; vocab-extension is a separate (rare) step.

## Keywords

`#tokenizers` `#bpe` `#sentencepiece` `#tiktoken` `#wordpiece` `#unigram` `#byte-latent` `#mamba`

## References / raw notes

### Canonical implementations

| Library | Language | Origin | Used by |
|---------|----------|--------|---------|
| [`huggingface/tokenizers`](https://github.com/huggingface/tokenizers) | Rust core + Python/Node bindings | Hugging Face | Anything via HF Transformers; the de-facto general-purpose option |
| [`openai/tiktoken`](https://github.com/openai/tiktoken) | Rust + Python | OpenAI | GPT-2/3/3.5/4/4o, Azure OpenAI; canonical for OpenAI token counting |
| [`google/sentencepiece`](https://github.com/google/sentencepiece) | C++ + Python | Google | T5/mT5, Llama 1/2 (BPE mode), Mistral, Gemma, many multilingual models |
| [`karpathy/minbpe`](https://github.com/karpathy/minbpe) | pure Python | Andrej Karpathy | reference / pedagogy; matches GPT-4's BPE on a clean corpus |

### Algorithm families

- **BPE (Byte Pair Encoding)** — start from characters/bytes, repeatedly merge the most-frequent adjacent pair until the vocab cap is reached. Sennrich et al., 2015. **Byte-level BPE** (GPT-2 onwards) operates on UTF-8 bytes so the vocabulary covers any Unicode without UNK tokens.
- **WordPiece** — like BPE but the merge criterion is likelihood gain rather than raw frequency. BERT, DistilBERT.
- **Unigram LM** — start from a large vocabulary, prune the tokens that hurt corpus likelihood the least until the cap is reached. Kudo, 2018. Default in SentencePiece.
- **SentencePiece** — a *framework* (not an algorithm) by Google that runs BPE or Unigram directly on raw Unicode (treating whitespace as a regular character via `▁`), so tokenization is language-agnostic and reversible.

### The "strawberry" gotcha and friends

GPT-4-class models famously miscount the R's in "strawberry" because the BPE vocabulary represents the word as something like `["str", "aw", "berry"]` — the model has no token-level signal for the individual characters. Same root cause:

- Counting letters or syllables.
- Reversing strings.
- Caesar / ROT-N ciphers.
- Sorting characters alphabetically.
- Detecting spelling errors at the character level.

Workarounds: (a) ask the model to write code that does the operation, (b) use a tool/agent step, (c) for serious char-level work pick a **byte-level** or **tokenizer-free** model. Reasoning models (o1, DeepSeek-R1) often get these right by spending compute decomposing the string into individual characters in their scratchpad.

### Multilingual and code costs

A heuristic from public tiktoken/Llama tokenizers:

- English: ~1.3 tokens/word.
- Code (Python/JS): ~1.5-2 tokens/significant word; Llama 3's expanded 128k vocab roughly halved the token cost of code vs Llama 2.
- Chinese / Japanese: ~1 token per character, sometimes ~1 per ideogram.
- Hindi / Thai / Tamil: pre-Llama-3, frequently 5-10 tokens per word, with each grapheme cluster split into byte fallbacks.

Direct consequences: API cost, effective context window, and per-token sampling temperature all differ across languages even for the same model.

### Chat templates

Modern instruct models embed conversation structure into the tokenizer via `chat_template` (a Jinja2 template attached to `tokenizer_config.json`). Hugging Face standardised this with `tokenizer.apply_chat_template(messages)`. If you build prompts by hand-concatenating `<|im_start|>` markers etc. you will eventually mis-mark the turn boundaries on a checkpoint update — always go through `apply_chat_template`.

### 2024-2025 trends

- **Tokenizer-free / byte-level models** — state-space models (Mamba, Mamba-2) and Meta's [Byte Latent Transformer (BLT)](https://ai.meta.com/research/publications/byte-latent-transformer-patches-scale-better-than-tokens/) (Dec 2024) operate directly on bytes, dynamically grouping them into "patches" — promising for multilingual and char-level tasks but not yet at parity on standard benchmarks.
- **Super-tokens / multi-token prediction (MTP)** — DeepSeek-V3 trains an auxiliary head to predict multiple tokens at once, which helps both inference speed (speculative decoding) and learning signal.
- **Tokenizer-influenced scaling laws** — papers (e.g. Tao et al., *Scaling Laws with Vocabulary*, 2024) show that the optimal vocab size grows with model size, faster than typically assumed; under-sized vocabs leave significant compute on the table.
- **Vocabulary extension / re-training** — projects like Sailor (extends Mistral/Qwen for SE Asian languages), Aya, Tamil Llama add language-specific tokens; non-trivial to do without breaking the model — usually requires a continued-pretraining pass.

### Useful tools

- [OpenAI tokenizer playground](https://platform.openai.com/tokenizer) — visualise GPT-* tokenization for a string.
- [`tiktokenizer.vercel.app`](https://tiktokenizer.vercel.app) — same idea but supports Llama, Claude (approximate), Gemma, Mistral, etc.
- [`huggingface/tokenizers` quicktour](https://huggingface.co/docs/tokenizers/quicktour) — train a new tokenizer in ~10 lines.
