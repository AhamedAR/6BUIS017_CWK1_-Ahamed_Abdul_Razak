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
| [Wikipedia – List of S&P 500 companies](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies) | Current constituent list (503 tickers) and GICS sectors | 23 November 2026 |
| Yahoo Finance via `yfinance` (`auto_adjust=False`) | Daily Close, Adjusted Close and Volume for each share, and the ^GSPC benchmark | 23 November 2026 |

- **Study period:** 1 January 2021 to 1 January 2025 (end date exclusive), giving 1,005 trading days.
- **Observation date (RFM):** 31 December 2024.
- **Shares analysed:** 486 of 503. Seventeen were excluded for no data, an incomplete price history or volume errors (see `exclusion_log.csv`).

---

## How to run

1. Open `w2052031_Ahamed_Abdul_Razak.ipynb` in **Google Colab** (File → Upload notebook, or open it from GitHub).
2. Click **Runtime → Run all**.
3. The last cell zips the outputs and downloads `cwk1_all_outputs.zip`, `cwk1_data.zip` and `requirements.txt`.

No login, API key or Google Drive access is needed.

### Automatic cached-data fallback

The notebook first tries to download live data from Yahoo Finance. **If live access fails**, it automatically:

1. uses `cwk1_data.zip` if it already exists in the Colab session, otherwise
2. downloads `cwk1_data.zip` from this repository:
   `https://raw.githubusercontent.com/AhamedAR/6BUIS017_CWK1_-Ahamed_Abdul_Razak/main/cwk1_data.zip`

It then continues the analysis with the same 2021–2024 data. The data source used is printed in cell 3.

**Note:** Yahoo Finance may slightly revise historical adjusted prices over time (for example after new dividends), so a fresh live download can differ a little from the report. To reproduce the reported figures exactly, use the cached dataset.

---

## Method summary

| Step | Method |
|---|---|
| Validation | Duplicate dates, missing values, non-positive prices, zero/negative volume, unequal histories and corporate-action checks. Missing prices are never filled. |
| Weekly returns | Friday-ending weeks (`W-FRI`), last available Adj Close; incomplete first/final weeks removed; 207 weekly returns per share |
| Weekly beta | Covariance of share and ^GSPC weekly returns ÷ variance of ^GSPC returns (full period, 207 aligned weeks) |
| High-volume day | Volume > 2 × mean volume of the previous 20 sessions (the day itself excluded) |
| Recency (R) | Calendar days from the latest high-volume day to 31 Dec 2024 |
| Frequency (F) | Number of high-volume days in the latest 60 trading sessions |
| Monetary (M) | Sum of Close × Volume over the latest 60 sessions (approximate traded value, USD) |
| Feature preparation | log1p applied to M; each feature matrix standardised separately |
| Clustering | Ward linkage (Euclidean) and K-means (k-means++, `n_init=50`, `random_state=42`); k compared from 2 to 8; k = 3 selected for all four solutions |
| Sensitivity | Correlations among R, F, log1p(M); Ward repeated with R and F only, compared using the adjusted Rand index |

---

## Limitations

- **Survivorship bias:** the current constituent list is applied to 2021–2024 data, so companies that left the index during that period are missing.
- **Price basis:** share returns use adjusted prices, while ^GSPC is a price index without dividends.
- **Descriptive only:** the clusters describe 2021–2024 behaviour. They are not forecasts of future returns and do not by themselves show reduced portfolio risk.
- **Liquidity proxy:** Monetary is approximate traded value only; spreads and price impact are not measured.

---
