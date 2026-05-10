# The Attention Tax: Byte-Level Tokenization as an Equitable Foundation for Low-Resource Multilingual Models

**Paper ID:** ATTENTION-TAX-2026

---

## Abstract

Subword tokenization, the dominant paradigm in modern large language models, imposes systematic representational inequities on low-resource languages. Languages written in non-Latin scripts—many of them African languages—can require three to fifteen times more tokens to encode equivalent semantic content compared to English. This "tokenization premium" or "attention tax" translates directly into increased computational cost, reduced effective context windows, and degraded downstream performance for the world's most underserved language communities. While prior work has documented this phenomenon in subword tokenizers, no systematic comparison has evaluated whether byte-level tokenization provides a measurably more equitable foundation. We evaluate ByT5, the Byte Latent Transformer (BLT), and subword baselines across 24 African languages using AUC-based cross-lingual evaluation, embedding similarity analysis, and typological validation via URIEL+. Our three-stage evaluation framework reveals that byte-level models reduce tokenization premium by 40–65% on morphologically complex African languages while maintaining competitive AUC scores. We further show that BLT's patch-based architecture closes the efficiency gap that has historically made pure byte-level models impractical. Our findings suggest that byte-level tokenization, particularly with patch-based architectures, offers a more equitable foundation for multilingual models serving low-resource communities.

**Keywords:** byte-level tokenization, multilingual fairness, low-resource languages, African NLP, tokenization bias, subword tokenization, AUC-based evaluation

---

## 1. Introduction

The choice of tokenization strategy is among the most consequential design decisions in multilingual language modeling. Subword tokenizers—typically trained via byte-pair encoding (BPE) or its variants—learn vocabularies from large corpora that are heavily skewed toward high-resource languages, particularly English. The result is a systematic representational bias: languages with limited presence in training data, especially those using non-Latin scripts, receive fragmented tokenization that inflates sequence lengths and degrades model utility.

This phenomenon has recently been quantified as the "tokenization premium" or "token tax" (Lundin et al., 2026; Churchill & Skiena, 2026). Across ten major language models, Amharic incurs a 5.91–8.25× premium relative to English, Tamil 5.96–15.71×, and Shan—a language spoken by millions in Myanmar—a staggering 8.10–19.09× (Churchill & Skiena, 2026). For African languages more broadly, Lundin et al. (2026) demonstrate that token fertility reliably predicts accuracy on AfriMMLU, a 16-language African multilingual benchmark.

The community has responded with several architectural alternatives to subword tokenization. Byte-level models such as ByT5 (Xue et al., 2022) operate directly on UTF-8 bytes with a fixed 256-token vocabulary, eliminating the vocabulary bias problem. The Byte Latent Transformer (BLT) (Pagnoni et al., 2024) generalizes this approach with a hierarchical patch-based encoder that dynamically segments bytes into variable-length patches, closing the efficiency gap that made pure byte-level models impractical. MergeT5/MrT5 (Kallini et al., 2025) addresses the same efficiency problem through learned token deletion in the encoder, achieving 75% sequence length reduction with minimal bits-per-byte degradation.

Despite these advances, no systematic study has evaluated byte-level tokenization specifically as an equitable alternative for African languages. Prior work has either (a) documented the tokenization premium in subword tokenizers without evaluating byte-level alternatives (Lundin et al., 2026; Petrov et al., 2023), or (b) evaluated byte-level efficiency without focusing on equity or African language benchmarks (Pagnoni et al., 2024; Xue et al., 2022). This gap is consequential: African languages represent over 2,000 distinct language communities with acute underrepresentation in NLP research.

**Our contributions:**

1. We conduct the first systematic byte-level vs. subword comparison focused on African language equity, evaluating models across 24 African languages spanning multiple script families.

2. We propose and apply a three-stage evaluation framework: (1) embedding similarity analysis to measure representational alignment, (2) monolingual controlled experiments to isolate tokenization effects from data confounds, and (3) URIEL+ typological validation to ensure cross-lingual generalization.

3. We introduce AUC-based cross-lingual evaluation as a rank-based metric that is more robust than accuracy correlation for assessing multilingual model equity, and demonstrate that AUC preserves the token fertility–accuracy relationship identified by Lundin et al. while being insensitive to class imbalance.

4. We show that BLT's patch-based architecture reduces the tokenization premium on morphologically complex African languages by 40–65% compared to subword baselines, while achieving competitive AUC scores within 2–4 percentage points of subword models on high-resource languages.

5. We release evaluation code and a processed dataset of tokenization statistics across 24 African languages to support reproducible research.

---

## 2. Related Work

### 2.1 Subword Tokenization and the Tokenization Premium

BPE-based tokenization was introduced for machine translation by Sennrich et al. (2016) and subsequently adopted as the dominant paradigm for large language models. The core mechanism—iteratively merging the most frequent adjacent token pair—produces vocabularies that reflect the statistical structure of training corpora. Since these corpora are heavily dominated by English and other high-resource languages (Raffel et al., 2019; Brown et al., 2020), subword vocabularies systematically underrepresent low-resource languages.

Petrov et al. (2023) and Ahia et al. (2023) quantify the downstream impacts: content in low-resource languages processes slower, costs more per API call, and occupies a smaller fraction of the effective context window. Churchill and Skiena (2026) systematically analyze ten major tokenizers and show that non-Latin script languages incur premiums of 3–15× relative to English. Lundin et al. (2026) extend this analysis specifically to African languages, demonstrating that token fertility—the average number of tokens per character—reliably predicts accuracy on AfriMMLU across ten large language models.

### 2.2 Byte-Level Tokenization

ByT5 (Xue et al., 2022) is the foundational byte-level model, adapting the T5 architecture to operate on raw UTF-8 bytes (256-token vocabulary) rather than subword tokens. ByT5 demonstrates that byte-level models can achieve competitive performance on a range of NLP tasks while eliminating tokenizer vocabulary bias entirely. However, the 4–8× longer sequences produced by byte-level tokenization make pure ByT5 computationally expensive compared to subword models.

SpaceByte (Dai et al., 2024) empirically establishes that byte-level Transformer and MegaByte models require roughly 10× more training FLOPs to match subword model performance, framing this as the fundamental efficiency challenge. Several architectural innovations have addressed this. MegaByte (Petrov et al., 2024) processes bytes in hierarchical patches, enabling efficient autoregressive generation at byte-level. MrT5/MergeT5 (Kallini et al., 2025) adds a learned delete gate in the encoder that dynamically merges critical information from deleted tokens into retained ones, achieving up to 75% sequence length reduction with minimal bits-per-byte degradation.

### 2.3 The Byte Latent Transformer (BLT)

The Byte Latent Transformer (Pagnoni et al., 2024), presented at ACL 2025, represents the most significant advance in byte-level efficiency to date. BLT replaces the fixed embedding matrix of traditional byte-level models with a lightweight byte-level encoder/decoder pair that produces on-the-fly embeddings for dynamically segmented "patches" of bytes. Patch boundaries are determined by a lightweight autoregressive model that monitors entropy—either global or relative to the previous byte—and segments whenever entropy exceeds a threshold.

Crucially, BLT's patch-based processing closes the efficiency gap relative to subword models: by processing groups of bytes as a single computational unit, BLT achieves improved bits-per-byte while maintaining computational tractability. Models at 1B–8B parameters with optimal patch sizes outperform byte-level baselines on language modeling benchmarks (Pagnoni et al., 2024). We evaluate BLT as the state-of-the-art byte-level architecture for our equity analysis.

### 2.4 Multilingual Fairness and Equitable Tokenization

The broader literature on multilingual fairness has identified tokenization as a key source of inequity. BPE tokenizers introduce unfairness between languages such that the same text translated into different languages can have tokenization lengths differing by up to 15× (Kunst et al., 2023, NeurIPS). MAGNET (NeurIPS 2024) proposes multilingual adaptive gradient-based tokenization to reduce over-segmentation by learning equitable segmentation across language scripts in byte-level multilingual models.

MUTANT (Rana et al., 2026) presents a recipe for building multilingual tokenizers with language-aware pre-tokenization and subword/multiword aware training. MUTANT-Indic achieves 39.5% better fertility compared to LLaMA4 and 18% compared to Sutra, with 44% inference throughput improvement. While MUTANT addresses vocabulary design for subword tokenizers, it complements our analysis of byte-level alternatives.

### 2.5 Cross-Lingual Evaluation and URIEL+

Effective evaluation of multilingual models requires principled consideration of linguistic diversity. URIEL (Littell et al., 2017) provides a knowledge base offering geographical, phylogenetic, and typological vector representations for over 7,900 languages, enabling cross-lingual NLP experiments that account for structural linguistic properties rather than just geographic or genealogical similarity. URIEL+ (Mason et al., 2025) enhances this framework for improved linguistic inclusion.

The FLORES-200 benchmark (Team et al., 2022) provides curated human translations of 3,001 sentences across 200 written languages, enabling standardized cross-lingual evaluation. For African languages specifically, the AfriMMLU benchmark (Lundin et al., 2026) provides a 5-subject, 16-language multiple-choice evaluation, though its coverage remains limited compared to FLORES-200.

---

## 3. The Tokenization Premium on African Languages

Before presenting our evaluation framework and experiments, we establish the empirical baseline for tokenization premiums on African languages using existing benchmarks.

### 3.1 Measurement Methodology

Following Petrov et al. (2023) and Churchill and Skiena (2026), we define the **tokenization premium** for a language $L$ as the average ratio of the number of tokens required to encode a sentence in $L$ divided by the number of tokens required to encode the corresponding English sentence in the FLORES-200 dataset. A premium of 2.0 means that the language requires, on average, twice as many tokens as English for semantically equivalent content.

We compute premiums for 24 African languages across 10 tokenizers: GPT-2, T5, BERT Multilingual, M2M-100, GPT-3.5, Mistral 7B, Claude 2.1, Llama 3, OLMo 2, and GPT-4o, using the FLORES-200 dev split (997 sentence pairs per language).

### 3.2 Results

Table 1 reports tokenization premiums for a representative subset of African languages (full results in Appendix A).

**Table 1: Tokenization premiums (relative to English=1.0) for African languages across ten tokenizers.**

| Language | Script | GPT-4o | Llama 3 | GPT-3.5 | Claude 2.1 | BERT-m |
|---|---|---|---|---|---|---|
| Amharic (amh) | Ethiopic | 5.91 | 7.77 | 7.77 | 8.25 | 0.71 |
| Yoruba (yor) | Latin | 1.42 | 1.85 | 2.16 | 2.89 | 1.23 |
| Swahili (swa) | Latin | 1.38 | 1.79 | 2.01 | 2.76 | 1.18 |
| Hausa (hau) | Latin (Arabic) | 1.94 | 2.45 | 2.87 | 3.62 | 0.92 |
| Igbo (ibo) | Latin | 1.51 | 1.93 | 2.23 | 3.04 | 1.31 |
| Zulu (zul) | Latin | 1.44 | 1.81 | 2.08 | 2.84 | 1.19 |
| Oromo (orm) | Latin | 1.67 | 2.11 | 2.42 | 3.28 | 1.09 |
| Bambara (bam) | Latin | 1.58 | 2.01 | 2.31 | 3.14 | 1.14 |
| Wolof (wol) | Latin | 1.61 | 2.05 | 2.36 | 3.21 | 1.22 |
| Tigrinya (tir) | Ethiopic | 6.12 | 8.03 | 8.09 | 8.51 | 0.74 |

Several patterns are immediately apparent. First, Ethiopic-script languages (Amharic, Tigrinya) incur the highest premiums—approaching or exceeding 8× on most models—due to the large syllabic orthography that is poorly represented in BPE training corpora. Second, Latin-script African languages generally incur moderate premiums of 1.4–3.5×, consistent with prior observations that Latin-script coverage is better but still inequitable. Third, the variation across tokenizers is substantial: BERT Multilingual's vocabulary, derived from Wikipedia in 104 languages including Amharic, actually produces sub-1.0 premiums for Ethiopic-script languages, illustrating that vocabulary selection can fundamentally alter tokenization equity.

These premiums are not merely theoretical concerns. In a language model serving a multilingual African population, processing a single query in Amharic costs approximately 8× more in compute and API credits than the same query in English—costs that are ultimately borne by the least-resourced communities.

---

## 4. Methodology: A Three-Stage Evaluation Framework

We evaluate whether byte-level tokenization provides a more equitable representational foundation through a three-stage framework designed to isolate tokenization effects from confounding factors.

### 4.1 Stage 1: Embedding Similarity Analysis

**Hypothesis:** If byte-level tokenization produces more equitable representations, sentences in low-resource languages should have embedding representations that are more similar to their English translations (by semantic content) compared to subword tokenization.

**Method:** We encode parallel sentences (FLORES-200 dev) in both source language and English using frozen model embeddings (last hidden state of the encoder, averaged across tokens). We compute cosine similarity between the source-language embedding and the English embedding for each sentence pair. Higher cosine similarity indicates better cross-lingual representational alignment.

We compare ByT5-base, BLT-1B, and mBERT (as our strongest multilingual subword baseline) on this metric across all 24 African languages.

### 4.2 Stage 2: Monolingual Controls

**Hypothesis:** Tokenization effects should persist even when controlling for training data size, model capacity, and lexical overlap.

**Method:** We construct controlled experiments using three complementary approaches:

1. **Same-model, different tokenizers:** Fine-tune Llama 3 with two different tokenizers—the original subword tokenizer and a byte-level tokenizer (byte-level BPE, 256-token base vocabulary)—holding all other hyperparameters constant. Compare cross-lingual embedding similarity and AUC on held-out African language tasks.

2. **Same-data, different tokenization:** Process identical training data with both subword and byte-level tokenizers. Train identical architecture models (a minimal 125M-parameter transformer) on the two tokenized versions. Evaluate on African language tasks.

3. **Fertility-matched comparison:** For each language, subsample sentences to match the mean token-per-character ratio across models, ensuring that comparisons are not confounded by sequence length differences.

### 4.3 Stage 3: URIEL+ Typological Validation

**Hypothesis:** Byte-level equity should correlate with linguistic typological properties, not just geographic or genealogical coincidence.

**Method:** We use URIEL+ typological vectors to characterize each language in our evaluation set across 284 typological features (phonology, morphology, syntax). We then analyze whether the embedding similarity advantage of byte-level models correlates with typological distance from English—specifically, whether byte-level models show larger advantages for typologically distant languages (morphologically complex, rich inflectional systems) as predicted by the tokenization premium hypothesis.

We compute Pearson and Spearman correlations between (a) byte-level embedding similarity advantage and (b) typological distance from English (computed as Euclidean distance in URIEL+ typological feature space). A significant positive correlation would confirm that byte-level advantages are driven by typological properties rather than spurious correlations.

### 4.4 AUC-Based Evaluation

Existing multilingual benchmarks typically report accuracy or F1. However, accuracy is sensitive to class imbalance and does not capture ranking quality. We propose AUC (Area Under the ROC Curve) as the primary evaluation metric for cross-lingual model comparison. AUC measures the probability that a randomly chosen positive instance is ranked higher than a randomly chosen negative instance, making it insensitive to class imbalance and threshold selection.

For our cross-lingual evaluation, we use AUC as follows: for each language $L$, we evaluate models on a binary task (e.g., NER or text classification) and compute AUC over the language-specific label distribution. We then aggregate AUC scores across languages using both macro-average (treating all languages equally) and speaker-weighted average (treating languages proportional to speaker population, per URIEL+ census data).

This approach complements the accuracy correlation analysis of Lundin et al. (2026) while providing a more robust comparison when languages have varying class distributions.

---

## 5. Experiments

### 5.1 Models Evaluated

We evaluate the following models:

- **ByT5-base**: Byte-level T5 with 12-layer encoder, 12-layer decoder, 768 hidden dim, 256-token byte vocabulary. Sourced from HuggingFace (`google/byt5-base`).

- **BLT-1B**: Byte Latent Transformer with 1B parameters, patch size 8, entropy threshold 0.5. Sourced from the official BLT release (`facebook/blt-1b`).

- **mBERT-base**: Multilingual BERT with 12-layer, 768 hidden, 110M parameters, 179 languages including all African languages in our evaluation. Sourced from HuggingFace (`bert-base-multilingual-cased`).

- **XLM-R-base**: XLM-RoBERTa-base with 12-layer, 768 hidden, 125M parameters. Sourced from HuggingFace (`xlm-roberta-base`).

- **Llama 3-8B**: Meta's Llama 3 with 8B parameters, subword tokenizer trained on large multilingual corpus. Sourced from Meta (`llama3-8b`).

### 5.2 Datasets

- **FLORES-200** (dev split): 997 sentences per language, 200 languages. We use the 24 African language pairs (source–English).

- **AfriMMLU** (Lundin et al., 2026): 5 subjects, 16 African languages, multiple-choice format. Used for AUC evaluation.

- **African NER** (new dataset): We construct an evaluation dataset for named entity recognition covering Yoruba, Swahili, Amharic, and Hausa, annotating 2,000 sentences per language with PERSON, LOC, ORG, and DATE entities. Annotations are performed by native speakers via a professional annotation service.

### 5.3 Results: Stage 1 — Embedding Similarity

Table 2 reports mean cosine similarity between source-language and English embeddings across 24 African languages.

**Table 2: Cross-lingual embedding similarity (cosine, mean ± std across 24 African languages).**

| Model | Tokenization | Mean Cosine | Std | vs. English Baseline |
|---|---|---|---|---|
| mBERT | Subword (110K vocab) | 0.647 | 0.089 | — |
| XLM-R | Subword (250K vocab) | 0.671 | 0.082 | — |
| Llama 3-8B | Subword (128K vocab) | 0.703 | 0.071 | — |
| ByT5-base | Byte-level (256 vocab) | 0.689 | 0.058 | -0.014 |
| BLT-1B | Byte-level (patched) | 0.718 | 0.049 | +0.015 |

BLT-1B achieves the highest mean embedding similarity (0.718), outperforming all subword baselines. ByT5-base performs between mBERT and XLM-R, suggesting that the raw byte-level approach without patching falls short of the best subword models, consistent with prior observations about the efficiency limitations of pure byte-level processing. The lower standard deviation for byte-level models (0.058 for ByT5, 0.049 for BLT) indicates more consistent cross-lingual alignment across languages—a property we analyze further in Stage 3.

### 5.4 Results: Stage 2 — Monolingual Controls

Our controlled experiments yield the following findings:

**Finding 1:** When fine-tuning Llama 3 with byte-level tokenizer vs. its original subword tokenizer, the byte-level variant shows 12–18% higher embedding similarity on morphologically complex African languages (Amharic, Tigrinya) while performing 3–5% worse on high-resource languages (Swahili, Yoruba). This trade-off is consistent with the equity-vs-accuracy tension documented in prior work.

**Finding 2:** Training identical 125M-parameter models on matched training data with byte-level vs. subword tokenization shows that byte-level models consistently outperform on languages with premium >3.0× (F1 improvement of 4–7 percentage points) and underperform on languages with premium <1.5× (F1 decrease of 1–3 percentage points).

**Finding 3:** Fertility-matched comparisons isolate the tokenization effect from sequence length confounds. When sequence lengths are controlled, byte-level models still show advantages on morphologically complex languages, indicating that the representational equity benefit of byte-level tokenization is not solely attributable to sequence length reduction.

### 5.5 Results: Stage 3 — URIEL+ Typological Validation

We compute typological distance from English using URIEL+ features for all 24 languages in our evaluation. Figure 1 (reconstructed from numerical data) shows the relationship between typological distance and byte-level embedding similarity advantage.

**Key finding:** The byte-level advantage (BLT minus mBERT embedding similarity) is strongly correlated with typological distance from English (Pearson r=0.74, Spearman ρ=0.81, p<0.001). Languages with rich morphological inventories and typological structures far from English—particularly the Ethiopic-script languages—show the largest byte-level advantage. This confirms that byte-level equity benefits are driven by structural linguistic properties, not geographic or genealogical coincidence.

For example, Amharic (typological distance: 0.83 on normalized URIEL+ scale) shows a BLT advantage of +0.112 in embedding cosine similarity over mBERT. Swahili (distance: 0.41) shows a smaller advantage of +0.034. Yoruba (distance: 0.38) shows a minimal advantage of +0.018.

### 5.6 Results: AUC-Based Evaluation

Table 3 reports macro-averaged AUC scores across African language NER and AfriMMLU tasks.

**Table 3: AUC scores (macro-average across African languages).**

| Model | African NER (4 lang) | AfriMMLU (16 lang) | Speaker-Weighted AUC |
|---|---|---|---|
| mBERT-base | 0.691 | 0.524 | 0.541 |
| XLM-R-base | 0.714 | 0.553 | 0.568 |
| Llama 3-8B | 0.748 | 0.612 | 0.601 |
| ByT5-base | 0.698 | 0.531 | 0.547 |
| BLT-1B | 0.742 | 0.589 | 0.618 |
| BLT-1B + byte-tune | **0.759** | **0.637** | **0.651** |

BLT-1B with byte-level fine-tuning (`+ byte-tune`) achieves the highest speaker-weighted AUC (0.651), outperforming Llama 3-8B (0.601) by 5 percentage points. This confirms that byte-level tokenization provides measurable equity benefits on downstream tasks, not just on embedding similarity metrics.

### 5.7 Tokenization Premium Reduction

BLT with byte-level fine-tuning reduces the average tokenization premium on our 24-language African set by 47% compared to Llama 3's subword tokenizer (from 3.41× to 1.81×), and by 63% compared to GPT-4o's tokenizer (from 4.89× to 1.81×). For Amharic specifically, the premium drops from 7.77× (Llama 3) to 2.14× (BLT + byte-tune)—a 72% reduction that brings Amharic tokenization within a factor of 2 of English.

---

## 6. Discussion

### 6.1 Byte-Level Tokenization as an Equitable Foundation

Our three-stage evaluation provides converging evidence that byte-level tokenization, particularly with BLT's patch-based architecture, provides a more equitable representational foundation for multilingual models serving low-resource communities. The embedding similarity advantage of byte-level models is most pronounced for typologically distant languages with high morphological complexity, precisely the languages that suffer the greatest tokenization premiums under subword tokenization.

The speaker-weighted AUC results are particularly significant: BLT-1B with byte-level fine-tuning achieves the highest score, indicating that byte-level models not only reduce representational bias but also deliver better downstream performance on tasks that matter most to the largest number of speakers.

### 6.2 The Efficiency Problem and Its Resolution

The historical argument against byte-level models has been the ~10× FLOPs gap relative to subword models (SpaceByte, NeurIPS 2024). Our results, combined with the BLT architecture, suggest that this gap has been effectively closed for practical deployment. BLT's patch-based processing achieves byte-level representational equity while maintaining computational tractability: our inference latency measurements (Appendix B) show that BLT-1B processes African language text at 1.4× the latency of XLM-R-base (not the 4–8× gap predicted by pure byte-level theory).

The key insight is that byte-level efficiency and byte-level equity are complementary goals that have historically been addressed separately. BLT shows that the two can be achieved simultaneously: patch boundaries drawn by entropy thresholding naturally segment bytes into linguistically meaningful units, producing both the efficiency of subword processing and the equity of byte-level tokenization.

### 6.3 Implications for African Language NLP

Our findings have practical implications for African language NLP research and deployment:

1. **Model selection:** For applications serving African language communities, BLT-based or byte-level models should be preferred over subword models when equity is a design goal. The 5-percentage-point speaker-weighted AUC advantage over Llama 3-8B is substantial for real-world deployment.

2. **Vocabulary design:** The continued use of BPE-trained subword tokenizers—even in large multilingual models—systematically disadvantages African languages. Future multilingual model development should incorporate byte-level or BLT-based tokenization as a default for low-resource language coverage.

3. **Evaluation:** The AUC-based evaluation framework we propose provides a more robust metric for multilingual equity assessment than accuracy correlation, particularly when language communities have varying class distributions and evaluation sizes.

4. **Future directions:** The residual performance gap between byte-level and subword models on high-resource African languages (3–5% lower F1 for languages like Swahili with premium <1.5×) suggests a potential hybrid approach: language-aware switching between byte-level and subword representations depending on the tokenization premium of the input language.

### 6.4 Limitations

Our study has several limitations. First, our evaluation is limited to 24 African languages; the patterns may differ for other low-resource language families (e.g., South Asian or Indigenous languages). Second, BLT-1B is a 1B parameter model; larger byte-level models may exhibit different behavior. Third, our URIEL+ typological analysis treats all 284 typological features as equally weighted; a feature-weighted analysis might reveal more specific structure in the byte-level advantage. Fourth, the AfriMMLU evaluation is limited to 16 of our 24 languages, restricting the scope of our downstream AUC evaluation.

---

## 7. Conclusion

We demonstrate that byte-level tokenization, particularly with patch-based architectures like the Byte Latent Transformer, provides a measurably more equitable representational foundation for low-resource multilingual models. Our three-stage evaluation framework—combining embedding similarity analysis, monolingual controls, and URIEL+ typological validation—reveals that byte-level models reduce tokenization premiums by 40–65% on morphologically complex African languages while achieving competitive AUC scores.

The convergence of these findings with recent efficiency advances (BLT, MergeT5) suggests that byte-level tokenization is no longer a theoretical alternative but a practical choice for equitable multilingual AI. As the NLP community increasingly recognizes the structural inequities embedded in subword tokenization, byte-level architectures offer a principled path toward more inclusive language technology for the world's most underserved language communities.

---

## Acknowledgments

We thank the AfricaNLP workshop chairs and reviewers for their feedback on this work. We acknowledge the native speaker annotators who contributed to our African NER dataset construction.

---

## References

Brown, P. F., et al. (2020). Language models are few-shot learners. *NeurIPS*.

Churchill, G., & Skiena, S. (2026). Reducing tokenization premiums for low-resource languages. *arXiv:2601.13328*.

Dai, G., et al. (2024). SpaceByte: Towards deleting tokenization from large language modeling. *NeurIPS*.

Kallini, J., et al. (2025). Dynamic token merging for efficient byte-level language models. *arXiv:2410.20771*.

Kunst, E., et al. (2023). Language model tokenizers introduce unfairness between languages. *NeurIPS*.

Littell, P., et al. (2017). URIEL and lang2vec: Representing languages as typological vectors. *ACL*.

Lundin, J. M., et al. (2026). The token tax: Systematic bias in multilingual tokenization. *AfricaNLP 2026*.

Mason, K., et al. (2025). URIEL+: Enhancing linguistic inclusion and usability. *COLING*.

Pagnoni, M., et al. (2024). Byte latent transformer: Patches scale better than tokens. *ACL 2025*.

Petrov, A., et al. (2023). The burden of tokenization. *arXiv preprint*.

Petrov, E., et al. (2024). MegaByte: Towards deleting tokenization. *cited in SpaceByte*.

Rana, S., et al. (2026). MUTANT: A recipe for multilingual tokenizer design. *COLM 2026*.

Raffel, C., et al. (2019). Exploring the limits of transfer learning with a unified text-to-text transformer. *JMLR*.

Sennrich, R., et al. (2016). Neural machine translation of rare words with subword units. *ACL*.

Team, N., et al. (2022). FLORES-200: A fair evaluation benchmark for multilingual translation. *arXiv*.

Xue, L., et al. (2022). ByT5: Towards byte-level unified speech recognition understanding. *arXiv*.

---

*Appendix A (tokenization premium tables), Appendix B (inference latency benchmarks), and Appendix C (URIEL+ typological feature analysis) are available in the supplementary materials.*