# FinTrust Digital Banking — Risk Review & Behavioral Analytics

## Project Overview
FinTrust Digital Banking seeks to optimize its transaction risk review process by detecting anomalous behavior and potential fraud indicators across customer profiles and transaction metadata.

This repository contains the end-to-end Machine Learning pipeline designed to analyze customer-transaction behaviors, test risk hypotheses, build leak-free domain-engineered features, evaluate baseline and advanced ensemble models under time-aware validation, and select an optimal candidate model for the `Risk_Review_Flag` target variable.

## Key Project Deliverables & Phasing

### Phase I & II: Problem Formulation & Exploratory Data Analysis (Week 1)
* **Candidate Features Assessment**: Merged `Customer` and `Transaction` datasets on `Customer_ID`. Evaluated candidate features across demographic profiles and transactional metadata
* **Risk Factor Hypotheses Formulation**: Formulated and statistically tested 4 core hypotheses ($H_1$ to $H_4$) covering amount-to-income ratios, unverified device flags, account maturity risks, and cross-border channel exposures

### Phase III: Data Preparation, Baseline Modeling & Refinement (Week 2)
* **Dataset Engineering (`exports/prepared_modelling_dataset.csv`)**: Engineered initial predictive features covering temporal indicators, behavioral frequency ratios, and composite risk flags
* **Imbalanced Evaluation Strategy**: Established Precision-Recall AUC (PR-AUC) as the primary evaluation metric due to the ~19.25% class imbalance, benchmarked against a 0.1925 random chance baseline

### Phase IV: Advanced Predictive Modeling, Time-Aware Validation & Candidate Selection (Week 3)

#### 1. Methodological Refinements & Data Leakage Elimination (Part A)
* **Time-Aware Validation Split**: Replaced randomized splitting with an **80/20 Chronological Train/Test Split** to reflect real-world sequential transaction scoring
* **Leak-Free Feature Pipeline**: Re-engineered customer historical baselines (`customer_avg_amount`, `Customer_Tx_Count`, `Amount_To_Customer_Avg_Ratio`) using strictly past cumulative windows (`shift(1)` expanding means) to eliminate temporal data leakage
* **Explicit Outlier Treatment & Target Leakage Control**: Applied IQR capping on `Amount_NGN` to handle extreme spikes cleanly and removed `Transaction_Status` to prevent post-execution target leakage
* **Logic Correction**: Fixed weekend indicator logic (`dayofweek >= 5`) to accurately capture both Saturday and Sunday operations.

#### 2. Advanced Ensemble Modeling & Threshold Tuning (Parts B & D)
* Deployed **XGBoost Classifier** and **LightGBM Classifier** with native class imbalance handling (`scale_pos_weight = 4.08`) and automated F1/Recall decision threshold optimization.
* Evaluated a 4-model candidate matrix under identical chronological test conditions (2,400 test records).

#### 3. Error Analysis & Permutation Model Drivers (Parts E & F)
* **Error Analysis**: Investigated the operational trade-off between **916 False Positives** (customer friction / review queue burden) and **124 False Negatives** (financial risk leakage) on the leading candidate
* **Permutation Feature Importance**: Identified `Transaction_Type`, `Amount_NGN`, `Transaction_Hour`, `International_Transaction`, and engineered `CrossBorder_Web_Interaction` as top empirical PR-AUC drivers.

#### 4. Final Candidate Selection (Part G)
* **Selected Candidate**: **Random Forest Classifier** ($Threshold = 0.4286$) carried forward into Week 4 due to its superior risk capture rate (**Recall = 73.16%**, 338/462 risks caught) and highest **F1-Score (0.3939)**.


## Model Performance Summary (Chronological Test Set — 2,400 Records)

| Model Architecture | Decision Threshold | Accuracy | Precision | Recall (Risk Capture) | F1-Score | PR-AUC | Operational Role & Selection Status |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Random Guess Baseline** | 0.5000 | — | 19.25% | 100.0% | — | **0.1925** | Uninformed reference baseline |
| **Baseline Logistic Regression** | 0.5700 | **70.08%** | **31.71%** | 48.05% | 0.3821 | **0.3360** | Precision-focused / false alarm reducer |
| **Random Forest (Candidate Model)** | **0.4286** | 56.67% | 26.95% | **73.16%** | **0.3939** | **0.3131** | **Selected Candidate**: Maximum risk coverage |
| **XGBoost Classifier** | 0.4786 | 63.04% | 28.38% | 60.39% | 0.3862 | **0.3024** | Balanced sequential gradient boosting |
| **LightGBM Classifier** | 0.4490 | 59.62% | 27.51% | 67.10% | 0.3902 | **0.2967** | Fast leaf-wise gradient boosting |

---

## Repository Structure

```text
.
├── data/
│   ├── FinTrust_Customer_Data.csv                # Customer demographic & account metadata
│   ├── FinTrust_Data_Dictionary.xlsx             # Feature definitions & data dictionary
│   └── FinTrust_Transaction_Data.csv             # Transaction activity records
├── exports/
│   ├── prepared_modelling_dataset.csv            # Deliverable 1: Initial feature dataset
│   └── prepared_modelling_dataset_v2.csv         # Deliverable 1 (v2): Leak-free refined dataset
├── notebooks/
│   ├── 01_week1_problem_formulation.html        # Week 1 notebook export (HTML)
│   ├── 01_week1_problem_formulation.ipynb       # Week 1 Jupyter Notebook
│   ├── 02_week2_data_prep_eda_modeling.html     # Week 2 notebook export (HTML)
│   ├── 02_week2_data_prep_eda_modeling.ipynb    # Main Week 2 Data Prep & Modeling Notebook
│   ├── 03_week3_advanced_modeling_validation.html # Week 3 notebook export (HTML)
│   └── 03_week3_advanced_modeling_validation.ipynb # Week 3 Advanced Modeling Notebook
├── .gitignore                                    # Git ignore rules for non-tracked files
├── 01_week1_problem_formulation.pdf             # Clean exported PDF report (Week 1)
├── 02_week2_data_prep_eda_modeling_report.pdf  # Executive PDF Report (Week 2)
├── 03_week3_advanced_modeling_validation_report.pdf # Executive PDF Report (Week 3)
├── fintrust_digital_banking.html                 # Full compiled HTML project report
└── README.md                                     # Main project documentation