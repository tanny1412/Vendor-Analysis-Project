# Vendor Analysis Project

A full-cycle data analytics project for evaluating vendor performance, pricing strategy, and inventory efficiency across a beverage distribution business. The analysis covers **2.3M+ purchase records** and **12.8M+ sales transactions** spanning the 2024 calendar year.

---

## Overview

This project answers key procurement and inventory questions:

- Which vendors drive the most revenue and profit?
- Are we over-reliant on a small set of suppliers?
- Does bulk purchasing actually lower unit costs?
- How much capital is locked in slow-moving inventory?
- Do high-volume vendors offer better or worse margins?

The answer to the last question is counterintuitive — high-volume vendors operate at ~31% margins while low-volume vendors average ~41%, suggesting different business models rather than poor negotiation.

---

## Project Structure

```
├── ingestion_db.py                   # Load raw CSVs into SQLite
├── get_vendor_summary.py             # Build aggregated summary table via SQL
├── Exploratory Data Analysis.ipynb   # Initial data exploration
├── Vendor Performance Analysis.ipynb # Full analysis with visualizations
├── vendor_sales_summary.csv          # Output: master analytics table (10,692 rows)
├── Vendor Performance Report.pdf     # Executive summary report
├── vendor_performance.pbix           # Power BI interactive dashboard
└── logs/                             # Ingestion and processing logs
```

---

## Data

All raw data is stored in a local SQLite database (`inventory.db`) built from source CSVs.

| Table | Records | Description |
|---|---|---|
| `purchases` | 2,370,000+ | Individual purchase transactions by vendor, brand, date |
| `sales` | 12,820,000+ | Store-level sales with revenue and excise taxes |
| `vendor_invoice` | 5,543 | Aggregated invoices including freight costs |
| `purchase_prices` | 12,261 | Product pricing catalog |
| `begin_inventory` | 206,529 | Inventory snapshot — Jan 1, 2024 |
| `end_inventory` | 224,489 | Inventory snapshot — Dec 31, 2024 |
| `vendor_sales_summary` | 10,692 | Pre-aggregated output combining all tables |

---

## Workflow

```
Raw CSVs → ingestion_db.py → SQLite DB
                                  ↓
              get_vendor_summary.py (SQL aggregation + feature engineering)
                                  ↓
              vendor_sales_summary.csv (master table)
                                  ↓
              Jupyter Notebooks → PDF Report + Power BI Dashboard
```

---

## Key Metrics Computed

| Metric | Formula |
|---|---|
| Gross Profit | `TotalSalesDollars − TotalPurchaseDollars` |
| Profit Margin | `(GrossProfit / TotalSalesDollars) × 100` |
| Stock Turnover | `TotalSalesQuantity / TotalPurchaseQuantity` |
| Sales-to-Purchase Ratio | `TotalSalesDollars / TotalPurchaseDollars` |

---

## Key Findings

### Vendor Concentration
The top 10 vendors account for **65.69%** of total procurement. DIAGEO NORTH AMERICA INC alone contributes **$67.99M** in sales. Heavy reliance on a small supplier pool introduces supply chain risk.

### Bulk Purchasing Impact
Volume-based pricing shows a clear incentive gradient:

| Order Size | Avg Unit Price |
|---|---|
| Small | $39.06 |
| Medium | $15.49 |
| Large | $10.78 |

That's a **72% reduction** from small to large orders — the bulk pricing strategy is working.

### Unsold Inventory
**$2.71M** in capital is currently locked in slow-moving or unsold inventory. DIAGEO holds the largest share ($722K). Several vendors show stock turnover below 1.0, meaning they sold less than they purchased.

### Margin vs. Volume Trade-off
A two-sample t-test (p ≈ 0.0000) confirms that top-performing vendors (by sales volume) have **significantly lower profit margins** than low performers — 31% vs. 41%. High-volume vendors compete on scale, not margin.

---

## Statistical Analysis

- **95% Confidence Intervals** computed for profit margins across vendor tiers
- **Two-Sample T-Test** comparing top-quartile vs. bottom-quartile vendors by sales
- **Pareto / 80-20 Analysis** on vendor contribution to total purchases
- **Correlation heatmaps** across all numeric metrics

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python (pandas, numpy) | Data wrangling and feature engineering |
| SQLite + SQLAlchemy | Database storage and complex SQL aggregations |
| matplotlib + seaborn | Visualizations |
| scipy.stats | Hypothesis testing and confidence intervals |
| Jupyter Notebooks | Interactive analysis |
| Power BI | Executive dashboard |

---

## Setup

```bash
# Install dependencies
pip install pandas sqlalchemy matplotlib seaborn scipy

# Step 1: Ingest raw CSVs into SQLite
python ingestion_db.py

# Step 2: Build aggregated summary table
python get_vendor_summary.py

# Step 3: Open notebooks for analysis
jupyter notebook
```

> **Note:** Raw CSV data files are not included in this repository due to size. Place them in the `/data` directory before running ingestion.

---

## Outputs

- **`vendor_sales_summary.csv`** — Master analytics table ready for BI tools or further analysis
- **`Vendor Performance Report.pdf`** — Executive-ready summary with charts and recommendations
- **`vendor_performance.pbix`** — Power BI dashboard for interactive exploration
