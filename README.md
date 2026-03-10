# MSIN0097 Predictive Analytics Individual Assignment

This repository contains the implemented end-to-end workflow for the coursework using the Conjura MMM dataset.

## Business Question

Primary decision in notebook 02:
- If additional marketing budget is available, which channels should receive incremental spend?
- What percentage split should be allocated across channels under the modeled scenario?

## Implemented Workflow

### Notebook 01: Data Cleaning, Processing, and EDA
File: `notebooks/01_data_cleaning_eda.ipynb`

Implemented sections:
- data loading and type standardization
- data quality audit (schema, missingness, duplicates, time coverage)
- feature cleaning and aggregated driver creation
- target distribution and outlier review
- time-based EDA and relationship analysis
- segment-level EDA
- time-series continuity checks
- leakage and modelling risk review
- cleaned dataset export for modelling handoff

Output handoff:
- cleaned dataset saved to `dataset/conjura_mmm_data_cleaned.parquet`

### Notebook 02: Revenue Forecasting and Channel Recommendation
File: `notebooks/02_revenue_forecasting_channel_recommendation.ipynb`

Implemented sections:
- load cleaned dataset
- choose one defensible comparable time series
- time-aware feature engineering
- chronological train/validation/test split
- model training and comparison
- test evaluation with metrics and residual diagnostics
- +10% channel scenario simulation
- channel ranking by incremental efficiency (`uplift_per_extra_spend`)
- budget allocation table with percentage split and amount
- final business recommendation and caveats

## Models Used in Notebook 02

- `seasonal_naive_lag_7` (baseline)
- `ridge`
- `elasticnet`
- `bootstrapped_ridge`

Current saved run in notebook 02 selected:
- `best_validation_model`: `bootstrapped_ridge`
- `simulation_model`: `bootstrapped_ridge`

## Evaluation Setup

- Primary selection metric: `RMSE` on validation split
- Supporting metrics: `MAE`, `MAPE`, `R2_log_scale`
- Additional diagnostics: actual-vs-predicted plot, residual-over-time plot, residual distribution

## Repository Structure

```text
.
|-- README.md
|-- requirements.txt
|-- dataset.zip
|   |-- conjura_mmm_data.csv
|   |-- conjura_mmm_data_dictionary.xlsx
|   `-- conjura_mmm_data_cleaned.parquet (generated)
|-- notebooks/
|   |-- 01_data_cleaning_eda.ipynb
|   `-- 02_revenue_forecasting_channel_recommendation.ipynb
```

## How to Run

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter lab
```

Run in order:
1. `notebooks/01_data_cleaning_eda.ipynb`
2. `notebooks/02_revenue_forecasting_channel_recommendation.ipynb`

## Conclusion

This project delivers a complete workflow from data cleaning/EDA (notebook 01) to time-series forecasting and incremental budget recommendation (notebook 02).

From the current saved run, `bootstrapped_ridge` is selected as the best validation model and is used for scenario simulation. The resulting channel split is a directional recommendation for **additional budget allocation**, with `META_OTHER_SPEND` ranked highest in marginal efficiency for this run.

The recommendation should be treated as model-guided evidence rather than a final fixed policy. Before real budget reallocation, apply business constraints (for example channel min/max caps) and validate with controlled experiments.

Current allocation output in notebook 02 (`total_future_budget = 100000`):
- `META_OTHER_SPEND`: `83.4501%` (`83,450.1384`)
- `META_FACEBOOK_SPEND`: `8.1498%` (`8,149.7963`)
- `META_INSTAGRAM_SPEND`: `5.5677%` (`5,567.7058`)
- `GOOGLE_PMAX_SPEND`: `2.8324%` (`2,832.3596`)
- `GOOGLE_PAID_SEARCH_SPEND`: `0.0000%`
- `TIKTOK_SPEND`: `0.0000%`
- `GOOGLE_SHOPPING_SPEND`: `0.0000%`
- `GOOGLE_DISPLAY_SPEND`: `0.0000%`
- `GOOGLE_VIDEO_SPEND`: `0.0000%`

## AI/Agent Disclosure

Agent usage and verification decisions are documented in:
- `references/agent-usage-log.md`
