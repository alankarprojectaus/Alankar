# Decoding Alankars in Hindi Poetry: A Multi-Label Classification Approach using Hybrid Models

> **B.Tech Final Year Project** · Department of Computer Science and Engineering  
> Triguna Sen School of Technology, Assam University, Silchar — 788011  
> May 2026

---

### Developed by

- @Sagar-Dewanjee
- @Madapa-Mithra
- @Gedela-Bala-Sai-Prakash

**Supervisors:** Dr. Sourish Dhar · Mr. Vishal Gour  
Department of Computer Science and Engineering, Assam University, Silchar

---

This repository contains the complete pipeline for **automated Alankar detection in Hindi poetry** using a **Multi-Label Classification** approach. It includes the raw metadata corpus, the annotated HiPoAD dataset across three configurations, and all model training notebooks spanning five architectures.

It supports **Hindi Devanagari text input**, **multi-label prediction** of seven Alankar types, and a **sliding-window corpus generation** strategy that scales a 1,825-poem corpus to ~589,779 labelled samples. The full annotated dataset (HiPoAD_annotated) is uploaded in .csv.gz format as their is size limit in github.

---

## 📌 Features

### Dataset — HiPoAD (Hindi Poetry Alankar Dataset)
-  1,825 Hindi poems collected from Hindwi.org, published collections & textbooks
-  Manually annotated across 7 Alankar categories by human experts
-  Sliding-window line-group strategy expands corpus to ~589,779 labelled samples
-  Three dataset configurations: D20 (20k n-gram), D33 (33k single-line), D589 (full 589k n-gram) [D589 datset is in .csv.gz format as their is size limit in github]

### Models
-  Classical baselines: Naïve Bayes (NB-AlankarNet) and Decision Tree (DT-AlankarNet) with One-vs-Rest TF-IDF
-  Hybrid neural model: TF-POSNet combining TF-IDF features with Part-of-Speech tag vectors
-  Transformer models: MuRIL-AlankarNet (fine-tuned on 17 Indian languages) and IndicFusion-AlankarNet (IndicBERT + BiLSTM)
-  All models use sigmoid activation and binary cross-entropy for independent per-label prediction

---

## 📁 Directory Structure

```
📦 alankar-detection
┃
┣ 📂 dataset
┃ ┣ 📜 HiPoAD_metadata.xlsx              # Raw 1825-poem metadata (title, author, year, emotion, etc.)
┃ ┣ 📜 HiPoAD_annotated.xlsx             # Full annotated corpus with Alankar labels
┃ ┣ 📜 D20.xlsx                          # Compact 20k n-gram corpus
┃ ┣ 📜 D33.xlsx                          # 33k single-line corpus (window size = 1)
┃ ┗ 📜 D589.xlsx                         # Full 589k n-gram corpus
┃
┣ 📂 notebooks
┃ ┣ 📂 NB_AlankarNet
┃ ┃ ┣ 📓 NB_AlankarNet_D20.ipynb
┃ ┃ ┣ 📓 NB_AlankarNet_D33.ipynb
┃ ┃ ┗ 📓 NB_AlankarNet_D589.ipynb
┃ ┣ 📂 DT_AlankarNet
┃ ┃ ┣ 📓 DT_AlankarNet_D20.ipynb
┃ ┃ ┣ 📓 DT_AlankarNet_D33.ipynb
┃ ┃ ┗ 📓 DT_AlankarNet_D589.ipynb
┃ ┣ 📂 TF_POSNet
┃ ┃ ┣ 📓 TF_POSNet_D20.ipynb
┃ ┃ ┣ 📓 TF_POSNet_D33.ipynb
┃ ┃ ┗ 📓 TF_POSNet_D589.ipynb
┃ ┣ 📂 MuRIL_AlankarNet
┃ ┃ ┣ 📓 MuRIL_AlankarNet_D20.ipynb
┃ ┃ ┣ 📓 MuRIL_AlankarNet_D33.ipynb
┃ ┃ ┗ 📓 MuRIL_AlankarNet_D589.ipynb
┃ ┗ 📂 IndicFusion_AlankarNet
┃   ┣ 📓 IndicFusion_AlankarNet_D20.ipynb
┃   ┣ 📓 IndicFusion_AlankarNet_D33.ipynb
┃   ┗ 📓 IndicFusion_AlankarNet_D589.ipynb
┃
┗ 📜 README.md
```

---

## 🏷️ Alankar Categories

| # | Alankar | Script | Type | Description |
|---|---|---|---|---|
| 1 | Anupras | अनुप्रास | Shabdh | Alliteration — repetition of consonant sounds for melodic effect |
| 2 | Yamak | यमक | Shabdh | Same word used multiple times with different meanings |
| 3 | Shlesh | श्लेष | Shabdh | Single word carrying multiple simultaneous meanings (pun) |
| 4 | Upma | उपमा | Arth | Simile — explicit comparison using markers like *सा*, *जैसा* |
| 5 | Roopak | रूपक | Arth | Metaphor — direct identity without comparison words |
| 6 | Utpreksha | उत्प्रेक्षा | Arth | Imaginative comparison / poetic hypothesis (*मानो*, *जनु*) |
| 7 | Virodhabhas | विरोधाभास | Arth | Apparent contradiction that reveals a deeper truth |

---

## 🚀 How It Works

### Dataset Construction & Annotation
1. Collect raw Hindi poems with metadata (title, author, year, word count, emotion, noun count).
2. Annotate each poem manually for the 7 Alankar categories at the line-group level.
3. Apply sliding-window strategy — for a poem of N lines, window sizes 1 to N generate overlapping line groups, expanding the corpus to ~589,779 labelled samples.

### Preprocessing
1. Labels with nan as detected samples have been removed.
2. Tokenize and normalize whitespace.
3. Extract Hindi POS tags using the Stanza NLP pipeline (17 Universal POS categories).

### Model Training
1. Classical models (NB, DT) trained under One-vs-Rest (OvR) framework on TF-IDF features.
2. TF-POSNet: TF-IDF ⊕ POS vector → Dense(128) → Dropout → Dense(64) → Dropout → Sigmoid.
3. MuRIL: fine-tune `google/muril-base-cased` with mean pooling + linear classification head.
4. IndicFusion: frozen `ai4bharat/indic-bert` → BiLSTM(256) → masked mean pooling → linear head.
5. All models apply sigmoid per label; class imbalance handled via inverse-frequency weighting.

---

## 📊 Results

### Subset Accuracy (%) — All Models × All Dataset Configurations

| Model | D20 | D33 | D589 |
|---|---|---|---|
| NB-AlankarNet | 43.01 | 22.83 | 47.27 |
| DT-AlankarNet | 77.97 | 11.15 | 56.70 |
| **TF-POSNet** | **92.43** | 55.07 | **88.89** |
| MuRIL-AlankarNet | 34.95 | 72.18 | 78.32 |
| IndicFusion-AlankarNet | 59.41 | 38.97 | 72.10 |

> Single-line (D33) models consistently underperform n-gram configurations, confirming that multi-line contextual overlap is essential for reliable Alankar detection.

---

## 📜 License & Usage

**Authors:** Sagar Dewanjee, Madapa Mithra, Gedela Bala Sai Prakash  
**Institution:** Assam University, Silchar — Department of Computer Science and Engineering

This repository is submitted as a B.Tech final year academic project. The HiPoAD dataset and associated code are made available for academic and non-commercial research purposes.

You **may**:
- ✅ View and study the code and methodology
- ✅ Cite this work in academic publications
- ✅ Build upon the dataset for non-commercial research with attribution

You **may not**:
- ❌ Copy, clone, or reuse the code for commercial purposes
- ❌ Redistribute the HiPoAD dataset without attribution
- ❌ Claim this work as your own

---

## 📖 Citation

```bibtex
@bachelorsthesis{dhar2026alankars,
  title     = {Decoding Alankars in Hindi Poetry: A Multi-Label Classification
               Approach using Hybrid Models},
  author    = {Dr.Sourish Dhar, Vishal Gour, Sagar Dewanjee, Madapa Mithra, Gedela Bala Sai Prakash},
  school    = {Assam University, Silchar},
  year      = {2026},
  month     = {May},
  type      = {B.Tech. Project Report},
  department= {Computer Science and Engineering}
}
```

---

🎯 *Designed for accurate, scalable, and reproducible multi-label detection of figurative language in Hindi poetry.*
