---
title: "Byte-Level Tokenization Papers"
date: 2026-04-15
tags: [research, paper]
created: 2026-04-15
---

# Paper Memory Bank: Byte-Level Tokenization

Last updated: 2026-04-15

### [mrt5] Dynamic Token Merging for Efficient Byte-level Language Models
- **Authors**: Julie Kallini, et al.
- **Venue**: arXiv:2410.20771 (cs.CL), v3 Apr 2025
- **URL**: https://arxiv.org/abs/2410.20771
- **Citations**: N/A (recent)
- **Status**: discovered
- **Topics**: byte-level-efficiency, dynamic-merging, ByT5-extension, encoder-shortening
- **Abstract**: MrT5 (MergeT5) addresses the inefficiency of byte-level models (like ByT5) by adding a token deletion mechanism in the encoder that dynamically shortens input sequence length. After fixed encoder layers, a learned delete gate merges critical info from deleted tokens into retained ones. Achieves up to 75% sequence length reduction with minimal bits-per-byte degradation.
- **Notes**: Very relevant — directly addresses the core efficiency bottleneck of byte-level models.
---

### [spacebyte] SpaceByte: Towards Deleting Tokenization from Large Language Modeling
- **Venue**: NeurIPS 2024
- **URL**: https://proceedings.neurips.cc/paper_files/paper/2024/file/e1f418450107c4a0ddc16d008d131573-Paper-Conference.pdf
- **Status**: discovered
- **Topics**: tokenizer-free, byte-level-scaling, subword-alternative
- **Abstract**: Empirical study showing byte-level Transformer and MegaByte models require roughly 10x more training FLOPs to match subword model performance. Argues for deletion of tokenization but acknowledges the computational cost gap.
- **Notes**: Key reference for the FLOPs gap problem that approaches like MrT5 aim to close.
---

### [blt] Byte Latent Transformer: Patches Scale Better Than Tokens
- **Venue**: ACL 2025 (ACL-Long 453)
- **URL**: https://aclanthology.org/2025.acl-long.453.pdf
- **Status**: discovered
- **Topics**: byte-patching, hierarchical-bytes, scaling-law
- **Abstract**: Proposes using byte-level patches instead of tokens. Models at 1B-8B parameters with optimal patch sizes report improved bits-per-byte. The key insight is that patch-level processing closes the efficiency gap vs subword.
- **Notes**: Major recent work — patch-based approach is architecturally distinct from MrT5's token merging.
---

### [byt5] ByT5: Towards Byte-level Unified Speech Recognition Understanding
- **Authors**: Google Research
- **Venue**: arXiv / T5 paper extension
- **Status**: discovered
- **Topics**: byte-level-baseline, ByT5, subword-comparison
- **Abstract**: Google's byte-level T5 variant — the main baseline that subsequent byte-level efficiency work (MrT5, SpaceByte) tries to improve upon. Operates on raw UTF-8 bytes instead of subword tokens.
- **Notes**: Foundational reference. Not a recent paper but necessary for any byte-level survey.
---

### [utf8-illformed] Byte-level Tokenizers Unavoidably Enable LLMs to Generate Ill-formed UTF-8
- **Venue**: OpenReview (submitted to ICLR/NeurIPS track)
- **URL**: https://openreview.net/pdf?id=j2hH02UVch
- **Status**: discovered
- **Topics**: security, unicode, byte-tokenizer-bug, UTF-8-exploits
- **Abstract**: Shows that byte-level tokenizers enable LLMs to generate visually confusable Unicode strings and other UTF-8 exploits. References Davis & Suignard (2014) visual spoofing attacks.
- **Notes**: Security/robustness angle on byte-level tokenization — less about efficiency, more about output quality.
---

### [distill-byte] Distilling Token-Trained Models into Byte-Level Models
- **Venue**: arXiv:2602.01007
- **URL**: https://arxiv.org/html/2602.01007v1
- **Status**: discovered
- **Topics**: knowledge-distillation, token-to-byte, model-porting
- **Abstract**: Recipe for transferring knowledge from subword-tokenized models (Llama, Qwen, OLMo) into byte-level models. Two-stage approach referenced.
- **Notes**: Practical engineering angle — relevant if trying to convert existing subword models to byte-level.
---

### [retrofit] Retrofitting Large Language Models with Dynamic Tokenization
- **Venue**: ACL 2025 (ACL-Long 1444)
- **URL**: https://aclanthology.org/2025.acl-long.1444.pdf
- **Status**: discovered
- **Topics**: tokenizer-retrofitting, dynamic-tokenization, post-hoc-modification
- **Abstract**: Method for retrofitting existing subword-trained LLMs with dynamic/token-aware modifications. References tokenizer choice literature.
- **Notes**: More about modifying tokenization post-hoc than pure byte-level, but related to the retrofitting/distillation angle.
---

### [megatech] MegaByte: Towards Deleting Tokenization (byte-level autoregressive)
- **Venue**: Referenced in SpaceByte as prior byte-level approach
- **Status**: discovered
- **Topics**: byte-level-autoregressive, megabyte-model, tokenization-alternative
- **Abstract**: Prior work on byte-level autoregressive modeling; mentioned in SpaceByte as requiring 10x FLOPs vs subword.
- **Notes**: Historical reference. Not primary source — SpaceByte cites it as the baseline inefficiency problem.

---

### [mutant] MUTANT: A Recipe for Multilingual Tokenizer Design
- **Authors**: Souvik Rana, Arul Menezes, Ashish Kulkarni, Chandra Khatri, Shubham Agarwal
- **Venue**: arXiv:2511.03237 (COLM submission, v2 March 2026)
- **URL**: https://arxiv.org/abs/2511.03237
- **Citations**: N/A (recent)
- **Status**: discovered
- **Topics**: tokenization, multilingual-nlp, low-resource-languages, vocabulary-design, fertility-score, indic-languages
- **Abstract**: Presents MUTANT, a recipe for building multilingual tokenizers with careful vocabulary design, language-aware pre-tokenization, and subword/multiword aware training. MUTANT-Indic achieves 39.5% better fertility vs LLaMA4 and 18% vs Sutra, with 44% inference throughput improvement.
- **Notes**: Addresses fertility directly — same metric as Token Tax paper (Lundin et al., AfricaNLP 2026) for African languages. Language-aware pre-tokenization is complementary to byte-level approaches.
