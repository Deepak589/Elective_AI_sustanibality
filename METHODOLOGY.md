# Methodology — Auditing AI-Based ESG Scores (Stage 1)

Two source datasets (Kaggle): **Public Company ESG Ratings** (`rq1.csv`, ~722 firms; `rq3.csv`,
503 S&P 500 firms with risk scores) and **S&P 500 ESG Sustainability Reports** text
(`train/validation/test.csv`, `text,label`). Each RQ below: dataset → steps → table → figure.

Shared preprocessing: drop nulls, lowercase column names, min-max / z-score normalize numeric
scores, TF-IDF (1–2 grams, min_df=2) for text. All artifacts saved as `.csv` (tables) and `.pdf` (figures).

---

### RQ1 — Cross-rater divergence  ·  notebook `rq1_rater_divergence.ipynb`
1. Load per-firm scores. **Need ≥2 raters**; raw file has one, so build a shared-latent + idiosyncratic
   multi-rater panel (or plug a real merged file into `compute_divergence()`).
2. Min-max normalize each rater to 0–100.
3. Pairwise **Spearman ρ** matrix across raters; per-firm dispersion = Divergence Index.
4. Bucket firms Low/Moderate/High.
- **Table 1:** per-firm scores, divergence index, category. **Fig 1:** rater agreement heatmap.

### RQ2 — Adversarial robustness  ·  `rq2_adversarial_robustness.ipynb`  *(fully real)*
1. Train TF-IDF + Logistic Regression on `train.csv`; evaluate clean `test.csv`.
2. Apply 3 attacks: synonym swap (WordNet), char-typo, greenwash-buzzword injection.
3. Re-evaluate each; compute Accuracy/Precision/F1/AUC and % drop vs baseline.
- **Table 2:** model × attack metrics + drop. **Fig 2:** F1 by attack (grouped bar).

### RQ3 — Zero-knowledge audit  ·  `rq3_zk_audit.ipynb`
1. Train real surrogate ESG-risk classifier (RandomForest) on `rq3.csv` → accuracy.
2. ZK proof generation is a circuit task (`ezkl`/`snarkjs`) — **not** from CSV; attach
   literature-anchored proof-time/protocol stubs per model.
3. Map each model to regulation target + compliance status.
- **Table 3:** model, accuracy, proof time, protocol, compliance. **Fig 3:** accuracy vs proof-cost scatter.

### RQ4 — Geographic / linguistic fairness  ·  `rq4_multilingual_bias.ipynb`
1. Needs multilingual reports (Nano-ESG, Chinese set); English-only here → build labelled
   synthetic panel (or load real `rq4_multilingual_esg_fairness.csv`).
2. Per language/market: mean prediction bias, report completeness, keyword density.
3. **Disparate impact** = group keyword rate ÷ English rate (4/5ths rule, <0.8 = biased).
- **Table 4:** per-language bias + disparate impact. **Fig 4:** bias by language (dev vs emerging).

### RQ5 — Temporal concept drift  ·  `rq5_temporal_drift.ipynb`
1. Needs a `date`/`year` column. If present: train on early period, test on later → real F1 drift.
   Else inject labelled synthetic drift.
2. Windowed F1 per period; classify drift severity; flag continual-training fixes.
- **Table 5:** model × period F1, drift magnitude, class. **Fig 5:** F1 vs test period (line).

---

**Kaggle run:** upload the data files as a Dataset, attach to a notebook, Run All. Notebooks auto-find
inputs under `/kaggle/input/**` and write outputs to `/kaggle/working/`.
