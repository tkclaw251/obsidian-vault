---
title: "African NLP Papers"
date: 2026-04-25
tags: [research, african-nlp, papers]
created: 2026-04-25
---

# Memory Bank: African NLP

## Papers Discovered

### [dense-retrieval-african-backbones] On Backbones and Training Regimes for Dense Retrieval in African Languages
- **Authors**: Akintunde Oladipo, Mofetoluwa Adeyemi, Jimmy Lin
- **Venue**: SIGIR 2024
- **URL**: https://dl.acm.org/doi/10.1145/3626772.3657952
- **Citations**: N/A (recent, moderate)
- **Status**: discovered
- **Topics**: dense-retrieval, african-nlp, low-resource-languages, information-retrieval, multilingual-nlp, backbone-comparison, training-regimes
- **Abstract**: Compares mBERT vs. AfriBERTa vs. AfroXLMR as backbones for dense retrieval in African languages across different domains. Shows pre-training domain of the backbone LM plays a huge role in retrieval effectiveness, especially without retrieval training data. Introduces CIRAL benchmark.
- **Notes**: Key finding: pre-training domain > model architecture for low-resource African retrieval. Code: github.com/castorini/afridpr_backbones. Links to AfriTeVa and CIRAL literature.

### [afri-nlp-landscape] Charting the Landscape of African NLP
- **Authors**: Jesujoba Alabi et al.
- **Venue**: EMNLP 2025
- **URL**: https://arxiv.org/abs/2505.21315
- **Citations**: N/A (preprint, EMNLP 2025)
- **Status**: discovered
- **Topics**: african-nlp, multilingual-nlp, information-retrieval, tokenization, llms-for-african-languages
- **Abstract**: Survey of 884 papers on NLP for African languages over 5 years. Identifies key trends, progress across core tasks, and promising directions for inclusive NLP research.
- **Notes**: Primary landscape survey. Directly relevant to "Tokenizer Fairness for African Languages" and "Dense Retrieval for Low-Resource Languages" tracks.

### [llm-african-lang-state] The State of Large Language Models for African Languages: Progress and Challenges
- **Authors**: Kedir Yassin H et al.
- **Venue**: arXiv 2506.02280 (2025)
- **URL**: https://arxiv.org/abs/2506.02280
- **Citations**: N/A
- **Status**: discovered
- **Topics**: african-nlp, llms, tokenization-bias, low-resource
- **Abstract**: Compares African language coverage across 6 LLMs, 8 SLMs, 6 specialized SLMs. Identifies 42 supported vs 2000+ African languages. Notes tokenization bias as a primary challenge.
- **Notes**: Directly identifies tokenization bias as a core challenge. Good complement to landscape survey.

### [token-tax] The Token Tax: Systematic Bias in Multilingual Tokenization
- **Authors**: Jessica M. Lundin, Ada Zhang, Nihal Karim, Hamza Louzan, Guohao Wei, David Ifeoluwa Adelani, Cody Carroll
- **Venue**: AfricaNLP 2026 (ACL Workshop), pages 103–112
- **URL**: https://aclanthology.org/2026.africanlp-main.10/
- **Citations**: N/A (very recent, March 2026)
- **Status**: discovered
- **Topics**: tokenization-bias, african-nlp, multilingual-nlp, fairness, low-resource-languages
- **Abstract**: Evaluates 10 LLMs on AfriMMLU (5 subjects, 16 African languages). Shows token fertility reliably predicts accuracy. Reasoning models (DeepSeek, o1) outperform non-reasoning across all languages. Doubling tokens = 4x training cost.
- **Notes**: Bridge paper connecting byte-level efficiency work (BLT, MrT5) with African NLP fairness. Authors: Adelani is active in African NLP.

### [m3-embedding] M3-Embedding: Multi-Linguality, Multi-Functionality, Multi-Granularity Text Embeddings Through Self-Knowledge Distillation
- **Authors**: Jianlv Chen, Shitao Xiao, Peitian Zhang, Kun Luo, Defu Lian, Zheng Liu
- **Venue**: arXiv:2402.03216 (Feb 2024), BAAI / University of Science and Technology of China
- **URL**: https://arxiv.org/abs/2402.03216
- **Citations**: High (widely used as BGE-M3 in multilingual retrieval pipelines)
- **Status**: discovered
- **Topics**: dense-retrieval, multilingual-NLP, information-retrieval, self-knowledge-distillation, 100-languages
- **Abstract**: M3-Embedding (BGE-M3) supports semantic retrieval across 100+ languages. Uniquely supports dense retrieval, multi-vector retrieval (ColBERT-style), and sparse retrieval in a single model. Uses self-knowledge distillation to integrate signals across retrieval modes.
- **Notes**: Baseline model in WinLP 2025 Nigerian low-resource paper. 100+ language support addresses African language coverage gap. github.com/FlagOpen/FlagEmbedding.
