# 📦 Stochastic Supplier Risk Management System
### Seasonal Monte Carlo Simulation — Footwear Peak Season Jul–Dec 2026

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Method](https://img.shields.io/badge/Method-Monte%20Carlo-orange)
![Status](https://img.shields.io/badge/Status-Complete-green)

---

## What is this project?

A stochastic simulation engine that analyzes the sales history of **44 footwear suppliers (2023–2025)** to forecast demand, quantify stockout risk, and recommend optimal safety stock levels for each month from July to December 2026.

Developed as a supply chain analytics project for a footwear retail company in Mexico.

---

## The Problem

The company tracks not only completed sales but also **denied units** — customers who couldn't purchase due to stockouts. The challenge: with only 3 years of history and 44 suppliers, how do you predict which ones will fail in 2026 and how much buffer inventory you need?

---

## Methodology

**Why Monte Carlo instead of Machine Learning?**  
When isolating a supplier's behavior in a specific month, the actual sample size is N = 3 observations. Any ML model would massively overfit those 3 points with no generalization capacity. Monte Carlo fits a statistical distribution to those observations and generates 10,000 possible scenarios — honestly quantifying uncertainty.

**5-step pipeline:**

1. **Real demand reconstruction**  
   `Real_Demand = Sales + (Denied × 0.25)`  
   Only 25% of denied units represent lost demand; 75% of rejected customers buy a substitute product in-store.

2. **Supplier Risk Score**  
   `Score = α × Denial_Rate + (1−α) × CV`  
   Optimized with Walk-Forward Cross-Validation and Spearman Rank Correlation (α = 1.0, ρ = 0.30).

3. **Automatic distribution fitting (AIC)**  
   For each supplier × month, the system selects between Normal and Gamma distributions using the Akaike Information Criterion.

4. **Seasonal Monte Carlo simulation**  
   10,000 iterations of Demand vs. Capacity per supplier per month → Stockout probability + P95 Safety Stock.

5. **Strategic What-If analysis**  
   Simulates risk reduction from transferring volume from high-risk suppliers to the star pool.

---

## Key Finding

The 3 best-performing suppliers (FR > 99.2%) manufacture **only 1 to 4 exclusive models**.  
The 5 worst manufacturers handle up to **19 simultaneous models**.

The root cause of chronic stockouts is not lack of raw capacity — it's **manufacturing complexity cost**: setup times, mold rotation, and fragmented production runs.

---

## Results

| Supplier | Fulfillment Rate | Avg. Stockout Prob. 2026 | Total Safety Stock Jul–Dec |
|----------|-----------------|--------------------------|---------------------------|
| PROV33 | 99.48% | 38.0% | 246,804 units |
| PROV6  | 99.26% | 38.8% | 48,868 units |
| PROV1  | 99.44% | 57.8% | 9,760,574 units |
| PROV15 | 90.92% | 59.4% | 85,713,680 units |
| PROV48 | 91.07% | 66.8% | 82,967,104 units |

**Best business case:** transferring demand from PROV10 → PROV1 would save ~1,000,000 units across the season.

---

## Repository Structure

```
├── analisis_proveedores_v4.py   # Full pipeline (Spanish)
├── whitepaper_v5.docx           # Technical & strategic report (Spanish)
├── anexos/
│   ├── tablas/
│   │   ├── tabla_riesgo_proveedores.csv
│   │   ├── tabla_prediccion_2026.csv
│   │   └── tabla_transferencia_estrategica.csv
│   └── graficas/
│       ├── 01_fulfillment_proveedores.png
│       ├── 02_demanda_tiempo.png
│       ├── 04_heatmap_prob_desabasto.png
│       ├── 05_heatmap_stock_seguridad.png
│       └── 03_prediccion_2026_*.png  (44 charts, one per supplier)
└── README.md
```

> ⚠️ The original dataset is not included due to company confidentiality. The full pipeline can be adapted to any transactional sales dataset with the same structure.

---

## How to Run

```bash
# Install dependencies
pip install pandas numpy scipy matplotlib seaborn

# Place BD_PROYECTO_RETO.csv in /content/ (Google Colab) or update ARCHIVO_CSV path
# Run
python analisis_proveedores_v4.py
```

The script automatically generates all CSV tables and PNG charts in the working directory.

---

## Tech Stack

`Python` · `Pandas` · `NumPy` · `SciPy (stats, MLE)` · `Matplotlib` · `Seaborn` · `Monte Carlo Simulation` · `Walk-Forward Cross-Validation` · `AIC Model Selection`

---

## Author

**Johanna Camila Willis Ruiz** - *Data Science & Mathematics Engineering student*

This project was developed individually for the **"Organizaciones Resilientes"** academic block, within the School of Engineering and Sciences at **Tecnológico de Monterrey**. 

It features a stochastic supply chain risk analysis applied to a real footwear retail company in Mexico.
