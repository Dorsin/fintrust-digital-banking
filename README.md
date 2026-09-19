# FinTrust Digital Banking — Risk Review & Behavioral Analytics

## 📌 Project Overview
FinTrust Digital Banking seeks to optimize its transaction risk review process by detecting anomalous behavior and potential fraud indicators across customer profiles and transaction metadata.

This repository contains the end-to-end Machine Learning pipeline designed to analyze customer-transaction behaviors, test risk hypotheses, and build predictive and anomaly detection models for the `Risk_Review_Flag` target variable.

---

## 🛠️ Key Deliverables (Phase II: Prepare)

### 1. Candidate Features Assessment (Part C)
* Merged `Customer` and `Transaction` datasets using an inner join on `Customer_ID`
* Selected and evaluated 12 candidate features combining demographic profiles and transactional patterns (`Amount_NGN`, `Device_Type`, `Location`, `International_Transaction`, `Monthly_Income_Band`, etc.)
* Documented feature selection rationale using domain risk analysis, non-linear association metrics ($\chi^2$, Mutual Information), and missingness handling strategies.

### 2. Risk Factor Hypotheses (Part D)
Formulated 4 statistically testable research hypotheses ($H_1$ through $H_4$) paired with explicit Null ($H_0$) and Alternative ($H_1$) formulations to guide Exploratory Data Analysis:
* **H1 (Amount-to-Income Ratio):** Transaction amounts exceeding typical monthly income bands present a higher risk review rate
* **H2 (Device Verification Anomaly):** Missing or unverified device types carry a significantly higher probability of risk flags
* **H3 (Account Maturity Risk):** Low-tenure accounts executing high-frequency transfers exhibit elevated risk review rates
* **H4 (Cross-Border Channel Exposure):** Web-based international transactions display higher risk exposure compared to domestic mobile transactions.

### 3. Initial Modelling Plan (Part E)
Established a 9-stage modeling architecture covering:
* **Data Preparation & Scaling:** One-Hot Encoding, Ordinal Mapping, Robust Scaling
* **Hypothesis Testing:** Non-parametric Mann-Whitney U tests and Chi-Square independence tests
* **Feature Engineering:** Ratio engineering, interaction terms, and missingness indicators
* **Model Benchmarks & Algorithms:** Logistic Regression baseline, Ensemble methods (Random Forest, XGBoost), PyTorch MLP, and Unsupervised Anomaly Detection (Isolation Forest, Autoencoders)
* **Evaluation & Validation:** Stratified 5-Fold Cross-Validation evaluated via PR-AUC, F1-Score, and Recall

---

## 📂 Repository Structure

```text
.
├── data/
│   ├── FinTrust_Customer_Data.csv          # Customer demographic & account metadata
│   ├── FinTrust_Data_Dictionary.xlsx       # Feature definitions & data dictionary
│   └── FinTrust_Transaction_Data.csv       # Transaction activity records
├── notebooks/
│   ├── 01_week1_problem_formulation.html  # Exported HTML version of Week 1 notebook
│   └── 01_week1_problem_formulation.ipynb # Main Jupyter Notebook (Phase I & II)
├── .gitignore                              # Git ignore rules for non-tracked files
├── 01_week1_problem_formulation.pdf        # Clean exported PDF report (no code inputs)
├── fintrust_digital_banking.html           # Full HTML export report
└── README.md                               # Main project documentation