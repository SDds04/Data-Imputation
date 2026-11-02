# Benchmarking Missing Data Imputation Across Four Iterative Pipeline Versions on the Adult Dataset

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange.svg)](https://scikit-learn.org/)
[![BSc Project](https://img.shields.io/badge/Academic-BSc%20Project-lightgrey.svg)]()
[![License: All Rights Reserved](https://img.shields.io/badge/License-All%20Rights%20Reserved-red.svg)](LICENSE)

## 📌 Project Overview
Missing values are a common challenge when preparing and analyzing real-world tabular data. This repository contains the implementation and experimental benchmark developed as part of a **B.Sc. Research Project**.

The goal of this project was to implement, evaluate, and reproduce key algorithms referenced in the research paper:
> **"Missing Value Imputation in Tabular Data Lakes Unleashed: A Hybrid Approach"**  
> *The VLDB Journal*, 2026, 35:11.  
> DOI: [10.1007/s00778-025-00957-1](https://doi.org/10.1007/s00778-025-00957-1)  
> Authors: Feng Luo, Hai Lan, Hui Luo, Zhifeng Bao, J. Shane Culpepper, Shazia Sadiq, and Xiaoli Wang.

The paper studies missing-value imputation in data lakes, where the best approach can depend on the available data, the table type, and whether the missing column is numerical or categorical. This project focuses on implementing and evaluating selected imputation methods using the **UCI Adult Income** dataset.

This project does **not** reproduce the paper’s complete data-lake system, which includes contextual search, acquisition-guided estimation, and a classifier for selecting an approach. Instead, it evaluates selected imputation algorithms on a single mixed-type dataset across controlled missingness rates (`0.0175%`, `1%`, `10%`).

---

## 📂 Repository Versions & Evolutionary Steps
The code is structured across four progressive iterations, illustrating key improvements in data preprocessing, feature engineering, and categorical encoding strategies:

1. **Version 1 (Baseline Pipeline):** 
   - Initial clean-up of the Adult dataset (handling missing placeholder characters `?`).
   - Removal of non-predictive sampling weights (`fnlwgt`) and duplicate education measures (`educational-num`).
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
   - Implemented **Ordinal Encoding** to compare its performance and runtime against **One-Hot Encoding**.

---

## 🛠️ Imputation Methods Evaluated
- **Numerical Imputation:**
  - **MICE** (Multivariate Imputation by Chained Equations / IterativeImputer)
  - **MissForest** (Non-parametric Random Forest-based imputation)
  - **GAIN** (Generative Adversarial Imputation Networks)
- **Categorical Imputation:**
  - **MissForest** (adapted for categorical feature spaces)
  - **Mode Imputation** (Most frequent value baseline; included for comparison)

---

## 🔬 Experimental Design & Preprocessing

### Creating a Complete Baseline
To measure how accurately each method fills missing values, the experiments begin with a complete baseline dataset:
1. Values marked with `?` are treated as missing.
2. Rows with missing values and duplicate rows are removed to create a complete baseline.
3. Missing values are then added to this baseline at controlled rates using the **MCAR** (Missing Completely at Random) mechanism. The original values are retained as ground truth for evaluation.

### Preprocessing Notes
- **`fnlwgt` (Final Weight):** Excluded from the feature set. While `fnlwgt` represents population sampling weights in census studies, it carries little predictive value and was omitted for this project.
- **`education`:** Removed because the dataset already contains the corresponding numeric feature, `educational-num`.
- **Categorical Grouping:** High-cardinality and sparse categories were grouped into broader bins (in Versions 3 & 4) across features like `native-country`, `occupation`, `workclass`, and `marital-status` to mitigate noise and stabilize tree-based imputation methods.
- **Numerical Features:** Scaled to the range `[0, 1]` using `MinMaxScaler`. This is especially relevant to GAIN, whose generator produces values in this range.

### Missingness Rates
Missing values are injected at three rates:
- **0.0175%** — based on the missing-value rates reported for numerical and categorical cells in the paper’s OpenData dataset.
- **1%**
- **10%**

---

## 📈 Evaluation Metrics
- **Numerical Features:** Root Mean Squared Error (**RMSE**) and Mean Absolute Error (**MAE**).
- **Categorical Features:** Imputation **Accuracy** and **Error Rate**.
- The imputed values are compared with the original ground-truth values that were hidden when missingness was added.

---

## ⚖️ License
This project is academic research work. **All Rights Reserved**. Unauthorized copying, distribution, or usage is strictly prohibited until official publication of the research. See the [LICENSE](LICENSE) file for details.
