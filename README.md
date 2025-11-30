# 🏡 HMDA California 2024 — Housing Loan Approval Classifier (Single-Family Cohort)

## ❓ Research Question
- *Can we accurately predict whether a housing-loan application will be approved based on applicant characteristics, financial indicators, and property-related information using only application-time-known features?*

## 📓 Notebook
- You may review the current Jupyter notebook for the full baseline analysis:
➡ **Notebook Link:** 
<a href="[https://example.com](https://github.com/joshdailey99/module20/blob/main/Module_20.ipynb)" target="_blank">["https://github.com/joshdailey99/module20/blob/main/Module_20.ipynb"](https://github.com/joshdailey99/module20/blob/main/Module_20.ipynb)</a>


## ✅ Cohort & Data Scope
- Dataset has been filtered to represent:
  - **Single family (1–4 residential units)**
  - **Non-manufactured homes**
  - **Not primarily business or commercial purpose**
  - **Owner-occupied purchase & refinance use cases**
- **Post-decision fields** (e.g., denial reason) are excluded from modeling to prevent **target leakage**

## 🧼 Data Cleaning Decisions (Baseline Phase)
- **Missing values ≤10%** → retained, **median-imputed (numeric)** or **mode-imputed (categorical)**
- **Excessive missingness ≧10%** → dropped from modeling 
- **Outlier rate moderate (≈4–8%)** → retained and **capped at 1st/99th percentiles (winsorization)** for:
  - `loan_amount`
  - `income`
  - `property_value`
  - `loan_to_value_ratio`
- **loan_term** → converted to **categorical mortgage-term buckets** instead of scaling to avoid skew distortion

## 📊 Exploratory Data Analysis (Section 1–6 Summary)
Key insights revealed by submission-time variables:
- **Income and property values contain significant upper-tail variance**, representing real underwriting separation but too skewed for direct modeling influence → justified capping instead of dropping
- **Loan size relative to income (`loan_to_income`) is likely to be a strong future predictive feature**
- **Collapsed demographic attributes (`derived_race`, `derived_ethnicity`, `derived_sex`) retained for analysis**, not dropped — demographics may still provide meaningful approval-pattern insight
- **Denial reason checkboxes confirmed to be post-decision disclosure fields**, not usable for predictive modeling

## 🤖 Baseline Modeling Notes
- Baseline classifier built using **Logistic Regression** to establish underwriting separability before tuning
- **Next phase will compare 4 classifiers** under identical input transforms:
  - Logistic Regression
  - K-Nearest Neighbors (KNN)
  - Decision Tree
  - Support Vector Machine (SVM)
- **No hyperparameter tuning performed yet**

## 🎯 Evaluation Metrics Selected
- **Accuracy** → primary correctness benchmark
- **F1-Score** → balances Precision/Recall under approval-heavy class imbalance
- **ROC AUC** → measures predictor separability at submission time
- **Confusion matrix included for interpretability**

---

## 🚀 Next Steps (Planned Modeling, Comparison & Tuning)

- **Fit baseline classifiers**
  - Logistic Regression → interpretable linear underwriting benchmark
  - K-Nearest Neighbors (KNN) → non-linear proximity-based baseline
  - Decision Tree → rule-based hierarchical decisioning baseline
  - Support Vector Machine (SVM) → margin-based non-linear baseline
  - *No tuning in this phase*

- **Evaluate model performance**
  - Metrics: Accuracy, F1-Score, ROC AUC
  - Include graphical Confusion Matrix for interpretability
  - Compare algorithms on same feature set to rank baseline suitability

- **Model tuning phase (anticipated)**
  - KNN → tune `n_neighbors`, distance metrics, and weighting
  - Decision Tree → tune `max_depth`, `min_samples_split`, `criterion`
  - Logistic Regression → tune `C` (regularization strength), penalty type, and solver
  - SVM → tune `C`, kernel type (RBF/linear), `gamma`
  - Techniques: `GridSearchCV` or `HalvingGridSearchCV`
  - Avoid leakage fields in all tuned models

- **Select best performing model**
  - Choose algorithm with strongest generalization and separability
  - Document decision based on tuned metric improvements vs baseline

- **Report final results**
  - Summarize performance gains from tuning
  - Validate if approval can be predicted within acceptable error bounds
  - Produce model ranking, tuning logs, final justification, and recommendations


---

## ⭐ Why This Matters
- Loan approval is a **first-order underwriting decision**
- Predicting approval **before full underwriting** can:
  - Reduce operational costs
  - Improve applicant experience
  - Enable faster credit decisioning for retail lenders
  - Provide fair algorithmic benchmarking using non-leaky features only

