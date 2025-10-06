
# 🧮 Credit Scorecard Project (LendingClub)

## 🎯 Objective
Simulate the development of a traditional credit scorecard to assess loan default risk using open-sourced LendingClub personal loan application data. The project mimics real-world model development by applying WOE binning, IV-based feature selection, regularised logistic regression, and out-of-time (OOT) validation.

<br></br>

## ⚙️ Key Features
- 📊 **WOE/IV binning** and transformation
- 📦 **Logistic regression scorecard** with interpretable features
- 🔍 **Lift & gain analysis** to evaluate segmentation performance
- 🧪 Developed using **SageMaker** for scale, reproducibility, and future extensibility
- 🪜 Designed with **business decisioning** in mind (cutoff-ready)

<br></br>

## 📁 Project Structure
│
├── data/                         # Raw, interim, and processed datasets
├── dictionaries/                # Metadata, feature dictionary, binning summary
├── models/logistic/            # Final model, coefficients, parameters
├── notebooks/                  # Jupyter notebooks for EDA, transformation, modelling
├── outputs/                    # Charts, lift/gain, PSI, etc.
├── pipelines/                  # (Optional) ML pipeline components
├── src/                        # (Optional) Source code and modules
├── requirements.txt            # Required packages
├── README.md                   # Project overview

<br></br>

## 📦 Dataset
- **Target**: Loan default flag (`GB_FLAG`) derived from `loan_status`
- **Sample size**: 1.35M approved & completed loans (2012–2018)
- **Development sample**: 2012–2016
- **Out-of-time test**: 2017–2018

<br></br>

## 🔨 Feature Engineering & Selection
- Initial candidate features: 103
- WOE binning + IV filtering
- 3 leakage-prone variables removed (`int_rate`, `loan_amnt`, `term`)
- Final model: 17 predictors selected via L1 regularisation

<br></br>

## 🧪 Model Performance (OOT)

| Metric    | Final Model | Benchmark (with pricing vars) |
|-----------|-------------|-------------------------------|
| AUC       | 0.65        | 0.68                          |
| F1 Score  | 0.40        | 0.41                          |
| Precision | 0.28        | 0.31                          |
| Recall    | 0.70        | 0.63                          |
| KS        | 0.22        | 0.26                          |

The final model performs comparably to the benchmark while avoiding pricing-based leakage variables, improving interpretability and generalisability.

<br></br>

## 🔍 Top Predictive Features

- `fico_range_low`: Lower FICO = higher default risk  
- `verification_status`: Verified income → lower risk  
- `num_rev_tl_bal_gt_0`: More open rev. accounts → lower risk  
- `bc_open_to_buy`: Higher available limit → lower risk  
- `mths_since_rcnt_il`: Longer time since last instalment → lower risk  

📓 See full model development in [`notebooks/03_LogisticRegression.ipynb`](notebooks/03_LogisticRegression.ipynb)

<br></br>

## 🧮 Final Scorecard

The final model was converted into a traditional scorecard using:
- A score range of **300–520**
- Score-to-odds anchor: **500 = odds of 50:1 (Good:Bad)**
- Scaling factor: **50 points to double the odds**

Each applicant receives a final score based on WOE-transformed input variables and associated points per bin. This score can be used by credit strategy teams to guide:
- Cutoff policy (e.g. decline below 560)
- Credit limit assignment
- Pricing tiers

📎 The full scorecard and score-to-bin mapping are available in [`notebooks/04_Scorecard_build.ipynb`](notebooks/04_Scorecard_build.ipynb)

<br></br>
---

## 🛠️ Future Enhancements

- Add a tree-based challenger model (e.g., XGBoost)
- Develop automated cutoff & limit strategy module
- Simulate profit/loss impact using assumptions
<br></br>

---
## 🚀 Setup

```bash
git clone https://github.com/Alexaz666/landingclub-scorecard.git
cd landingclub-scorecard
pip install -r requirements.txt

