# Dissertation: Customer Segmentation and CLV Forecasting
## UCI Online Retail II Dataset (Dataset 502)

**Author:** Vishal Chalia
**Programme:** MSc Data Science, AI & Digital Business
**Institution:** Gisma University of Applied Sciences, Berlin
**Year:** 2025

---

## Overview

This repository contains all code, analysis, and outputs for my MSc dissertation:

> **"Beyond RFM: Integrating Return Behaviour and Channel Complexity for Enhanced Customer Segmentation and CLV Forecasting in E-Commerce"**

The study proposes an extended **RFMRV framework** (Recency, Frequency, Monetary Value, Cancellation Ratio, Basket Diversity Index, Cross-Channel Consistency Rate, Cost-Per-Engagement) applied to the UCI Online Retail II dataset to improve customer segmentation quality and CLV forecast accuracy compared to standard RFM.

---

## Research Questions

1. **RQ1** — Does RFMRV produce statistically superior customer segments compared to RFM alone?
2. **RQ2** — Does B2B/B2C stratification improve cluster quality within each framework?
3. **RQ3** — Does RFMRV improve CLV forecasting accuracy over standard RFM?
4. **RQ4** — What distinct segment-level strategies does RFMRV enable that RFM cannot?

---

## Dataset

- **Source:** UCI Machine Learning Repository — [Online Retail II Dataset (ID: 502)](https://archive.ics.uci.edu/dataset/502/online+retail+ii)
- **Records:** 1,067,371 transactions
- **Period:** 01 December 2009 – 09 December 2011
- **Sheets:** Year 2009–2010 (Cohort 2010) and Year 2010–2011 (Cohort 2011)
- **Key columns:** `Invoice`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `Price`, `Customer ID`, `Country`

> **Important:** This is Dataset 502, NOT Dataset 352. Column names differ — use `Price` (not `UnitPrice`) and `Customer ID` (with space).

---

## Repository Structure

```
.
├── README.md                          # This file
├── Dissertation_Vishal_Chalia.ipynb   # Main analysis notebook (Google Colab)
├── outputs/
│   ├── figures/                       # All generated charts and visualisations
│   └── Thesis_-UCI.html               # Full HTML export of notebook with outputs
└── data/
    └── (Dataset not included — download from UCI link above)
```

---

## Methodology

### Feature Engineering — RFMRV (7 Features)

| Feature | Description |
|---|---|
| **R** — Recency | Days since last purchase |
| **F** — Frequency | Number of unique invoices |
| **M** — Monetary | Total net revenue |
| **R (CancelRatio)** | Proportion of cancelled invoices |
| **V (BDI)** | Basket Diversity Index — unique products per order |
| **V (CCR)** | Cross-Channel Consistency Rate — order regularity |
| **V (CPE)** | Cost-Per-Engagement — average spend per interaction |

### Clustering
- Algorithms: **K-Means**, **Hierarchical Agglomerative Clustering (HAC)**, **DBSCAN**
- Evaluated on: **RFM (3 features)** vs **RFMRV (7 features)**
- Cohorts: **2010** and **2011**
- Metrics: **Silhouette Score**, **Davies-Bouldin Index**, **Calinski-Harabasz Index**

### Temporal Migration
- Cohort migration matrix (2010 → 2011)
- Segments: *High-Value Serial Returners* and *Low-Value Dormant*
- Finding: **100% diagonal stability** — no cross-segment migration observed

### CLV Forecasting
- **Probabilistic:** BG/NBD + Gamma-Gamma model
- **Machine Learning:** Random Forest and Gradient Boosting on RFM vs RFMRV
- **Best model:** Random Forest + RFMRV
  - R² = 0.4098 | MAE = 0.4813 | RMSE = 0.6651 | CV-RMSE ≈ 0.7027
  - ~12.4% relative R² improvement over RFM baseline

### B2B/B2C Stratification
- Proxy rule: customers with mean order quantity above the 90th percentile or from non-UK countries classified as B2B
- B2B Silhouette ≈ 0.82 vs B2C ≈ 0.54–0.70 — confirms stratification improves cluster quality

---

## Key Results

| Model | Features | R² | MAE | RMSE |
|---|---|---|---|---|
| Random Forest | RFMRV | **0.4098** | 0.4813 | 0.6651 |
| Gradient Boosting | RFMRV | 0.3871 | 0.5102 | 0.6934 |
| Random Forest | RFM | 0.3648 | 0.5287 | 0.7123 |
| Gradient Boosting | RFM | 0.3412 | 0.5519 | 0.7387 |
| BG/NBD + Gamma-Gamma | RFM | — | — | CV=1.10 |
| BG/NBD + Gamma-Gamma | RFMRV | — | — | CV=1.07 |

---

## How to Run

### Option 1: Google Colab (Recommended)

1. Open [Google Colab](https://colab.research.google.com/)
2. Go to **File → Open notebook → GitHub** and paste this repo URL
3. Select `Dissertation_Vishal_Chalia.ipynb`
4. Install dependencies (first cell):

```python
!pip install lifetimes ucimlrepo scikit-learn pandas numpy matplotlib seaborn scipy
```

5. Upload dataset when prompted:
```python
from google.colab import files
uploaded = files.upload()  # Upload online_retail_II.xlsx from UCI
```

6. Run all cells sequentially (**Runtime → Run all**)
7. Final cell zips and downloads all outputs automatically

### Option 2: Local Jupyter

```bash
git clone https://github.com/vishalchalia24/Dissertation_UCI_Online_Retail_II_Dataset_502_Vishal_Chalia.git
cd Dissertation_UCI_Online_Retail_II_Dataset_502_Vishal_Chalia
pip install -r requirements.txt
jupyter notebook Dissertation_Vishal_Chalia.ipynb
```

---

## Dependencies

```
pandas >= 1.5
numpy >= 1.23
scikit-learn >= 1.1
matplotlib >= 3.5
seaborn >= 0.12
scipy >= 1.9
lifetimes >= 0.11.3
ucimlrepo >= 0.0.3
openpyxl >= 3.0
```

---

## Contributions

This dissertation makes four original contributions:

1. **RFMRV Framework** — extends RFM with three behavioural features capturing return behaviour, basket diversity, and engagement consistency.
2. **B2B/B2C Stratification** — demonstrates that pre-segmentation by customer type significantly improves cluster geometric quality.
3. **Cohort Migration Matrix** — provides first longitudinal stability analysis of RFMRV segments across two full trading years.
4. **CLV Benchmarking** — directly compares probabilistic (BG/NBD) vs ML (RF, GB) CLV forecasting under both RFM and RFMRV feature sets.

---

## Citation

If referencing this work:

```
Chalia, V. (2025) Beyond RFM: Integrating Return Behaviour and Channel Complexity
for Enhanced Customer Segmentation and CLV Forecasting in E-Commerce.
MSc Dissertation, Gisma University of Applied Sciences, Berlin.
Available at: https://github.com/vishalchalia24/Dissertation_UCI_Online_Retail_II_Dataset_502_Vishal_Chalia
```

---

## Dataset Citation

Chen, D. (2019) *Online Retail II*. UCI Machine Learning Repository. Dataset ID: 502.
Available at: https://archive.ics.uci.edu/dataset/502/online+retail+ii
[Accessed: March 2025]

---

## Licence

This project is for academic purposes only. The UCI Online Retail II dataset is used under the Creative Commons Attribution 4.0 International licence (CC BY 4.0).

---

*For questions or feedback, connect via [LinkedIn](https://www.linkedin.com/in/vishalchalia) or raise a GitHub issue.*
