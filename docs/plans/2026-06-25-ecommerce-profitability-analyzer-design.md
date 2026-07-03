# E-commerce Profitability Analyzer — Design Document

**Date:** 2026-06-25  
**Goal:** Train data analysis skills (career advancement + practical application)  
**Complexity:** Intermediate

---

## Overview

A Jupyter-based analysis pipeline that ingests raw transaction, product, and marketing data, cleans/joins everything into an analytical model, and produces a structured report answering 3 core business questions:

1. **True product profitability** — not just revenue minus COGS, but factoring in returns, refunds, and marketing cost allocation
2. **Customer segment economics** — RFM segmentation (Recency, Frequency, Monetary) plus cohort retention analysis
3. **One actionable recommendation** — backed by the data, e.g. "stop advertising Product X on Channel Y" or "discount overstock in Category Z"

---

## Tech Stack

- **Python + pandas** for ETL and analysis (Jupyter notebook as the workspace)
- **SQLite** as the analytical database (SQL joins/aggregations for practice)
- **Matplotlib + seaborn** for static charts (optionally Plotly for interactive)
- **Clean project structure:** `data/`, `notebooks/`, `src/` (reusable Python modules), `output/`

---

## Data Model

6 tables with realistic patterns baked in:

### Tables

| Table | What it holds | Key columns |
|---|---|---|
| **products** | Product catalog (~200 SKUs) | name, category, unit_cost, unit_price, supplier, is_active |
| **customers** | Customer accounts (~5,000) | name, email, acquisition_date, acquisition_channel, city/state |
| **orders** | Order headers (~20,000) | customer_id, order_date, status (completed/cancelled/returned), payment_method, shipping_cost |
| **order_items** | Line items per order | order_id, product_id, quantity, unit_price_at_time, discount_pct |
| **returns** | Return events | order_item_id, return_date, reason (damaged/wrong_item/not_needed/quality), refund_amount |
| **marketing_spend** | Daily ad costs per channel | date, channel (google/facebook/email/tiktok), campaign_name, spend, impressions, clicks |

### Realistic Patterns

- **Seasonality:** sales spike in Nov/Dec, slump in Jan/Feb
- **Customer tiers:** 20% of customers drive 60% of revenue (Pareto distribution)
- **Return-rate variation:** some products return at 2%, others at 18% (clothing vs. consumables)
- **Diminishing marketing returns:** each channel has a different CAC curve
- **Price changes over time:** `unit_price_at_time` captures historical pricing

### Data Generation

- **Path A (chosen):** Generate realistic synthetic data with Python
- **Tools:** `pandas` or `polars`, `numpy`, and `Faker` for names/addresses
- Output as CSV files into `data/raw/`
- A single `generate_data.py` script with a fixed seed for reproducible data

---

## Project Structure (planned)

```
data-analytics/
├── data/
│   ├── raw/          # Generated CSV files
│   └── processed/    # Cleaned/joined datasets
├── notebooks/
│   ├── 01_data_generation.ipynb
│   ├── 02_etl_and_cleaning.ipynb
│   ├── 03_product_profitability.ipynb
│   ├── 04_customer_segmentation.ipynb
│   └── 05_report_and_recommendations.ipynb
├── src/
│   ├── config.py
│   ├── database.py
│   └── helpers.py
├── output/
│   └── charts/
├── docs/
│   └── plans/
└── README.md
```

---

## Key Analysis Deliverables

1. **Product profitability** — margin analysis with returns and marketing cost allocation
2. **RFM segmentation** — Recency, Frequency, Monetary analysis + customer cohorts
3. **Marketing ROI** — cost per acquisition by channel, diminishing returns curves
4. **Executive summary** — top-line findings and one data-backed recommendation

---

## Status

- [x] Design finalized
- [ ] Data generation script
- [ ] ETL pipeline
- [ ] Product profitability analysis
- [ ] Customer segmentation
- [ ] Marketing ROI analysis
- [ ] Final report & recommendation
