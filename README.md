---
title: Mac Voili
version: 0.1.17
description: Voice isolation for noisy sound sources and SDR radio applications, macOS app for Apple silicon
license: Apache-2.0
---

<p align="center"><img src="assets/voili-256.png" width="128" alt="Mac Voili"></p>

# Mac Voili

**Voice isolation for noisy sound sources and SDR radio applications** · macOS app for Apple silicon

**Download:** [latest release](https://github.com/HDZucht/mac-voili/releases/latest) (DMG) · **Deutsch:** [ANLEITUNG.md](ANLEITUNG.md)

## Install

1. Open the DMG and drag "Mac Voili.app" onto "Applications".
2. First start: right-click the app → "Open" → "Open". The app is signed with its own identity and not notarised by Apple, so macOS asks once.
3. For system audio (browser, KiwiSDR, WebSDR, YouTube) install the virtual device BlackHole 2ch: `brew install blackhole-2ch` or [github.com/ExistentialAudio/BlackHole](https://github.com/ExistentialAudio/BlackHole).
4. In Mac Voili choose "BlackHole 2ch" as input. System audio is routed to it and restored when you quit.

Requirements: Mac with Apple silicon (M1 or later), macOS 14 or later. On first start the app tunes itself to the GPU (about 20 seconds).

## What the program is for

Mac Voili pulls voices out of noisy audio, live, while you listen. It hooks into the Mac's system audio and runs every sound through a neural network that keeps only the human voice: the web browser with KiwiSDR or WebSDR, SDR programs such as SDR Oxide or SDR++, streams and recordings. Hiss, crackle, whistles, carriers, interference and adjacent stations are largely removed.

It is made for radio that is otherwise hard to listen to:

- **Shortwave broadcasts (AM)** with fading, static crashes and adjacent-channel interference
- **SSB** in amateur radio and in HF aviation and maritime communication, often at the edge of audibility
- **Distant stations through web SDR receivers** (KiwiSDR, WebSDR, UberSDR)
- **Noisy recordings and streams** of any kind

The network is Inworld Voice Isolator v1 (Apache-2.0). It runs on your Mac's GPU; the audio stays on the computer.

## The key trick: set speech and music differently

The isolator knows only voice and everything else. For speech that is exactly right: the noise disappears, the voice remains. For music, however, the isolator also removes the instruments, because they are not a voice. What remains is hollow singing and fragments.

Broadcast stations keep switching between news, presentation and music. Mac Voili therefore uses Apple's sound classification to detect what is playing and switches between **two strengths**:

| Control | Recommended | Effect |
|---|---|---|
| **Voice extraction for speech** | **95–100 %** | Full noise removal, only the voice remains |
| **Voice extraction for music** | **0–20 %** | Music stays complete, only a little isolator |
| **Treble for music** | **−6 to −18 dB** | Lowers the hiss during music without destroying the instruments |

This keeps a heavily disturbed station listenable throughout: during speech the isolator filters hard, during music the treble control takes over the noise reduction. With **Automatic** on, Mac Voili switches to the music setting after 3.5 s of music and back after 1.5 s of speech, with a smooth transition. The "Active:" label shows which setting applies.

## Recommended settings

### Extremely noisy SSB (amateur radio, aviation, maritime)

- Choose **Quality**. The offline model sees a stretch of audio before and after each syllable and therefore gets more out of weak signals.
- **Automatic off, speech by hand.** SSB carries no music, and heavy noise can mislead the sound classification.
- **Voice extraction for speech 100 %.**
- **Bandwidth 6 or 8 kHz.** SSB occupies about 3 kHz; above that there is only noise, and the network computes less.
- **Volume +10 to +20 dB.** The isolated voice is quieter than the noisy original; a soft limiter catches peaks.

### Shortwave broadcasts with news and music

- **Quality**, **Automatic on**.
- **Voice extraction for speech 95–100 %**, **for music 0–20 %**.
- **Treble for music −6 dB** for light hiss, **−12 to −18 dB** for heavy hiss.
- **Bandwidth 8 or 12 kHz.**

### Good signals, tuning, conversations

- **Fast** (real-time model, delay about 0.35 s). For tuning to a station and for contacts where the delay gets in the way.
- Then switch to **Quality** for weak passages.

### More comfortable over long sessions

The isolated radio voice is mono, dry and narrow-band, which is tiring over time. Two controls give it space:

- **Width (stereo) about 30 %:** the voice spreads across the stereo image, each frequency in its own direction. In mono it stays unchanged.
- **Reverb 15–25 %, reverb time 300–500 ms:** a small room around the voice. Longer times up to 1000 ms sound like a hall.

## Notes

- **If the voice sounds choppy or metallic,** lower voice extraction to 85–90 %. A little of the original makes it more natural.
- **Delay:** "Quality" takes about 1–2 s depending on the Mac; auto latency picks the shortest setting without dropouts. "Fast" takes about 0.35 s.
- **Voili Server** (web version in the browser) has the same sound controls. There the control is called "Mix" and counts the share of the original: mix 5 % equals voice extraction 95 %. The music switch corresponds to "Automatic".

## License and sources

Apache-2.0, see [LICENSE](LICENSE) and [NOTICE](NOTICE). Model: [Inworld Voice Isolator v1](https://github.com/inworld-ai/voice-isolator) (Apache-2.0), re-implemented in Rust with [Burn](https://github.com/tracel-ai/burn) on Metal. The licenses of all bundled Rust crates are in `THIRD-PARTY-LICENSES.txt` inside the app (Contents/Resources). The browser version is [Voili Server](https://github.com/HDZucht/voili-server).

---

© 2026 Hans-Dieter Zucht · Hans-Dieter Zucht (@HDZucht) and Argus (33feb1a0) (Claude Opus 5.5) — code suggestion with Claude Code
