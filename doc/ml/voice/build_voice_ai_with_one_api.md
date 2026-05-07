---
title: Voice-agent platforms — Vapi, Retell, LiveKit Agents, Pipecat, Vocode
main_link: https://www.assemblyai.com/build-voice-ai
keywords: [voice-agents, vapi, retell, livekit, pipecat, vocode, telephony, real-time, voice-ai-platform]
status: reviewed
---

# Voice-agent platforms — Vapi, Retell, LiveKit Agents, Pipecat, Vocode

**Main link:** <https://www.assemblyai.com/build-voice-ai>

## Summary

A new product category emerged in 2024: the **voice-agent-as-a-service platform**, which packages the full real-time loop — **VAD → STT → LLM → TTS → audio out**, plus interruption handling and telephony — behind one SDK or one API. The pitch is "build a voice product in minutes without juggling five vendors": one provider stitches together AssemblyAI/Deepgram/Whisper for STT, OpenAI/Anthropic/your-own-LLM for the brain, ElevenLabs/Cartesia/OpenAI for TTS, and Twilio/Telnyx for phone numbers. Closed-source SaaS examples include **Vapi, Retell, Synthflow, Bland, Air.ai, VoiceFlow Agents**; the open-source / framework counterparts are **LiveKit Agents, Pipecat (Daily.co), Vocode**.

## Insight

These platforms exist because building a "good" voice agent is harder than gluing four APIs together. The hard parts are:

- **End-to-end latency under ~700 ms** (humans notice anything slower than ~1 s as awkward). Each hop — VAD detect end-of-speech, STT finalise, LLM TTFT, TTS first-byte, network buffering — eats budget. Streaming everything is non-negotiable.
- **Interruption / barge-in.** When the user starts speaking while the bot is talking, you must cut the TTS, flush buffers, re-arm the STT, and rewind any partial LLM output that was about to be played.
- **Voice activity detection (VAD).** Cheap (Silero, WebRTC VAD) and good enough for most cases, but tuning the silence-threshold for telephony noise floors is its own art.
- **Telephony glue.** SIP trunks, PSTN numbers, DTMF, voicemail detection, call recording, transfers — these are not LLM problems but they're 80% of a "voice for support" deployment.
- **Custom LLM / function calling / RAG.** Most platforms now let you point at your own model endpoint and define tools the agent can call mid-conversation.

**Decision shortcut.**

| Need | Pick |
|---|---|
| Fastest to a working phone bot, drag-and-drop UI | **Vapi**, **Retell**, **Synthflow** |
| Outbound-call mass-dialling sales/support | **Air.ai**, **Bland** |
| Full control, you'll write Python, open source | **Pipecat** (Daily.co), **Vocode** |
| WebRTC + voice + video, you'll write TS/Python/Go/Rust | **LiveKit Agents** |
| In-browser only, no telephony | **Vercel AI SDK** + Web Speech API + ElevenLabs |
| Bypass the STT/TTS pipeline entirely | **OpenAI Realtime** (`gpt-4o-realtime`), **Sesame CSM**, **Moshi** |

**The 2024-2025 wildcard: speech-native models.** OpenAI's `gpt-4o-realtime`, Sesame's CSM, and Kyutai's Moshi do **end-to-end speech-to-speech in a single model** — no separate STT/TTS, no concatenated latency budget, much more natural prosody. They're not yet a drop-in for these platforms (they cost more per minute, function-calling is awkward, voice cloning controls vary), but they're eating the bottom of the latency curve. Watch for the platforms above to add a "realtime model" mode and skip half their own pipeline.

**Honest gotchas.**

- **Pricing stacks.** You pay the platform's per-minute fee on top of the underlying STT, TTS, and LLM costs. A 5-minute call can cost $0.30-1.00 all-in.
- **Voice cloning ethics.** ElevenLabs / OpenAI TTS / Cartesia all support cloning. Most platforms don't enforce consent — that's on you. See `[[../../tools/security/README|security]]` for the deepfake/fraud angle.
- **Lock-in risk.** Vapi/Retell config files don't port to Pipecat or LiveKit. Pick the layer (open framework vs SaaS) early.
- **Closed platforms move fast and break configs.** A Twilio integration that worked last quarter may need re-wiring this quarter. Pin SDK versions.

## Similar / related topics

- **Vapi** — closed-source SaaS, currently the popular default for "stand up a phone agent in an afternoon"; YC W23.
- **Retell AI** — closed-source SaaS, comparable to Vapi, low-latency-focused.
- **LiveKit Agents** — open-source agent framework on top of LiveKit's WebRTC stack; Python + Node SDKs; the production substrate behind OpenAI's Realtime demos.
- **Pipecat** — open-source Python framework from Daily.co; pluggable STT/TTS/LLM blocks; very active community.
- **Vocode** — open-source Python framework, slightly older than Pipecat, similar shape.
- **Synthflow / Bland / Air.ai** — outbound-call SaaS, often pitched at sales teams.
- **OpenAI Realtime API (`gpt-4o-realtime`)** — single-model speech-to-speech, the "skip the pipeline" option.
- **Sesame CSM** / **Kyutai Moshi** — research/open speech-native models in the same space.

## Internal links

<!-- reviewed -->
- [[voice/README|voice]] — STT/TTS/voice landscape parent
- [[assemblyai]] — the STT layer most of these platforms call into
- [[../agents/README|agents]] — the broader agent-orchestration story (text-side counterpart)
- [[../llm/runtimes/README|llm/runtimes]] — where the brain runs
- [[../mcp/README|mcp]] — MCP servers can sit behind the agent's tool-calling

## Keywords

`#voice-agents` `#vapi` `#retell` `#livekit` `#pipecat` `#realtime` `#telephony` `#speech-to-speech` `#voice-ai-platform`

## References / raw notes

The AssemblyAI marketing tagline that originally seeded this article:

> Speech-to-text, speaker diarization, PII redaction, topic detection, and LLM integration — all in one platform. Build intelligent voice products in minutes without juggling five vendors.

Canonical references for the platform/framework layer:

- AssemblyAI's "Build Voice AI" pitch: <https://www.assemblyai.com/build-voice-ai>
- Vapi: <https://vapi.ai/>
- Retell AI: <https://www.retellai.com/>
- LiveKit Agents: <https://docs.livekit.io/agents/>
- Pipecat (Daily.co): <https://github.com/pipecat-ai/pipecat>
- Vocode: <https://github.com/vocodedev/vocode-core>
- Synthflow: <https://synthflow.ai/>
- Bland AI: <https://www.bland.ai/>
- OpenAI Realtime API: <https://platform.openai.com/docs/guides/realtime>
- Sesame CSM: <https://www.sesame.com/research/crossing_the_uncanny_valley_of_voice>
- Kyutai Moshi: <https://moshi.chat/>
- Silero VAD: <https://github.com/snakers4/silero-vad>
