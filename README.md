# Stage 1 deliverables — provenance (READ FIRST)

**Critical for publication:** tables/figures in `tables/` and `figures/` here are **SYNTHETIC
PLACEHOLDERS** for building the report/poster skeleton. They must NOT appear as findings in the
IEEE paper. Run the notebooks to get **computed** results, then swap those in.

| RQ | Notebook | What is REAL vs synthetic |
|----|----------|---------------------------|
| RQ1 | `rq1_rater_divergence.ipynb` | Synthetic multi-rater (raw = 1 rater). ρ tuned to ~0.54 (MIT). Plug real merged file to make it real. |
| RQ2 | `rq2_adversarial_robustness.ipynb` | **REAL** — trains on `train.csv`, attacks computed on `test.csv`. |
| RQ3 | `rq3_zk_audit.ipynb` | Surrogate accuracy REAL; proof times = literature stubs (ZK needs a circuit toolchain). |
| RQ4 | `rq4_multilingual_bias.ipynb` | Synthetic (data is English-only); disparate-impact formula real. |
| RQ5 | `rq5_temporal_drift.ipynb` | Synthetic drift (no timestamps); becomes real if you add a `date`/`year` column. |

**To make RQ1/RQ4 fully real you must upload:** multi-rater ESG panel (MSCI+S&P+Bloomberg+Refinitiv;
e.g. MIT Aggregate Confusion 924-firm set) and multilingual reports (Nano-ESG German, Chinese ESG set).

## Folders
- `tables/` — 5 synthetic CSV tables (table1..table5)
- `figures/` — 5 figures, each `.pdf` (vector, for LaTeX) + `.png` (preview)
- `notebooks/` — 5 Kaggle notebooks (raw data in → CSV + PDF out)
- `METHODOLOGY.md` — concise per-RQ pipeline
