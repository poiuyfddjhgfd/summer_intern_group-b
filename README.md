# Crop Yield Prediction Integration (ENSO, WD & Cloudburst Features)

## Overview
This repository contains the integration pipeline and evaluation framework for enriching the district-level ICRISAT crop yield panel with macro-climate indices (**ENSO**) and extreme localized weather event indicators (**Western Disturbances** and **Cloudburst** data). 

The primary goal is to evaluate whether adding fine-grained climate and extreme event features improves predictive performance over standard geographic/crop baselines.

---

## Dataset & Feature Engineering

1. **ICRISAT Crop Yield Panel (Base Target Dataset)**
   * **Scope:** 311 districts across India (1966–2017), ~28,723 record rows.
   * **Key Preprocessing:** Replaced high-cardinality one-hot district encoding with **district target encoding** to prevent overfitting and significantly improve baseline model $R^2$.

2. **ENSO (El Niño–Southern Oscillation) Index**
   * **Scope:** National/Global climate signal (1950–2024).

3. **Western Disturbances (WD)**
   * **Scope:** Monthly counts across India (1949–2018).

4. **Cloudburst Features**
   * **Scope:** Extreme weather features extracted across 5 target districts (*Guntur, Kota, Ludhiana, Nashik, Patna*) spanning 1989–2020.
   * **Key Feature:** `cloudburst_probability` (~0.2% rare-event daily occurrence threshold).

---

## Evaluation & Ablation Framework

The framework conducts a two-stage ablation study to assess the impact of macro-level vs. hyper-local weather signals:

### 1. Unfair Full-Dataset Ablation (All 311 Districts)
* **Model A (ENSO Integration):** $R^2 = 0.070$ vs. Baseline $R^2 = 0.040$.
* *Takeaway:* On a national scale, ENSO provides a modest but consistent predictive lift over raw district baselines.

### 2. Controlled / Fair Subset Ablation (5 Cloudburst Districts)
* **Model A (ENSO):** $R^2 = 0.568$ vs. Baseline $R^2 = 0.583$.
* **Model F (WD + Cloudburst):** **$R^2 = 0.612$** (Best performing model).

---

## Key Finding: The ENSO-Flip Phenomenon

A critical insight from this research is the divergence between national and localized model evaluations:

* **National Scale:** Global climate indicators like ENSO provide meaningful signal across large, varied geography.
* **Local Scale (5-District Sample):** When restricted to a small, geographically narrow subset (5 districts), local extreme weather variables (WD + Cloudburst) dominate predictive power ($R^2 = 0.612$), while broad ENSO signals can introduce noise relative to the baseline.

---

## Repository Structure

```text
├── data/
│   ├── ICRISAT_yield_panel.csv
│   ├── enso_vertical_output.csv
│   ├── WD_monthly_counts_India.csv
│   └── cloudburst_features.csv
├── notebooks/
│   └── crop_yield_integration_v3.ipynb   # Main execution notebook
└── README.md
