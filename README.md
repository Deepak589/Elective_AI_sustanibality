# ESG Audit — Results Summary

**Auditing AI-Based ESG Scores: How much can you trust a machine-generated sustainability score?**

Two public datasets used across all five research questions:
- **Public Company ESG Ratings** — 503 S&P 500 firms with environment, social, governance and controversy risk scores, sector, and headcount.
- **S&P 500 ESG Report Sentences** — ~2,600 labelled sentences from sustainability reports used for text-robustness tests.

## Five Research Questions & Real Results

| RQ | Question | Key Finding |
|----|----------|-------------|
| RQ1 | Do a rater's own ESG sub-scores agree on which firm is better? | Across 430 firms the four risk dimensions rank companies almost independently — average Spearman **ρ = 0.12**. A single headline ESG number hides real internal disagreement. |
| RQ2 | Can adversarial or greenwashing text fool an ESG classifier? | Synonym swaps and typos leave F1 near **0.72** (baseline). Greenwashing injection drops F1 to **0.53** — a **27% drop**. The cheapest attack does the most damage. |
| RQ3 | Can a closed model be audited without handing over its weights? | A surrogate reproduces vendor risk levels at **88% accuracy** from scores alone — enough to check a closed model behind a zero-knowledge proof. Proof times are from the ZK literature, not measured here. |
| RQ4 | Do the scores quietly penalise some sectors or firm sizes? | Energy and Utilities firms almost never reach the low-risk band (disparate-impact ratio ≈ **0.05**, well below the 0.8 threshold). Split by headcount there is no disparate impact — the unfairness is **sectoral, not size-based**. |
| RQ5 | Does a model still work when the kind of firm it sees changes? | Stable on average, but hold out Financial Services or Real Estate and elevated-risk detection **collapses to zero F1**. |

## Takeaway

AI-based ESG scores are useful but fragile. Within one rater the dimensions disagree, text models can be gamed by greenwashing, whole sectors get penalised, and performance can vanish on firms the model hasn't seen enough of. Before scores like these steer real capital, they need the same routine auditing expected of any high-stakes model.

## Data & Results Provenance

### Input datasets
| File | Status |
|------|--------|
| `rq3.csv` | **REAL** — actual S&P 500 ESG risk scores from Sustainalytics/Yahoo Finance (1,634 firms) |
| `train.csv` / `test.csv` / `validation.csv` | **REAL** — actual labelled ESG report sentences (~2,600 total) |

### Result tables
| File | Status | Notes |
|------|--------|-------|
| `table1_rq1_dimension_divergence.csv` | **REAL** | 403 named S&P 500 firms; divergence index computed from `rq3.csv` sub-scores (Environment, Social, Governance, Controversy) |
| `table2_rq2_robustness.csv` | **REAL** | TF-IDF+LR model trained on `train.csv`, attacked on `test.csv`; all 4 rows are computed |
| `table3_rq3_zk_audit.csv` | **MIXED** | Row 1 (Surrogate-RF, 87.1% accuracy) is real — computed from `rq3.csv`. Rows 2–6 (FinBERT-ESG, ESG-BERT, LLaMA-ESG, MSCI Proxy, Bloomberg AI) are **literature stubs** — those models were not run; proof times and metrics are sourced from published papers |
| `table4_rq4_sector_size_fairness.csv` | **REAL** | Disparate-impact ratios computed from `rq3.csv` using the 4/5ths rule |
| `table5_rq5_distribution_shift.csv` | **REAL (proxy)** | F1 scores computed via sector-holdout cross-validation on `rq3.csv`; no timestamps in the data so sector is used as a stand-in for temporal distribution shift |

## Files
- `esg_audit_all_RQ.ipynb` — full notebook reproducing all results
- `fig1_rq1_dimension_divergence`, `fig2_rq2_robustness`, … — figures as `.pdf` (vector) + `.png`
- `METHODOLOGY.md` — concise per-RQ pipeline
