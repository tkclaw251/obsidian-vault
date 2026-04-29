---
title: "Technique Registry"
date: 2026-04-25
tags: [research, technique-registry]
created: 2026-04-25
---

# Technique Registry

**Purpose:** Accumulated record of techniques discovered by research-scout across nexus cycles. Used by research-analyst for synthesis.

---

## 2026-04-18 (Week of Apr 12–18)

**Papers scored this week:** 2
**Sources:** reading-log.md (african-nlp tracks), byte-level-tokenization memory bank

### New Techniques Added

| # | Technique | Category | Source | Cross-Domain Value |
|---|---|---|---|---|
| 1 | Patch-Based Byte Processing (BLT) | Byte-level efficiency | ACL 2025 | Multilingual fairness, retrieval, model efficiency |
| 2 | Token Merging / Delete Gate (MrT5) | Byte-level efficiency | arXiv 2410.20771 | Low-resource deployment, mobile |
| 3 | Tokenizer-Free Baseline (SpaceByte/MegaByte) | Byte-level efficiency | NeurIPS 2024 | FLOPs gap framing |
| 4 | Subword → Byte Distillation | Knowledge transfer | arXiv 2602.01007 | Model porting, retrofit existing models |
| 5 | Dynamic Tokenization Retrofit | Post-hoc tokenization | ACL 2025 (Long 1444) | Fast iteration on tokenization strategies |
| 6 | Byte-Level UTF-8 Security Mitigations | Security/robustness | OpenReview | Production deployment safety |
| 7 | Tokenization Bias Quantification (African LLMs) | Fairness/measurement | arXiv 2506.02280, EMNLP 2025 | African NLP gap documentation |

---

## 2026-04-25 (Week of Apr 19–25)

**Papers scored this week:** 6
**Sources:** reading-log.md (SIGIR 2024, EMNLP 2025, AfricaNLP 2026, arXiv Apr 2026)

### New Techniques Added

| # | Technique | Category | Source | Cross-Domain Value |
|---|---|---|---|---|
| 8 | CIRAL Benchmark (Cross-Lingual IR for African Languages) | Benchmark / Evaluation | SIGIR 2024 (Oladipo et al.) | Enables rigorous evaluation of cross-lingual/bi-text retrieval for African languages |
| 9 | Pre-Training Domain Overrides Architecture (Dense Retrieval) | Empirical finding / Training regime | SIGIR 2024 (Oladipo et al.) | Reframes backbone selection: domain alignment > model architecture in low-resource retrieval |
| 10 | Multi-Modal Self-Knowledge Distillation (BGE-M3) | Training methodology / Multi-task | arXiv:2402.03216 (Chen et al.) | Jointly trains dense + multi-vector + sparse retrieval via self-distillation. Unified embeddings across retrieval modes. |
| 11 | Few-Shot → Fine-Tuning Cascade for African LLM Adaptation | Training / Prompting strategy | arXiv:2604.00706 (AfrIFact, Apr 2026) | Two-stage adaptation: few-shot prompting (+43%) then fine-tuning (+26%) on African language fact-checking |
| 12 | Reasoning Models Narrow African Language Accuracy Gaps | Empirical finding / LLM comparison | AfricaNLP 2026 (token-tax) | DeepSeek-R1/o1 outperform non-reasoning models on African languages, narrowing historical gaps |

### Key Insight
Two converging threads this week:
1. **Retrieval pipeline matures** — BGE-M3 + CIRAL benchmark + AfriBERTa/AfroXLMR backbone comparisons suggest the low-resource African retrieval stack is solidifying around domain-aligned backbones + multilingual embeddings + cross-lingual evaluation benchmarks.
2. **Byte-level gap confirmed** — No paper this week tested byte-level/tokenizer-free approaches on African language data. Tokenization bias is now well-documented (token-tax, llm-african-lang-state) but solutions remain unvalidated in this domain.

Suggested synthesis angle: combine BGE-M3's multi-modal self-distillation approach with byte-level inputs — would the retrieval quality gains from multi-modal distillation transfer to byte-level embeddings for African languages?

### Week-over-Week Context
- Byte-level literature (T1–T6 from Apr 18): BLT, MrT5, SpaceByte/MegaByte, Subword→Byte Distillation, Dynamic Tokenization Retrofit — all mature but **not yet validated on African languages**
- African NLP fairness (T7 from Apr 18): tokenization bias documented; now reinforced by token-tax (T12 above)
- Retrieval pipeline (T8–T11 this week): mature benchmarks and backbones; cross-lingual failure confirmed (T11)
- Reasoning models (T12 this week): suggest compute-scaling may be a parallel path to language equity — byte-level efficiency fits into this narrative as reducing per-token cost
- No overlap yet between byte-level techniques (T1–T6) and African language evaluation (T8–T13)
