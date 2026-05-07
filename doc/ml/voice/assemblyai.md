---
title: AssemblyAI — production cloud speech-to-text
main_link: https://www.assemblyai.com/
keywords: [assemblyai, voice, speech, stt, asr, transcription, diarization, universal-2]
status: reviewed
review_date: 2026/05/03
---

# AssemblyAI — production cloud speech-to-text

**Main link:** <https://www.assemblyai.com/>

## Summary

AssemblyAI is a developer-focused **cloud speech-to-text (STT/ASR)** API. Their flagship is the **Universal-2** model (2024) for English, plus a multilingual line. Beyond raw transcription they bundle **speaker diarization, sentiment analysis, summarization, PII redaction, topic detection, content moderation, and chapter detection** behind one HTTP API and a small set of SDKs. They support both **batch transcription** (upload an audio URL or file, get a JSON result) and **real-time streaming** (websocket, ~300-500 ms latency) — the two shapes you need for, respectively, ingesting recorded media and powering live voice agents.

## Insight

Pick AssemblyAI when you want **production-grade accuracy + extras + a polished SDK**, and you don't want to run any infrastructure. The "extras" — especially diarization that actually works on noisy two-speaker calls, and the structured summarization endpoint — are the differentiator vs simply slapping Whisper on a server. Their docs and React/Node/Python SDKs are unusually good for the category.

**Decision shortcut.**

| Need | Pick |
|---|---|
| Cheapest viable cloud STT, English only, no extras | **OpenAI Whisper API** ($0.006/min) |
| Lowest streaming latency, real-time voice agents | **Deepgram** (Nova-3, Aura), **Cartesia Ink** |
| Best accuracy + diarization + summarization in one API | **AssemblyAI** Universal-2 |
| Self-hosted, free if you have a GPU, offline | **whisper.cpp** / **faster-whisper** / **WhisperX** |
| Hyperscaler default, you're already on AWS/GCP/Azure | **AWS Transcribe / Google Cloud Speech / Azure Speech** |
| Open source with state-of-the-art word-error-rate | **NVIDIA Parakeet / Canary**, **Moonshine** |

**Honest gotchas.**

- **Per-minute pricing adds up.** Roughly $0.12-0.37/hour for batch async, more for real-time + features. If you're transcribing thousands of hours of podcast/video archive, run the maths against a self-hosted Whisper-large box or against Deepgram's bulk tiers.
- **Universal-2 is English-first.** The multilingual stack is competitive but not always best-in-class per language; for non-English-heavy workloads benchmark against Whisper-large-v3 / Deepgram Nova-3 / Speechmatics.
- **Streaming has its own model and pricing.** Don't extrapolate batch accuracy to the streaming endpoint blindly; test the model variant you'll actually use.
- **Diarization is "good enough", not magic.** It still confuses overlapping speakers and quiet speakers. For courtroom/medical-grade diarization you'll need post-processing or a specialist (e.g., **pyannote.audio**).
- **The "LeMUR" LLM-on-transcripts feature** is a wrapper over frontier models — it's convenient but you can replicate it with Anthropic/OpenAI directly if you want control over prompts and pricing.

## Similar / related topics

- **Deepgram** — closest peer; Nova-3 model, slightly faster + cheaper batch, very strong streaming, the go-to for real-time voice agents.
- **OpenAI Whisper API** — cheapest cloud option, basic features only (no diarization, no PII redaction, no streaming until very recently).
- **whisper.cpp / faster-whisper / WhisperX** — open-source baselines; free if you have GPU/CPU budget; WhisperX adds forced alignment + diarization.
- **ElevenLabs Scribe** — newer entrant (2024) from the TTS leader, focuses on accuracy + speaker labels.
- **Speechmatics** — UK-based, very strong on accents and languages, enterprise-leaning pricing.
- **NVIDIA Parakeet / Canary** — open weights, currently top of HuggingFace Open ASR Leaderboard for English.

## Internal links

- [[voice/README|voice]] — STT/TTS/voice-agent landscape parent
- [[build_voice_ai_with_one_api]] — voice-agent-as-a-service platforms (the layer above raw STT)
- [[../llm/README|llm]] — the LLM that consumes the transcript

## Keywords

`#assemblyai` `#stt` `#asr` `#speech-to-text` `#transcription` `#diarization` `#universal-2` `#voice`

## References / raw notes

- AssemblyAI homepage: <https://www.assemblyai.com/>
- "Build Voice AI" landing: <https://www.assemblyai.com/build-voice-ai>
- Universal-2 announcement: <https://www.assemblyai.com/blog/announcing-universal-2-speech-recognition-model>
- Open ASR Leaderboard (HF): <https://huggingface.co/spaces/hf-audio/open_asr_leaderboard>
- Deepgram (peer): <https://deepgram.com/>
- Whisper.cpp: <https://github.com/ggml-org/whisper.cpp>
- faster-whisper: <https://github.com/SYSTRAN/faster-whisper>
- WhisperX (forced alignment + diarization): <https://github.com/m-bain/whisperX>
- pyannote.audio (diarization specialist): <https://github.com/pyannote/pyannote-audio>
