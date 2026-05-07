---
title: Voice AI — STT, TTS, voice agents, and speech-native models
keywords: [voice, stt, asr, tts, voice-agents, whisper, elevenlabs, deepgram, livekit, realtime]
status: reviewed
review_date: 2026/05/03
---

# Voice AI — STT, TTS, voice agents, and speech-native models

The voice section covers the four layers of modern voice AI: **speech-to-text (STT/ASR)**, **text-to-speech (TTS)**, **voice agents** (the real-time loop that stitches STT + LLM + TTS + telephony together), and the emerging **speech-native models** that collapse the whole pipeline into one model. Use this README as the landscape map; the two articles below dig into one cloud STT vendor (AssemblyAI) and the voice-agent-platform layer.

## Articles

- [AssemblyAI — production cloud speech-to-text](assemblyai.md) — Universal-2, diarization, summarization, redaction; vs Deepgram / Whisper / open source.
- [Voice-agent platforms — Vapi, Retell, LiveKit Agents, Pipecat, Vocode](build_voice_ai_with_one_api.md) — the "voice-agent-as-a-service" category that emerged in 2024.

## STT / ASR landscape

| Tier | Pick |
|---|---|
| Open-source baseline, free with GPU | **OpenAI Whisper** + optimised forks (**whisper.cpp**, **faster-whisper**, **WhisperX**) |
| Top-of-leaderboard open weights | **NVIDIA Parakeet / Canary**, **Moonshine** |
| Production cloud, lowest streaming latency | **Deepgram** Nova-3 |
| Production cloud, accuracy + extras (diarization, summarization, PII) | **AssemblyAI** Universal-2 ([article](assemblyai.md)) |
| Cheapest viable cloud, English-first | **OpenAI Whisper API** |
| Strong on accents, multilingual, enterprise | **Speechmatics** |
| TTS-vendor-bundled STT, newer | **ElevenLabs Scribe** |
| Hyperscaler default | **AWS Transcribe / Google Cloud Speech-to-Text / Azure Speech** |

## TTS landscape

| Tier | Pick |
|---|---|
| Quality leader (closed, expensive, voice cloning) | **ElevenLabs** |
| Lowest-latency cloud (Sonic) | **Cartesia** |
| Cheap, OpenAI-bundled | **OpenAI TTS** (`tts-1`, `tts-1-hd`, `gpt-4o-mini-tts`) |
| Open-source workhorse | **Coqui XTTS-v2** (Coqui shut down 2024 but model lives on), **Bark** (Suno) |
| Open-source, surprisingly good for 82M params | **Kokoro** (kokoro-onnx) |
| Open-source, slow but expressive | **Tortoise** |
| Hyperscaler defaults | **Google WaveNet/Chirp HD**, **AWS Polly Neural / Generative**, **Azure Neural** |

## Voice agents (the real-time loop)

The pipeline is **VAD → STT → LLM → TTS → speaker output**, with interruption handling and (for phone use) telephony glue. See the [voice-agent-platforms article](build_voice_ai_with_one_api.md) for the full breakdown; the short list:

- **Closed SaaS:** Vapi, Retell, Synthflow, Bland, Air.ai, VoiceFlow Agents.
- **Open frameworks:** LiveKit Agents, Pipecat (Daily.co), Vocode.

## Speech-native models (the 2024-2025 wildcard)

Single-model **speech-to-speech** — no STT/TTS pipeline, no concatenated latency budget, much more natural prosody:

- **OpenAI `gpt-4o-realtime`** — the most-used, websocket API, function-calling supported.
- **Sesame CSM (Conversational Speech Model)** — research-grade demo, very natural prosody.
- **Kyutai Moshi** — open weights (Apache-2.0), full-duplex, runs locally.

These don't yet replace the four-layer pipeline for production (cost per minute, function-calling story, voice-cloning controls, language coverage), but they're eating the low end of the latency curve and worth tracking.

## Decision shortcut

| You want… | Reach for… |
|---|---|
| Transcribe a podcast / video archive (batch) | **Whisper-large-v3** self-hosted, or **AssemblyAI** / **Deepgram** if you need diarization out of the box |
| Live captions in a meeting app | **Deepgram** streaming, or **AssemblyAI** real-time |
| Phone bot, fastest path | **Vapi** or **Retell** (closed SaaS) |
| Phone/web bot, full control | **LiveKit Agents** or **Pipecat** + your own STT/TTS choices |
| Audiobook / character voice | **ElevenLabs** (cloning), or **Kokoro** / **XTTS** if open source |
| Lowest-latency natural conversation | **OpenAI Realtime** or **Moshi** (skip the STT/TTS pipeline) |
| In-browser only, no telephony | Web Speech API + **ElevenLabs** + your LLM of choice |

## Keywords

`#voice` `#stt` `#asr` `#tts` `#voice-agents` `#realtime` `#speech-to-speech` `#whisper` `#elevenlabs` `#deepgram` `#livekit`

## See also

- [[../README|ml]] — ML hub
- [[../llm/README|llm]] — the LLMs that voice agents call into
- [[../tools/README|ml/tools]] — adjacent ML-tool landscape (audio loaders, dataset tools)
- [[../agents/README|agents]] — text-side agent orchestration; many of the same patterns
- [[../mcp/README|mcp]] — MCP servers can expose audio tools to agents
- [[../../tools/security/README|tools/security]] — voice-cloning ethics, deepfake detection
