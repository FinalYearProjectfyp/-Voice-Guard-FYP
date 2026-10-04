# VoiceGuard — AI-Cloned Voice Detection System

## 1. What It Is
VoiceGuard is a machine learning–based security system that analyzes recorded or real-time audio and classifies it as either a genuine human voice or an AI-generated/cloned one. It combines audio feature extraction (spectrograms, MFCCs, raw waveform) with a deep learning classifier trained to detect the subtle artifacts voice-cloning tools leave behind — artifacts usually inaudible to humans but statistically detectable. It returns a real/fake verdict with a confidence score, deliverable through a dashboard or, longer-term, an API.

## 2. What to Build
1. **Baseline** — train/evaluate RawNet2 or AASIST on the ASVspoof dataset; report accuracy, precision, recall, and Equal Error Rate (EER)
2. **Voice-cloning module** — generate controlled fake samples using Coqui TTS (XTTS-v2) / RVC with consenting participants, for extra training/test data
3. **Feature extraction** — spectrograms/MFCCs + raw waveform, using librosa/torchaudio
4. **Model improvement** — fine-tune Wav2Vec2/XLS-R and compare against the baseline
5. **Generalization tests** — cross-generator (train on one cloning tool, test on another) and cross-language (English → Urdu) — the project's core research contribution
6. **Real-time pipeline** — chunked (2–3s) streaming classification with a rolling confidence score
7. **Dashboard** — Streamlit app: upload/record audio, live real/fake indicator, confidence score
8. **Evaluation & documentation** — full metrics across voices, noise conditions, and both languages

## 3. The Problem
Voice-cloning tools can now recreate a person's voice convincingly from just a few seconds of sample audio. Fraudsters already use this for fake emergency calls, impersonation of executives to authorize fraudulent transfers, and fabricated statements attributed to public figures. AI-driven fraud attempts have risen sharply, and because a cloned voice can sound identical to the real one, human listeners are poorly equipped to catch it. Most phone systems, call centers, and everyday users have no automated way to verify whether a voice is genuinely human.

## 4. The Solution
VoiceGuard addresses this directly: it classifies audio as real or AI-generated using feature extraction combined with a deep learning classifier, surfacing a confidence score rather than just a binary flag, and — via a lightweight explainability layer — shows which part of the audio triggered the decision.

## 5. Gap vs. Existing Tools

**Hugging Face community demos**
- Inconsistent performance beyond known/trained cloning tools
- Lab-trained only — not tested on real-world/noisy/compressed audio
- Open but often broken/unmaintained
- No explainability, no Urdu/South Asian language coverage

**Resemble AI (Detect API)**
- Admitted weak point on unseen cloning tools
- Closed API — not reproducible for research
- Limited explainability
- No Urdu/South Asian language coverage

**AI or Not**
- Score-only, unverified accuracy claims
- Known weak point on real-world audio
- Closed, no explainability
- No Urdu/South Asian language coverage

**Pindrop**
- Claims strong generalization, but disputed in academic literature
- Strong on real-world audio, but closed/proprietary — not reproducible
- Not publicly explainable
- No Urdu/South Asian language coverage

**The consistent gap:** every existing tool is closed to inspection/retraining, lacks transparent cross-generator robustness results, or has never been evaluated on Urdu or any South Asian language. VoiceGuard closes all three gaps at once as an open, reproducible system explicitly tested across cloning tools and across English/Urdu.

## 6. Scope

**In scope — core**
- Real vs. AI-generated/cloned voice detection
- Baseline model on ASVspoof (public dataset)
- RawNet2/AASIST as the starting architecture
- Standard feature extraction (spectrograms/MFCC/raw waveform)
- Accuracy, precision, recall, EER as baseline benchmark

**In scope — extension**
- Fine-tuned Wav2Vec2/XLS-R improvement model
- Consent-based voice-cloning module for controlled test data
- Cross-generator and cross-language (English vs. Urdu) generalization testing
- Real-time, chunk-based detection pipeline
- Streamlit dashboard with live demo mode

**Out of scope**
- Fully deployed, production-ready commercial product
- Comprehensive coverage of every language/cloning tool
- Integration with real telecom/phone-call infrastructure
- Non-speech audio deepfakes (e.g., music)

## 7. Target Audience

**Primary**
- Banks and financial institutions concerned with voice-based fraud
- Call centers using or considering voice authentication
- Everyday phone users vulnerable to impersonation/emergency scam calls

**Secondary**
- Telecom providers, as a potential call-security layer
- Media/journalism organizations verifying audio authenticity
- Law enforcement and forensic investigators verifying voice evidence
- Audio security researchers, via the Urdu/cross-generator results

## 8. Research Paper Shortcomings 

1. **Yi et al. — Scale and Diversity of Anti-Spoofing Datasets:** only random data sampling was tested, not smarter selection strategies; model-capacity effects unexplored.
2. **Khan et al. — Voice Spoofing Countermeasures Survey:** no single feature/classifier combo was consistently best; authors call for more cross-corpus evaluation, explainability, and non-English/fairness-aware detection.
3. **Yi, Wang, Tao et al. — Audio Deepfake Detection Survey:** in-domain performance degrades sharply out-of-domain (EER up 2–52%); authors list multilingual datasets and interpretability as future directions.
4. **Müller et al. — Does Audio Deepfake Detection Generalize?:** models under 2% error on standard benchmarks saw error jump 200–1000% on real-world audio; field is over-tuned to ASVspoof rather than real-world conditions.
5. **Almutairi & Elgibreen — Review of Modern Audio Deepfake Detection:** almost no non-English research existed at review time; authors explicitly call for research on underrepresented languages and accent robustness.

These gaps collectively support VoiceGuard's focus on cross-generator robustness and Urdu-language coverage — areas the literature confirms are unaddressed.

## Tech Stack
Python, PyTorch, librosa, torchaudio, Streamlit, Git/GitHub
