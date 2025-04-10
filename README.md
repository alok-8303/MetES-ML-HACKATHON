# ⚡ Metallurgica Hackathon 2025 – Predicting Electrical Conductivity (%IACS) of Alloys

This repository contains our solution for the **Metallurgica Hackathon 2025**, where the objective was to predict the **electrical conductivity (%IACS)** of copper-based alloys using their elemental composition and processing parameters.

## 🧪 Problem Statement

Given a dataset of alloy samples with detailed elemental composition, processing conditions, and mechanical properties, the task is to predict the electrical conductivity of these alloys, which is a key property for their application in electrical components.

## 📁 Dataset Overview

- **Training samples:** 1600 (after cleaning: 1598)
- **Test samples:** ~350
- **Features:** 41 columns
  - **Elemental Composition:** 27 numerical columns (Cu, Al, Ag, etc.)
  - **Processing Parameters:** 7 columns (Tss, tss, CR reduction %, etc.)
  - **Mechanical Properties:** Hardness, Yield strength, Ultimate tensile strength
  - **Categorical:** Alloy class, Aging, Secondary thermo-mechanical process
- **Target Variable:** Electrical conductivity (%IACS)

## ⚙️ Preprocessing

- **Missing Values:** 
  - Dropped columns with >80% missing values (e.g., Yield strength, UTS).
  - Imputed numerical features with mean, categorical with 'missing'.
- **Encoding:**
  - One-hot encoding for categorical features.
- **Dropped Columns:**
  - `ID` and `Alloy formula` (redundant due to elemental breakdown).

## 🛠️ Feature Engineering

Custom domain-driven features were created:

- `Cu_Al_ratio = Cu / (Al + 0.001)`
- `conductive_elements_sum = Cu + Ag`
- `total_alloying = sum(all numerical features)`
- `thermal_factor = Tss * log1p(tss)`
- `aging_factor = Tag * log1p(tag)`

These features improved model interpretability and predictive power.

## 🤖 Model Development

- **Primary Model:** `CatBoostRegressor`
  - `iterations = 500`
  - `learning_rate = 0.03`
  - `depth = 3`
  - `loss_function = 'MAE'`
  - Achieved **Validation MAE: 13.3832**
- **Ensemble Attempts:** 
  - `StackingRegressor` with models like XGB, LGBM, Ridge, Lasso, ElasticNet.
  - Did **not** outperform CatBoost.

## 🧪 Evaluation

- **Validation Strategy:** 80/20 train-validation split.
- **Final MAE on test submission:** **13.3770**
- **Submission Format:** CSV with `ID` and predicted `%IACS`.

## 📈 Potential Improvements

- Expand validation size (20–30%)
- Drop uninformative/sparse features (e.g., rare elements)
- Apply hyperparameter tuning (`GridSearchCV`)
- Consider transformations on target if skewed
- Refine ensemble modeling

## 🏁 Conclusion

This project demonstrates robust data handling, meaningful feature engineering based on metallurgical principles, and effective modeling using CatBoost. With further fine-tuning, it holds potential for even better performance on similar tasks.


