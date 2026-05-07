---
title: rsllm — Rust AI stream analyzer & Twitch bot (Candle-based)
main_link: https://github.com/groovybits/rsllm
keywords: [rsllm, rust, candle, ndi, twitch, mpegts, tts, metal, niche]
status: reviewed
---

# rsllm — Rust AI stream analyzer & Twitch bot (Candle-based)

**Main link:** <https://github.com/groovybits/rsllm>

## Summary

`rsllm` (RsLLM) is a niche, hobby/research Rust pipeline by Chris
Kennedy (Feb 2024) that wires a Candle-based local LLM (Mistral or
Gemma) to Twitch chat, NDI video/audio output, Stable Diffusion
image generation, and TTS (MetaVoice / OpenAI TTS / Mimic3) — with
the explicit goal of running an "automated AI Twitch streamer" or
analysing MPEG-TS streams and OS stats on Apple Silicon. It also
supports an OpenAI-compatible backend (e.g. `llama.cpp` server)
when local-Candle isn't enough.

## Insight

Honest framing: `rsllm` is an **opinionated demo / personal toolkit**
in the broadcasting/video-AI niche, not a general-purpose Rust LLM
runtime. Reach for it only if your problem actually intersects its
sweet spot:

- You want a **Twitch chatbot that also generates images and TTS
  speech and broadcasts via NDI** — there's nothing else in Rust
  packaged this way.
- You're on **Apple Silicon (M1/M2/M3)** — the project is
  Metal-first; CUDA/Nvidia paths are explicitly listed as
  "needs someone to fix it up."
- You want to study **a real Candle integration** with multiple
  modalities (text, image, audio) wired together.

For anything else, the Rust LLM landscape has better-fitting tools:

| You want                                  | Use this instead                |
|-------------------------------------------|---------------------------------|
| LLM-app framework (agents/RAG/tools)      | [[rig]]                         |
| Run an inference server locally           | [[ollama]] (or `llama.cpp` directly) |
| Embed Candle without the Twitch bits      | `candle` directly (HF)          |
| Higher-level Rust LLM toolkit + UI        | `kalosm`                        |
| Pure-Rust Mistral/Llama inference engine  | `mistral.rs`                    |

Other caveats: features like Whisper voice input, RAG over Chroma,
SCTE-35 ad-break detection, ZeroMQ daemon mode are listed on the
roadmap but were WIP at the time the README was written; treat the
roadmap as aspirational. Maintenance pace is single-author.

## Similar / related topics

- [[rig]] — general-purpose Rust LLM-app framework (different scope).
- `candle` — HuggingFace's Rust inference framework that rsllm wraps.
- `mistral.rs` — production-shaped pure-Rust inference engine.
- AUTOMATIC1111 / ComfyUI — the SD UI server rsllm can call out to.
- StreamElements / Streamlabs — the non-AI Twitch-bot incumbents.

## Internal links
<!-- reviewed -->
- [[rig]] — the more general Rust LLM client framework.
- [[ollama]] — alternative for "just serve a model locally."
- [[programming/rust/ml/ml_in_rust|ml_in_rust]] — broader Rust ML landscape (incl. Candle/burn).
- [[../README|llm]] — parent LLM hub.

## Keywords

`#rsllm` `#rust` `#candle` `#ndi` `#twitch` `#mpegts` `#tts` `#metal` `#niche`

## References / raw notes

- Repo: <https://github.com/groovybits/rsllm>
- Author: Chris Kennedy (Feb 2024).
- Licence: MIT.

### What it is (from upstream)

> Rust AI Stream Analyzer Twitch Bot. RsLLM is an AI pipeline 100% in
> Rust for Transformer/Tensor code that leverages the Candle framework
> from HuggingFace. […] Designed for developers and researchers
> aiming to integrate local LLMs with Rust, bypassing the need for
> external dependencies and Python code for chatbots and other AI
> programs.

### Key features

- **Local LLM** via Candle (Mistral, Gemma); Apple Metal optimised.
- **AI analyzer** processing inputs and generating outputs across
  text, voice, speech and images (WIP).
- **Voice / speech** — planned Whisper integration for voice-driven
  interaction.
- **Image generation + NDI output** — Stable Diffusion via Candle or
  AUTOMATIC1111; NDI (Network Device Interface) for broadcast-grade
  streaming over IP.
- **TTS** — MetaVoice (default, WIP), OpenAI TTS API (paid,
  high-quality), Mimic3 (local, free).
- **Twitch chat integration** — real-time AI replies in Twitch chat.
- **MPEG-TS analysis** and OS-stats prompt-injection patterns.

### Components

- **Candle integration** — Rust-native LLMs (Mistral, Gemma)
  optimised for Metal on macOS.
- **OpenAI-API-compatible backend** for `llama.cpp` server, so
  external/custom models can be plugged in.
- **Real-time analysis & content generation** — text, image, speech.

### Setup

Prerequisites:

- Rust + Cargo.
- Ideally an Apple Silicon Mac (M1/M2/M3).
- NDI library (only if you want NDI output for OBS).

```sh
git clone https://github.com/groovybits/rsllm.git
cd rsllm
./scripts/compile.sh    # handles NDI SDK + DYLD_LIBRARY_PATH
cp .env.example .env    # set OpenAI key if desired
```

### Example commands

```sh
./scripts/compile.sh           # build
./scripts/twitch.sh            # full Twitch-bot example
./scripts/mpeg_analyzer.sh     # experimental MpegTS analyzer (WIP)
./scripts/mpeg_poetry.sh       # MPEG-TS prompt-injection poetry demo
./scripts/system_health.sh     # OS-stats prompt injection
```

Running with Candle + OS-stats prompt injection:

```sh
cargo run --release --features fonts,ndi,mps,metavoice,audioplayer -- \
  --candle_llm gemma \
  --model-id "2b-it" \
  --max-tokens 800 \
  --temperature 0.8 \
  --ai-os-stats \
  --sd-image \
  --ndi-images \
  --ndi-audio \
  --system-prompt "You create image prompts from OS system stats health state." \
  --query "How is my system doing? Create a report on the system health as visual image descriptions."
```

### NDI specifics

- Add `--features ndi` to the Cargo build.
- `scripts/compile.sh` retrieves `libndi.dylib` automatically.
- Set `DYLD_LIBRARY_PATH` so the linker finds it:

  ```sh
  export DYLD_LIBRARY_PATH=$PWD:$DYLD_LIBRARY_PATH
  ```

- Optional: log into HuggingFace Hub via `huggingface-cli login` to
  silence some warnings.

### Roadmap (aspirational, as of upstream README)

- Local DB (SQLite/MongoDB) → Chroma DB for RAG over chat history.
- MPEG-TS chat (freeform analysis over current and historical streams).
- Image/TTS latency improvements + NDI pre-queue for sync.
- Perceptual hashing (DCT64) for ad-break / commercial detection;
  SCTE-35 integration.
- Daemon mode over ZeroMQ.
- Whisper Candle for speech-to-text.
- `ffmpeg-next-sys` for real-time video transformation.
- Cap'n Proto for protocol serialisation.
- MetaMusic (mood-based music generation).

### Acknowledgments (upstream)

Candle (HuggingFace), OpenAI (API + TTS), MetaVoice, Mimic3, Whisper,
Google Gemini, Mistral.
