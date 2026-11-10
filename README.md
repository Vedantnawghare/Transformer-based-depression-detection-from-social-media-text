<div align="center">

# 🧠 Transformer-Based Depression Detection from Social Media

### User-level analysis with contextual language representations

![Research](https://img.shields.io/badge/Research-Depression%20Detection-6C63FF?style=for-the-badge)
![NLP](https://img.shields.io/badge/NLP-Transformers-2563EB?style=for-the-badge)
![Experiments](https://img.shields.io/badge/Experiments-28-0F766E?style=for-the-badge)
![Explainable AI](https://img.shields.io/badge/Explainable%20AI-SHAP%20%7C%20LIME-F59E0B?style=for-the-badge)

*A controlled study of transformer architectures and text representations for user-level depression detection from social-media timelines.*

</div>

---

## ✨ Overview

This project investigates **user-level depression detection from social-media timelines**. Instead of classifying individual posts independently, it considers the collected posts associated with each user.

The study evaluates four text-representation conditions across seven transformer architectures, producing **28 experimental configurations**. To reduce direct diagnostic leakage, explicit first-person diagnostic and self-disclosure posts are removed before the representation conditions are constructed. The framework also integrates **53 user-level metadata features** through gated multimodal fusion.

## 🎯 Research Objectives

- Compare alternative ways of representing social-media text.
- Evaluate seven transformer architectures under a common protocol.
- Investigate the use of complementary user-level metadata.
- Reduce direct diagnostic leakage through diagnostic-anchor removal.
- Evaluate classification performance and inspect selected configurations using explainability methods.

## 🧪 Experimental Design

### Text representation conditions

| Condition | Description |
|---|---|
| **RAW / Minimal** | Retains most original social-media characteristics, including punctuation, emojis, hashtags, mentions, capitalization, repeated characters and URLs. |
| **Aggressive** | Applies extensive text normalization and removes many social-media artifacts. |
| **Selective** | Normalizes selected content while retaining potentially meaningful emotional, psychological and stylistic information. |
| **Emotion-Based** | Adds an auxiliary representation derived from 11 emotion probabilities, projected to 64 dimensions and supplied through a separate fusion branch. |

### Transformer architectures

| Model | Description |
|---|---|
| BERT | General-purpose contextual encoder |
| DistilBERT | Computationally compact transformer |
| Sentence-BERT | MPNet-based representation model |
| MentalBERT | Mental-health-domain pretrained model |
| EmoBERTa | Emotion-oriented RoBERTa-based model |
| Longformer | Sparse-attention architecture |
| BigBird | Sparse-attention architecture |

**4 representation conditions × 7 transformer models = 28 configurations.**

### User-level metadata

The framework extracts 53 metadata features across seven groups.

| Feature group | Features |
|---|---:|
| Behavioral | 8 |
| Linguistic | 4 |
| Readability | 3 |
| Emotional | 6 |
| Psychological | 7 |
| Writing style | 5 |
| LDA topic distributions | 20 |
| **Total** | **53** |

Metadata is standardized using training-set statistics and integrated with the text representation through gated multimodal fusion. The Emotion-Based condition includes an additional emotion branch.

## 📊 Dataset and Evaluation

The experiments use the **MDDL (Matter about Depression) user-level social-media dataset**.

| Partition | Total users | D2 (negative) | D1 (positive) |
|---|---:|---:|---:|
| Training | 6,135 | 4,122 | 2,013 |
| Validation | 767 | 515 | 252 |
| Test | 767 | 516 | 251 |
| **Total** | **7,669** | **5,153** | **2,516** |

The partitions are fixed and mutually exclusive at the user level, preventing posts from the same user from appearing across different partitions.

**Diagnostic-anchor control:** 1,920 anchor records were removed, corresponding to 1,847 unique `(username, tweet_id)` pairs.

**Input length:** All seven models use a maximum input length of **512 tokens** in the reported 28-configuration experiments. These experiments therefore do not measure the effect of increasing Longformer or BigBird input length to 4,096 tokens.

Evaluation metrics include **Accuracy, Precision, Recall, F1-score, ROC-AUC**, and confusion-matrix analysis.

## 🏆 Key Results

Held-out test performance for the **RAW representation condition**:

| Transformer | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| 🥇 **Sentence-BERT** | **92.96%** | **87.45%** | **91.63%** | **89.49%** | 97.44% |
| **MentalBERT** | 92.83% | 87.40% | 91.24% | 89.28% | **97.68%** |
| BERT | 89.05% | 77.93% | 92.83% | 84.73% | 96.83% |
| Longformer | 89.05% | 79.51% | 89.64% | 84.27% | 96.17% |
| BigBird | 88.14% | 74.84% | **96.02%** | 84.12% | 96.49% |
| EmoBERT | 88.79% | 80.44% | 86.85% | 83.52% | 95.38% |
| DistilBERT | 87.74% | 80.08% | 83.27% | 81.64% | 94.60% |

<blockquote>
<strong>📌 Main findings</strong>

- Sentence-BERT + RAW achieved the highest RAW test accuracy (**92.96%**) and F1-score (**89.49%**).
- MentalBERT + RAW achieved a closely comparable F1-score (**89.28%**) and the highest RAW ROC-AUC (**97.68%**).
- BigBird achieved the highest recall (**96.02%**) but lower precision (**74.84%**).
- The best representation varies by transformer architecture; no single representation is uniformly optimal.
</blockquote>

**Across all 28 configurations:** Sentence-BERT + RAW achieved the highest F1-score (**89.49%**). Longformer + Selective achieved the highest ROC-AUC (**97.89%**), with **92.31% accuracy** and **88.72% F1-score**.

## 🔍 Explainability and Representation Analysis

The research uses complementary methods to inspect selected configurations:

| Method | Purpose |
|---|---|
| **SHAP** | Examines feature contributions to predictions. |
| **LIME** | Explains individual predictions using local approximations. |
| **t-SNE** | Visualizes learned representation structure. |
| **Grad-CAM-style token sensitivity** | Examines token-level sensitivity in selected configurations. |

These methods complement predictive metrics; they are not clinical explanations or evidence of causal relationships.

## 🗂️ Suggested Repository Structure

```text
.
├── README.md
├── notebooks/
│   ├── bert/
│   ├── distilbert/
│   ├── sentence_bert/
│   ├── mentalbert/
│   ├── emoberta/
│   ├── longformer/
│   └── bigbird/
├── data/
│   └── README.md
├── results/
│   ├── metrics/
│   ├── confusion_matrices/
│   └── training_curves/
├── explainability/
│   ├── shap/
│   ├── lime/
│   ├── tsne/
│   └── token_sensitivity/
├── figures/
└── requirements.txt
```

*This is a suggested organization. Adjust folder names to match the files actually included in your repository.*

## ⚙️ Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>
```

### 2. Create a Python environment

```bash
python -m venv .venv
```

**Windows**
```bash
.venv\Scripts\activate
```

**macOS / Linux**
```bash
source .venv/bin/activate
```

### 3. Install dependencies

If your repository includes a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

The exact dependency versions and notebook execution commands should be added based on the actual project environment; they are not specified in the paper.

### 4. Prepare the dataset

Place the dataset where the notebooks or data-loading scripts expect it. The dataset is not included in this README. Follow its applicable access conditions and do not upload private or restricted data to a public repository.

### 5. Run the experiments

Open the relevant notebooks and execute the preprocessing, feature extraction, transformer representation, fusion, training, and evaluation steps. Use the fixed user-level partitions to reproduce the reported setup.

## ⚠️ Limitations and Responsible Use

- The experiments use one user-level social-media dataset; results may not generalize to other platforms, populations, languages, or cultural contexts.
- All models use a 512-token maximum input length in the reported 28-configuration matrix.
- This system performs computational pattern detection and **does not provide a clinical diagnosis**.
- Predictions should not replace professional mental-health assessment or be used as the sole basis for decisions about individuals.
- Respect dataset access conditions, privacy, consent, and platform terms when reproducing or extending this work.

## 👥 Authors

**Dr. Ruchi Patel · Harshini Bonde · Vedant Nawghare · Shravani Sadawarte**

**Symbiosis Institute of Technology, Nagpur, India**

## 📄 Research Paper

**Transformer-Based Depression Detection from Social Media Text Using Contextual Language Representations**

When the final paper PDF is added to the repository, link it here:

`[Read the research paper](./<paper-filename>.pdf)`

## 📌 Citation

Replace this placeholder with the final published bibliographic details when available:

```text
Patel, R., Bonde, H., Nawghare, V., Sadawarte, S.
"Transformer-Based Depression Detection from Social Media Text
Using Contextual Language Representations."
Symbiosis Institute of Technology, Nagpur, India.
```

---

<div align="center">

**Research in contextual NLP, social-media analysis, and responsible AI.**

⭐ If this repository is useful to your work, consider giving it a star.

</div>
