---
title: "EmbeddingGemma 2 repurposed for speech-to-text: 1 hour of audio in 12.6 s"
slug: "embeddinggemma-transcribe"
date: 2026-10-13T12:03:15+0000
summary: "An independent Mac app turns Google's EmbeddingGemma 2, a multimodal embedding model, into a speech-to-text system by training only a small reader that maps audio slices to the nearest of about 229,000 English words. It transcribes an hour of audio in 12.6 seconds on a base M3 MacBook Air, but its word error rate is about 5%, versus 2-3% for leading speech models."
source: "https://eg2.haku.sh/"
source_title: "Embedding Gemma Transcribe"
source_site: "eg2.haku.sh"
hn_url: "https://news.ycombinator.com/item?id=50069971"
---

**Bottom line:** The author reports that EmbeddingGemma 2, which puts text, images, audio and video in one shared embedding space, can be made to transcribe speech without retraining its audio encoder. The result is very fast but noticeably less accurate than the best dedicated models. These are the author's own benchmarks.

- **How it works:** the audio encoder is unchanged. A small trained "reader" maps each short audio slice into the text space, the closest word among roughly 229,000 English words wins, and a tiny extra model adds punctuation and capitalization. Training the reader took under two hours on 8 H100s using openly licensed English speech.
- **Speed:** about 288x real time on the Neural Engine of a base 8 GB M3 MacBook Air (1 hour in 12.6 s), versus about 150x for IBM Granite Speech 5.0 TurboCTC on the GPU via MLX (25.7 s) in the author's same-machine test. Parakeet TDT 0.6B v3 figures (156x on M4 Pro, 335x on M5 Pro) come from FluidAudio's published benchmarks on different hardware and audio, so they aren't a like-for-like comparison.
- **Accuracy:** word error rate is about 5% today, against 2-3% for leading speech models. The author expects a better-trained reader to close the gap.
- **Use:** free macOS 14+ app for Apple Silicon (715 MB model download on first launch). Drop in a file or hold Right Option to dictate into any app; it runs on-device. Not affiliated with Google; the base model is Apache 2.0.
