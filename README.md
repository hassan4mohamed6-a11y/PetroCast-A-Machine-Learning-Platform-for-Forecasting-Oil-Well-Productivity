# PetroCast: A Machine Learning Platform for Forecasting Oil Well Productivity

A web-based machine learning platform that predicts oil production output from well operational data — without writing a single line of code. Built with Python and Streamlit, PetroCast automates the full forecasting pipeline from data upload to model training, evaluation, and prediction.

---

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Models](#models)
- [Methodology](#methodology)
- [Model Performance](#model-performance)
- [Technologies](#technologies)
- [Usage](#usage)
- [Input Data Format](#input-data-format)
- [Team](#team)

---

## Overview

This project was developed as part of the **AIE121: Machine Learning** course at Galala University under the supervision of **Professor Shaker El-Sappagh** during Spring 2025.

PetroCast addresses a critical gap in petroleum engineering: traditional forecasting relies on manual spreadsheets and basic regression models that fail to capture nonlinear relationships in production data. PetroCast replaces that with an automated, interpretable, and reliable ML workflow accessible to engineers and analysts with no data science background.

---

## Features

- **Automated Data Validation** — Checks for required columns, removes negative oil volumes, filters wells with fewer than 6 records
- **Outlier Handling** — Winsorization capping values below the 1st and above the 99th percentile
- **Data Leakage Detection** — Automatically flags and removes features with Pearson correlation > 0.95 with the target
- **Time-Aware Training** — Chronological 80/20 train-test split with 5-fold TimeSeriesSplit cross-validation
- **Hyperparameter Tuning** — GridSearchCV optimization for all models
- **SHAP Explainability** — Beeswarm plots showing feature contributions for every prediction
- **Residual Analysis** — Visual plots to detect overfitting, underfitting, or systematic bias
- **Model Comparison** — Side-by-side R² and MSE comparison across all models with bar chart visualization
- **Real-Time Prediction** — Interactive sliders for instant oil volume forecasting
- **Unit Conversion** — Output in barrels/day or liters/day
- **Model Export** — Download trained models as `.pkl` files for deployment

---

## Models

| Model | Type |
|---|---|
| XGBoost | Gradient Boosting |
| Random Forest | Ensemble (Bagging) |
| Support Vector Regression (SVR) | Kernel-based |
| Bayesian Ridge | Linear with uncertainty estimation |
| Decision Tree | Rule-based |

All models are wrapped in Scikit-learn pipelines. SVR includes StandardScaler for normalization.

---

## Methodology

1. **Data Preparation** — Validate columns, clean negatives, remove sparse wells, cap outliers, flag leakage
2. **Feature Selection** — `DEPTH_MD`, `Reservoir_pressure`, `Working_hours`
3. **Time-Aware Splitting** — Sort by `Date`, 80% train / 20% test chronologically
4. **Hyperparameter Tuning** — GridSearchCV with 5-fold TimeSeriesSplit, scored by negative MSE
5. **Evaluation** — Test R², Cross-Validated R², Test MSE, CV MSE; overfitting warning if R² gap > 0.2
6. **Interpretability** — SHAP beeswarm plots, residual plots, correlation heatmap, outlier boxplots

---

## Model Performance

| Model | Test R² | CV R² | Test MSE | CV MSE |
|---|---|---|---|---|
| **XGBoost** | **0.92** | **0.89** | **12.34** | **14.02** |
| Random Forest | 0.88 | 0.86 | 15.67 | 16.20 |
| SVR | 0.84 | 0.82 | 18.90 | 19.80 |
| Bayesian Ridge | 0.75 | 0.74 | 24.10 | 25.00 |
| Decision Tree | 0.71 | 0.70 | 28.55 | 30.00 |

XGBoost achieved the best performance. SHAP analysis confirmed that `DEPTH_MD` and `Reservoir_pressure` are the dominant predictive signals.

---

## Technologies

| Technology | Purpose |
|---|---|
| Python | Core language |
| Streamlit | Web interface |
| Scikit-learn | ML models and pipelines |
| XGBoost | Gradient boosting model |
| SHAP | Model explainability |
| Pandas / NumPy | Data processing |
| Matplotlib / Seaborn | Visualizations |

---

## Usage

1. Install dependencies:
   ```bash
   pip install streamlit pandas numpy scikit-learn xgboost shap matplotlib seaborn
   ```
2. Run the app:
   ```bash
   python -m streamlit run Petrocast-1.py
   ```
3. Upload your CSV file
4. Select a model and click **Train**
5. View metrics, residual plots, and SHAP explanations
6. Use the prediction sliders to forecast oil output
7. Export the trained model as `.pkl` if needed

---

## Input Data Format

Your CSV must contain the following columns:

| Column | Description |
|---|---|
| `DEPTH_MD` | Measured depth of the well |
| `Reservoir_pressure` | Reservoir pressure reading |
| `Working_hours` | Hours the well was operational |
| `Oil_volume` | Target — oil production volume |
| `Date` | Date of the record (for time-aware splitting) |
| `WELL` | Well identifier |

---



> **Course:** AIE121 — Machine Learning | **Supervisor:** Prof. Shaker El-Sappagh | **Galala University, Spring 2025**
