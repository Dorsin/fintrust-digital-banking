# FinTrust Digital Banking — Risk Review & Behavioral Analytics

## 📌 Project Overview
FinTrust Digital Banking seeks to optimize its transaction risk review process by detecting anomalous behavior and potential fraud indicators across customer profiles and transaction metadata.

This repository contains the end-to-end Machine Learning pipeline designed to analyze customer-transaction behaviors, test risk hypotheses, build 16 domain-engineered features, and evaluate predictive models for the `Risk_Review_Flag` target variable.

## 🛠️ Key Project Deliverables & Phasing

### Phase I & II: Problem Formulation & Exploratory Data Analysis
* **Candidate Features Assessment**: Merged `Customer` and `Transaction` datasets on `Customer_ID`. Evaluated candidate features across demographic profiles and transactional metadata
* **Risk Factor Hypotheses Formulation**: Formulated and statistically tested 4 core hypotheses ($H_1$ to $H_4$) covering amount-to-income ratios, unverified device flags, account maturity risks, and cross-border channel exposures

### Phase III: Data Preparation, Feature Engineering & Modeling (Week 2)
1. **Prepared Modelling Dataset (`exports/prepared_modelling_dataset.csv`)**:
   * Engineered **16 predictive features** covering temporal indicators (`Is_Night_Transaction`, `Is_Weekend_Transaction`), behavioral frequency ratios (`Transaction_Frequency`, `Amount_To_Customer_Avg_Ratio`), channel mismatches, and composite risk flags (`New_Account_High_Risk`)
   * Maintained raw categorical data in the exported dataset to prevent data leakage, handling One-Hot Encoding and scaling dynamically inside Scikit-Learn pipelines

2. **Imbalanced Evaluation Strategy (PR-AUC Focus)**:
   * Shifted from ROC-AUC to **Precision-Recall AUC (PR-AUC)** due to the ~19.58% class imbalance in the target variable
   * Benchmarked model performance against a **0.1958 random guess chance baseline**

3. **Model Benchmarking**:
   * **Baseline Logistic Regression**: Achieved **PR-AUC = 0.3051** (Recall: 57.87%, Precision: 28.54%) with `class_weight='balanced'`
   * **Advanced Random Forest**: Achieved **PR-AUC = 0.3050** (Accuracy: 70.17%, Recall: 41.91%, Precision: 30.78%), significantly reducing false alarm overhead (False Positives reduced from 681 to 443)

4. **Synthetic Data & Bias Analysis**:
   * Documented artifact bias and distorted prior probabilities inherent to algorithmically generated benchmark datasets (e.g., artificial ~20% prevalence vs. <1-2% real-world fraud)
   * Outlined future calibration (Platt Scaling) and explainability (SHAP / LIME) roadmaps.

## 📊 Performance Summary

| Model | Accuracy | Precision | Recall | F1-Score | PR-AUC | Key Operational Role |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Random Guess Baseline** | — | 19.58% | 100.0% | — | **0.1958** | Uninformed baseline reference |
| **Baseline Logistic Regression** | 63.38% | 28.54% | **57.87%** | **0.3823** | **0.3051** | High-coverage / risk-averse flagger |
| **Advanced Random Forest** | **70.17%** | **30.78%** | 41.91% | 0.3550 | **0.3050** | Operational workload & false alarm reducer |

---

## 📂 Repository Structure

```text
.
├── data/
│   ├── FinTrust_Customer_Data.csv           # Customer demographic & account metadata
│   ├── FinTrust_Data_Dictionary.xlsx        # Feature definitions & data dictionary
│   └── FinTrust_Transaction_Data.csv        # Transaction activity records
├── exports/
│   └── prepared_modelling_dataset.csv       # Deliverable 1: Final feature-engineered dataset
├── notebooks/
│   ├── 01_week1_problem_formulation.html   # Week 1 notebook export (HTML)
│   ├── 01_week1_problem_formulation.ipynb  # Week 1 Jupyter Notebook
│   ├── 02_week2_data_prep_eda_modeling.html# Week 2 notebook export (HTML)
│   └── 02_week2_data_prep_eda_modeling.ipynb# Main Week 2 Data Prep & Modeling Notebook
├── .gitignore                               # Git ignore rules for non-tracked files
├── 01_week1_problem_formulation.pdf        # Clean exported PDF report (Week 1)
├── 02_week2_data_prep_eda_modeling_report.pdf # Executive PDF Report (Week 2)
├── fintrust_digital_banking.html            # Full compiled HTML project report
└── README.md                                # Main project documentation