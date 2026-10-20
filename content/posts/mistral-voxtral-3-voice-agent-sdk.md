---
title: "Mistral Voxtral 3: 50-language speech models and a voice agent SDK"
slug: "mistral-voxtral-3-voice-agent-sdk"
date: 2026-10-20T17:02:22+0000
summary: "Mistral released Voxtral 3, a family of speech-to-text and text-to-speech models plus a voice agent SDK that runs listening, reasoning, tool calling and speaking in one real-time streaming pipeline. It's available through Mistral's EU-hosted API or for self-deployment, which Mistral pitches as data sovereignty."
source: "https://mistral.ai/news/voxtral-3/"
source_title: "Voxtral 3: Voice Agent SDK & Speech Models in 50+ Languages"
source_author: "Mistral AI"
source_site: "mistral.ai"
hn_url: "https://news.ycombinator.com/item?id=50168396"
---

Mistral released Voxtral 3, a family of speech-to-text and text-to-speech models plus a voice agent SDK that runs listening, reasoning, tool calling and speaking in one real-time streaming pipeline. It's available through Mistral's EU-hosted API or for self-deployment, which Mistral pitches as data sovereignty.

- **Transcription:** Voxtral Small Transcribe 3 covers 50+ languages, with diarization (who spoke when), context biasing for domain terms without fine-tuning, and end-of-turn detection. Mistral says it outperforms Whisper, Scribe v2 and Gemini 3.5 Transcribe on its benchmarks. The 4B-parameter Mini variant costs $0.0035 per minute.
- **Speech synthesis:** Voxtral Mini TTS 3 adapts emotion, pace and emphasis to the conversation, with 35+ languages and 50+ dialects.
- **Voice agent SDK:** Agents can call tools and complete multi-step tasks during live phone or web conversations. Mistral's demo books a medical appointment from a French referral letter.
- **Pro Voice:** Voice adaptation that Mistral says reproduces a speaker's character and style across languages from about 30 minutes of audio.
- **Deployment:** Use the EU-hosted API or run it on your own infrastructure, with control over whether audio is retained.

The benchmark claims are Mistral's own.
