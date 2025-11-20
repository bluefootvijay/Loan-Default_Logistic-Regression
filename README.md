# LoanTap - Personal Loan Approval Prediction

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Libraries](https://img.shields.io/badge/Libraries-Scikit_Learn%20|%20Statsmodels%20|%20SMOTE-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## Project Overview
Financial institutions face a critical trade-off: approving too many risky loans leads to **Non-Performing Assets (NPAs)**, while being too conservative results in **lost business revenue**. 

This project builds a logistic regression model to predict whether a credit line should be extended to an individual based on their financial profile. The goal is to balance risk and growth by identifying potential defaulters without rejecting good customers.

---

## Table of Contents
- [Project Overview](#-project-overview)
- [Business Problem](#-business-problem)
- [Data Dictionary](#-data-dictionary)
- [Analysis Workflow](#-analysis-workflow)
- [Key Insights](#-key-insights)
- [Model Performance](#-model-performance)
- [Strategic Recommendations](#-strategic-recommendations)
- [Technologies Used](#-technologies-used)

---

## Business Problem
1. **Risk Assessment:** Can we accurately predict if a borrower will "Charge Off" (default) on their loan?
2. **Feature Importance:** What are the primary drivers of default? (e.g., Annual Income, Home Ownership, Loan Term).
3. **Trade-off Analysis:** How do we tune the model to catch defaulters (high recall) while ensuring we don't reject too many good customers (precision)?

---

## Data Dictionary
The dataset consists of historical loan data with the following key features:
* **Target Variable:** `loan_status` (Fully Paid vs. Charged Off).
* **Loan Details:** `loan_amnt`, `term` (36/60 months), `int_rate`, `installment`.
* **Borrower Profile:** `annual_inc`, `home_ownership` (Rent/Mortgage/Own), `emp_length`.
* **Credit History:** `dti` (Debt-to-Income ratio), `revol_bal` (Revolving Balance), `open_acc` (Open Credit Lines).
* **Grade:** `grade` and `sub_grade` assigned by LoanTap.

---

## Analysis Workflow

### 1. Data Preprocessing
* **Cleaning:** Handled missing values and dropped irrelevant columns (e.g., address).
* **Encoding:** Converted categorical variables (e.g., `home_ownership`, `term`) into dummy variables.
* **Imbalance Handling:** The dataset was imbalanced (~80% Fully Paid vs. ~20% Charged Off). **SMOTE (Synthetic Minority Over-sampling Technique)** was used to balance the classes for better model training.

### 2. Exploratory Data Analysis (EDA)
* **Univariate Analysis:** Analyzed distributions of loan amounts, interest rates, and income.
* **Bivariate Analysis:** Compared default rates across different Grades, Home Ownership status, and Loan Purposes.
* **Correlation:** Checked for multicollinearity between features like `loan_amnt` and `installment`.

### 3. Logistic Regression Modeling
* Trained a Logistic Regression model using `scikit-learn`.
* Evaluated performance using Confusion Matrix, Precision, Recall, and ROC-AUC score.
* Tuned the decision threshold to optimize the trade-off between catching defaulters and approving loans.

---

## Key Insights

### 1. Borrower Characteristics
* **Debt Consolidation:** The majority of loans (60%) are taken for debt consolidation, indicating customers are using this to restructure existing liabilities.
* **Loan Tenure:** 77% of loans are for a 36-month term. Longer terms (60 months) are generally associated with higher risk.
* **Housing:** 90% of borrowers either Rent or have a Mortgage; only 10% fully own their homes.

### 2. Drivers of Default
* **High Impact Features:** The most significant predictors of default were:
    * **Annual Income:** Lower income correlates with higher default risk.
    * **Loan Term:** 60-month loans have higher default rates than 36-month loans.
    * **Revolving Balance:** High revolving balances indicate financial stress.
    * **Home Ownership:** Renters are slightly riskier than homeowners.

---

## Model Performance

| Metric | Class 0 (Defaulters) | Class 1 (Fully Paid) |
| :--- | :--- | :--- |
| **Precision** | 0.32 | 0.88 |
| **Recall** | **0.63** | 0.67 |
| **Accuracy** | 65% | |

> **Interpretation:** The model has a Recall of **63%** for defaulters, meaning it successfully catches ~63% of bad loans. However, the low Precision (0.32) means that to catch these bad loans, the model also flags some good customers as risky. This is an acceptable business trade-off to prevent large NPA losses.

---

## Strategic Recommendations

Based on the analysis, the following strategies are recommended:

1.  **Differentiated Approval Process:**
    * **High Grade (A/B):** Fast-track approvals with competitive interest rates to capture market share.
    * **Low Grade (E/F/G):** Implement stricter manual verification for applicants in lower grades, specifically checking their **Debt-to-Income (DTI)** ratios.

2.  **Tenure-Based Policy:**
    * Be cautious with **60-month terms**, as they show a higher correlation with default. Consider capping the loan amount for longer tenures.

3.  **Early Warning System:**
    * Closely monitor borrowers with **high revolving balances** and **rented accommodation**, as they are statistically more likely to default.

4.  **Risk vs. Growth:**
    * While minimizing NPAs is crucial, the bank should not stop disbursing loans entirely. The model threshold can be adjusted to accept slightly more risk in exchange for higher interest income from the "risky but paying" segment.

---

## Technologies Used
* **Python** (Pandas, NumPy)
* **Machine Learning** (Scikit-Learn, SMOTE)
* **Visualization** (Matplotlib, Seaborn)
* **Statistical Modeling** (Statsmodels)
