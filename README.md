# Comparative Analysis of Missing Data Imputation Methods on the Adult Dataset

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange.svg)](https://scikit-learn.org/)
[![BSc Project](https://img.shields.io/badge/Academic-BSc%20Project-lightgrey.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📌 Project Overview
This repository contains the implementation and experimental benchmark developed as part of a **B.Sc. Research Project**.

The goal of this project was to implement, evaluate, and reproduce key algorithms referenced in the research paper:
> **"Missing Value Imputation in Tabular Data Lakes Unleashed: A Hybrid Approach"**  
> *The VLDB Journal (2026) 35:11*  
> *Authors: Feng Luo, Hai Lan, Hui Luo, Zhifeng Bao, J. Shane Culpepper, Shazia Sadiq, Xiaoli Wang*  
> *DOI: [10.1007/s00778-025-00957-1](https://doi.org/10.1007/s00778-025-00957-1)*

Using the **UCI Adult Income** dataset, this project investigates how classical statistical methods, non-parametric tree ensembles, and generative deep learning paradigms perform when recovering artificially introduced missingness across mixed (numerical and categorical) tabular feature spaces at controlled rates (e.g., `1.75%`, `10%`).

---

## 📂 Repository Versions & Evolutionary Steps
The code is structured across four progressive iterations, illustrating key improvements in data preprocessing, feature engineering, and categorical encoding strategies:

1. **Version 1 (Baseline Pipeline):** 
   - Initial clean-up of the Adult dataset (handling missing placeholder characters `?`).
   - Removal of non-predictive sampling weights (`fnlwgt`) and duplicate education measures.
   - Evaluates base numerical algorithms (MICE, MissForest, GAIN) and categorical strategies (MissForest, Mode Imputation).
2. **Version 2 (Ordinal Encoding Pipeline):** 
   - Incorporates `OrdinalEncoder` into the preprocessing and categorical imputation stages.
   - Reconstructs predicted values back into original string labels via `inverse_transform` with boundary clipping and rounding.
3. **Version 3 (Advanced Feature Grouping & One-Hot Encoding):** 
   - Domain-informed category aggregation (consolidating high-cardinality features like `native-country`, `occupation`, `workclass`, and `marital-status`).
   - Re-categorization of education levels into standardized tiers.
   - Utilizes **One-Hot Encoding** with an `argmax` selection strategy to reconstruct imputed categorical variables.
4. **Version 4 (Refined Feature Engineering + Ordinal Encoding + Visualization):** 
   - Retains the robust category consolidation and preprocessing pipeline introduced in Version 3.
   - Reapplies **Ordinal Encoding** to compare its performance against **One-Hot Encoding**.

---

## 🛠️ Imputation Methods Evaluated
- **Numerical Imputation:**
  - **MICE** (Multivariate Imputation by Chained Equations / IterativeImputer)
  - **MissForest** (Non-parametric Random Forest-based imputation)
  - **GAIN** (Generative Adversarial Imputation Networks)
- **Categorical Imputation:**
  - **MissForest** (adapted for categorical feature spaces)
  - **Mode Imputation** (Most frequent baseline)

---

## 📈 Evaluation Metrics
- **Numerical Features:**
  - Root Mean Squared Error (**RMSE**)
  - Mean Absolute Error (**MAE**)
- **Categorical Features:**
  - Imputation **Accuracy**
  - **Error Rate**

---

## 🔧 Preprocessing Notes
- **`fnlwgt` (Final Weight):** Excluded from the feature set. While `fnlwgt` represents population sampling weights in census studies, it carries little predictive value and was omitted for this project.
- **Categorical Grouping:** High-cardinality and sparse categories were grouped into broader bins (in Versions 3 & 4) to mitigate noise and stabilize tree-based imputation methods.

