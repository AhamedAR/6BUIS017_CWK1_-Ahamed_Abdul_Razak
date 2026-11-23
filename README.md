# 6BUIS017W CWK1 – Market Analysis and Stock Segmentation Using RFM

**Module:** 6BUIS017W CRM & CM with Business Intelligence (University of Westminster / IIT)
**Student:** Ahamed Abdul Razak – UOW ID w2052031
**Submission:** Individual coursework 1 (CWK1), November 2026

This repository contains the Google Colab notebook, the cached dataset and all outputs for my CWK1 analysis. S&P 500 shares are grouped in two ways:

1. **Risk and return:** mean weekly return and weekly beta against the S&P 500 (^GSPC).
2. **Trading activity:** an adapted Recency, Frequency and Monetary (RFM) model built from unusually high trading-volume days.

Both groupings use Ward agglomerative clustering and K-means. The notebook then runs sensitivity checks (correlations and removing Monetary) and builds a proposed data mart.

---

## Repository contents

| File | Description |
|---|---|
| `w2052031_Ahamed_Abdul_Razak.ipynb` | The full analysis notebook (Tasks 1 to 3), with outputs |
| `cwk1_data.zip` | Cached dataset: daily share prices and the ^GSPC benchmark for the study period (CSV) |
| `cwk1_all_outputs.zip` | All outputs: feature table with cluster labels, inclusion/exclusion log, validation rules, run metadata, data mart tables and figures |
| `requirements.txt` | Python package versions used for the final run |
| `README.md` | This file |

### Inside `cwk1_data.zip`

| File | Contents |
|---|---|
| `data/sp500_stock_data_2021_2024.csv` | Adj Close, Close and Volume for all 503 listed tickers (1,005 trading days) |
| `data/sp500_benchmark_data_2021_2024.csv` | ^GSPC daily prices and volume for the same period |

### Inside `cwk1_all_outputs.zip` (main files)

| File | Contents |
|---|---|
| `feature_table_with_clusters.csv` | One row per included share: mean weekly return, beta, R, F, M, sector and the four cluster labels |
| `inclusion_log.csv` / `exclusion_log.csv` | All 503 tickers, included or excluded, with the reason |
| `validation_rules_summary.csv` | The validation and calculation rules applied |
| `worked_example_AAPL.csv` | Worked RFM example for one share |
| `run_metadata.json` | Retrieval date, study period, observation date, selected k, random seed, calculation version and package versions |
| `data_mart/` | Proposed data mart tables (facts and dimensions) |
| `figures/` | All charts used in the report |

---

## Data sources

| Source | What it provides | Retrieved |
|---|---|---|
| [Wikipedia – List of S&P 500 companies](https://en.wikipedia.org/wiki
