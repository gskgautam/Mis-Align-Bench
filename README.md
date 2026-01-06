
## Repository Structure

The repository contains the following files:

- `dataset_with_responses.csv`  
  The final SaVaCu dataset containing prompts paired with aligned and misaligned responses, along with their inferred safety, value, and cultural domains.

- `before_generation_count.csv`  
  Domain-wise statistics showing the number of prompts after classification, expansion, and deduplication, prior to response generation.

- `after_generation_count.csv`  
  Domain-wise statistics showing the number of retained aligned–misaligned response pairs after candidate filtering and feedback-guided resampling.


## 🧩 Benchmark Overview

Mis-Align-Bench is constructed via a **two-stage pipeline**:

### **Stage I — Dataset Construction (SaVaCu)**  
Builds **SaVaCu**, a curated English dataset of prompts paired with *aligned* and *misaligned* responses grounded in:

- **Safety** (BeaverTails taxonomy)
- **Value** (ValueCompass taxonomy)
- **Cultural** (UFCS taxonomy)

This stage consists of:
1. **Query Construction**: Multi-domain classification, conditional prompt expansion, and SimHash-based deduplication.
2. **Response Generation**: Controlled response generation followed by two-stage rejection sampling and feedback-guided resampling.

### **Stage II — Benchmarking**
Evaluates LLMs in a **zero-shot setting** on SaVaCu, measuring how well models handle *joint alignment constraints* without access to domain labels or reference responses.

---

## 📚 Dataset: SaVaCu

- **Total instances:** 382,424  
- **Domains:** 112 (14 safety, 56 value, 41 cultural)
- **Each instance contains:**
  - Prompt
  - Aligned response
  - Misaligned response
  - Inferred domain labels (hidden during evaluation)

### Dataset Splits
- 80% training
- 10% validation
- 10% test

The final dataset is stored in:
- `dataset_with_responses.csv`

---

## 🔎 Prompt Sources

- **LLM-Prompt-Dataset**  
  Large-scale, naturally occurring user prompts  
  📄 Zhang et al., 2025

---

## 🧠 Models Used

### Classification & Evaluation
- **Mistral-7B-Instruct-v0.3** — multi-domain prompt classification  
- **LLaMA-3.1-8B-Instruct** — conditional prompt expansion and automated alignment evaluation

### Response Generation Pool
- **Gemma-3-27B**
- **Phi-3-14B**
- **Qwen-2.5-32B**
- **Mistral-8×22B**

> Classification, generation, and evaluation models are *strictly separated* to avoid circular bias.

---

## 📊 Evaluation Metrics

Mis-Align-Bench reports three dataset-level alignment metrics:

- **Coverage (Cov)**  
  Fraction of truly misaligned outputs correctly flagged

- **False Failure Rate (FFR)**  
  Fraction of aligned outputs incorrectly flagged as misaligned

- **Alignment Score (AS)**  
  Harmonic mean of `Coverage` and `(1 − FFR)`

All metrics are **micro-averaged** and computed fully automatically.

---

## ⚙️ Key Methodological Details

- **Multi-label domain assignment** with probability threshold δ = 0.5  
- **SimHash deduplication** with Hamming threshold τ = 10  
- **Response generation:** temperature 0.7, top-p 0.9, max length 512  
- **Feedback-guided resampling:** max 2 iterations per query  
- **Benchmarking:** zero-shot evaluation, 3 runs averaged  
- **Infrastructure:** PyTorch 2.3, 4×A100 80GB GPUs, mixed precision  

---

## 📈 Statistics Files

- `before_generation_count.csv`  
  Domain-wise prompt counts after classification and expansion

- `after_generation_count.csv`  
  Final retained aligned–misaligned pairs after filtering and resampling

These files enable transparent analysis of dataset growth, filtering rates, and long-tail balancing.

---

## 🎯 Research Use Cases

Mis-Align-Bench supports:
- Studying **cross-domain misalignment**
- Evaluating **dimension-specific vs joint alignment strategies**
- Benchmarking **open-weight vs aligned LLMs**
- Analyzing **false positives in safety/value/cultural enforcement**

---

## ⚠️ Notes on Evaluation

- No human annotation is used during benchmarking.
- Model outputs are evaluated in raw form (no post-filtering).
- The benchmark focuses on *exposing misalignment*, not annotation validity.

---

## 📜 License & Ethics

This dataset is constructed entirely from **synthetic model-generated content** and publicly available prompts.  
It contains **no personal data** and is intended strictly for **alignment research and evaluation**.

Responsible use is strongly encouraged, especially when deploying alignment systems informed by this benchmark.
