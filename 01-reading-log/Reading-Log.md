---
title: "Reading Log"
date: 2026-04-22
tags: [research, reading-log]
created: 2026-04-22
---

# Reading Log — Research Scout

Last updated: 2026-04-22

---

## 2026-04-22

### [dense-retrieval-african-backbones] On Backbones and Training Regimes for Dense Retrieval in African Languages
- **Authors**: Akintunde Oladipo, Mofetoluwa Adeyemi, Jimmy Lin
- **Venue**: SIGIR 2024
- **URL**: https://dl.acm.org/doi/10.1145/3626772.3657952
- **Citations**: N/A (recent, moderate)
- **Status**: discovered
- **Topics**: dense-retrieval, african-nlp, low-resource-languages, information-retrieval, multilingual-nlp, backbone-comparison, training-regimes
- **Abstract**: Compares mBERT vs. AfriBERTa vs. AfroXLMR as backbones for dense retrieval in African languages across different domains. Shows that pre-training domain of the backbone LM plays a huge role in retrieval effectiveness, especially in the absence of retrieval training data. Introduces CIRAL benchmark for cross-lingual IR evaluation in African languages.
- **Score**: 5/8
- **Relevance**: Bridge paper — connects dense retrieval literature with African NLP specificity. Provides empirical guidance on backbone selection for low-resource African retrieval.
- **Notes**: Authors are from Waterloo (Jimmy Lin group). CIRAL test collection available. Key finding: pre-training domain matters more than model architecture for low-resource retrieval.

---

## 2026-04-16

### [afri-nlp-landscape] Charting the Landscape of African NLP: Mapping Progress and Shaping the Road Ahead
- **Authors**: Jesujoba Alabi et al.
- **Venue**: EMNLP 2025
- **URL**: https://arxiv.org/abs/2505.21315
- **Citations**: N/A (preprint, EMNLP 2025)
- **Status**: discovered
- **Topics**: african-nlp, multilingual-nlp, information-retrieval, tokenization, llms-for-african-languages
- **Abstract**: Survey of 884 papers on NLP for African languages over 5 years. Identifies key trends, progress across core tasks, and promising directions for inclusive NLP research.
- **Score**: 5/8
- **Relevance**: Directly addresses "Tokenizer Fairness for African Languages" and "Dense Retrieval for Low-Resource Languages" tracks.
- **Notes**: High-value landscape survey. Supplement with per-topic deep dives.

### [llm-african-lang-state] The State of Large Language Models for African Languages: Progress and Challenges
- **Authors**: Kedir Yassin H et al.
- **Venue**: arXiv 2506.02280 (2025)
- **URL**: https://arxiv.org/abs/2506.02280
- **Status**: discovered
- **Topics**: african-nlp, llms, tokenization-bias, low-resource
- **Abstract**: Compares African language coverage across 6 LLMs, 8 SLMs, 6 specialized SLMs. Identifies 42 supported vs 2000+ African languages. Notes tokenization bias as a primary challenge.
- **Score**: 5/8
- **Relevance**: Strong for tokenizer fairness track — explicitly identifies tokenization bias as a core challenge for African languages.
- **Notes**: Complements the landscape survey. Good background for the "attention tax" framing.

---

## 2026-04-20

### [token-tax] The Token Tax: Systematic Bias in Multilingual Tokenization
- **Authors**: Jessica M. Lundin, Ada Zhang, Nihal Karim, Hamza Louzan, Guohao Wei, David Ifeoluwa Adelani, Cody Carroll
- **Venue**: AfricaNLP 2026 (ACL Workshop)
- **URL**: https://aclanthology.org/2026.africanlp-main.10/
- **Citations**: N/A (very recent, March 2026)
- **Status**: discovered
- **Topics**: tokenization-bias, african-nlp, multilingual-nlp, fairness, low-resource-languages
- **Abstract**: Evaluates 10 LLMs on AfriMMLU (5 subjects, 16 African languages). Shows token fertility reliably predicts accuracy — higher fertility = lower accuracy. Doubling tokens = 4x training cost. Motivates morphologically-aware tokenization for equitable NLP.
- **Score**: 5/8
- **Relevance**: Bridge paper — connects byte-level efficiency literature (BLT, MrT5) with African NLP fairness literature. Documents that tokenization bias is measurable and has direct accuracy/cost consequences.
- **Notes**: Authors include David Ifeoluwa Adelani (active in African NLP community). Reasoning models (DeepSeek, o1) outperform non-reasoning peers even on low-resource languages.

---

## 2026-04-23

### [afrifact] AfrIFact: Cultural Information Retrieval, Evidence Extraction and Fact Checking for African Languages
- **Authors**: Israel Abebe Azime et al. (including David Ifeoluwa Adelani, Jesujoba Alabi)
- **Venue**: arXiv:2604.00706 (April 2026)
- **URL**: https://arxiv.org/abs/2604.00706v1
- **Status**: discovered
- **Topics**: african-nlp, information-retrieval, low-resource-languages, fact-checking, cross-lingual-retrieval, multilingual-nlp
- **Abstract**: Introduces AfrIFact dataset covering information retrieval, evidence extraction, and fact checking in 10 African languages. Shows even the best embedding models lack cross-lingual retrieval capabilities for African languages. Few-shot prompting improves AfriqueQwen-14B by up to 43%, fine-tuning further improves by 26%.
- **Score**: 6/8
- **Relevance**: Directly connects to dense retrieval + African NLP. Key finding: best embedding models still lack cross-lingual retrieval for African languages.
- **Notes**: Authors include leading African NLP researchers. Directly connects to dense retrieval track.

---

## 2026-04-24

### [trans-tokenization] Trans-Tokenization and Cross-Lingual Vocabulary Transfers: Language Adaptation of LLMs for Low-Resource NLP
- **Authors**: François Remy, Pieter Delobelle, H. Avetisyan, A. Khabibullina, Miryam de Lhoneux, Thomas Demeester
- **Venue**: COLM 2024
- **URL**: https://arxiv.org/abs/2408.04303
- **Citations**: 41
- **Status**: discovered
- **Topics**: tokenization, low-resource-NLP, multilingual-NLP, cross-lingual-transfer, vocabulary-transfer, LLM-adaptation
- **Abstract**: Introduces trans-tokenization — cross-lingual vocabulary transfer strategy that adapts a high-resource monolingual LLM to a target low-resource language by initializing token embeddings via weighted averages of semantically similar source-language tokens. Validated with Tweeties trans-tokenized LLMs across diverse languages. Introduces Hydra LLMs with swappable embedding tables.
- **Score**: 7/8
- **Relevance**: Bridge paper — connects tokenizer-free/byte-level literature (BLT, MrT5) with cross-lingual vocabulary transfer for low-resource languages. The Hydra LLM design could be a model-level implementation path for tokenizer fairness.
- **Notes**: Authors include Miryam de Lhoneux (Uppsala, active in low-resource NLP). Accepted at COLM 2024. Code/data available.
