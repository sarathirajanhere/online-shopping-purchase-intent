# Online Shopping Behaviour: Analysing Customer Activity and Predicting Purchase Intent

An IDRA Data Science and AI Capstone Project focused on analysing online shopping behaviour and predicting customer purchase intent using machine learning.

## Project Overview

This project analyses 25,000 online shopping sessions to identify patterns in customer activity and predict whether a session results in a purchase.

The project covers:

- Data Cleaning
- Data Preprocessing
- Exploratory Data Analysis (EDA)
- Statistical Analysis
- Feature Engineering
- Machine Learning
- Model Evaluation
- Findings and Recommendations

## Machine Learning

This is a binary classification problem where the target variable is `purchased`.

Models evaluated:

- Logistic Regression
- Random Forest

The final model selected was **Logistic Regression** based on the held-out F1-score.

### Final Performance

| Metric | Score |
|---|---:|
| Accuracy | 53.8% |
| Precision | 26.5% |
| Recall | 59.7% |
| F1-Score | 36.7% |
| ROC-AUC | 0.567 |

## Repository Structure

```text
│
├── P_10_Ecommerce.csv
├── P10_Ecommerce_cleaned.csv
├── P10_Online_Shopping_Capstone_Report_FINAL_READY.pdf
├── P10_Online_Shopping_Purchase_Intent.ipynb
├── analysis_summary.json
├── top_feature_importance.csv
└── README.md
