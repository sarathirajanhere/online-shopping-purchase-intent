# Online Shopping Behaviour: Analysing Customer Activity and Predicting Purchase Intent

## IDRA Data Science and AI Capstone Project

------------------------------------------------------------------------

## 1. Project Overview

This capstone project analyses online shopping sessions to identify
session-context and browsing signals associated with purchase intent.

The project uses the supplied **Project 10 e-commerce dataset
(`P_10_Ecommerce.csv`)**, containing **25,000 online shopping sessions
and 29 raw columns**. The target variable is `purchased`, where:

-   `1` = completed purchase
-   `0` = no completed purchase

The analysis follows a complete data science workflow:

1.  Data understanding
2.  Data quality and cleaning
3.  Leakage audit
4.  Data preprocessing
5.  Exploratory data analysis (EDA)
6.  Statistical analysis
7.  Feature engineering
8.  Classification modelling
9.  Model evaluation
10. Findings, limitations and recommendations

------------------------------------------------------------------------

## 2. Objectives

-   Audit the supplied dataset for completeness, duplicates and feature
    suitability.
-   Identify features that are appropriate for prediction before
    purchase completion.
-   Perform a leakage audit on potentially outcome-linked variables.
-   Construct a leakage-safe feature set for purchase-intent prediction.
-   Compare classification models using a held-out test set.
-   Interpret the results and provide practical recommendations.

------------------------------------------------------------------------

## 3. Dataset

### Dataset details

  Property               Value
  ---------------------- ----------------------------
  Dataset                `P_10_Ecommerce.csv`
  Rows                   25,000
  Raw columns            29
  Target                 `purchased`
  Purchase sessions      5,616
  Purchase rate          22.5%
  Time coverage          01 Jan 2024 -- 30 Dec 2024
  Missing cells          0
  Exact duplicate rows   0

------------------------------------------------------------------------

## 4. Leakage Audit

A major part of the project was checking whether variables contained
information that would only be available after, or too close to, the
purchase outcome.

The final predictive feature set excludes:

-   `added_to_cart`
-   `payment_method`
-   `rating`
-   `revenue`
-   `revenue_normalized`
-   `cart_abandoned`
-   Review-related fields
-   Identifier fields

In particular, `added_to_cart` and `cart_abandoned` were excluded
because the supplied data do not establish a valid pre-prediction
timestamp and their combinations can directly encode the purchase
outcome.

This prevents artificially inflated model performance and makes the
final model a more defensible estimate of what can be predicted from
session context and browsing information.

------------------------------------------------------------------------

## 5. Final Features

### Categorical features

-   `device_type`
-   `user_type`
-   `marketing_channel`
-   `product_category`
-   `visit_weekday`
-   `visit_month`
-   `visit_season`
-   `session_duration_bucket`

### Numerical features

-   `unit_price`
-   `discount_percent`
-   `pages_viewed`
-   `time_on_site_sec`
-   `pages_per_minute`
-   `discounted_unit_price`

### Engineered features

**`pages_per_minute`**

Measures browsing pace using pages viewed relative to session duration.

**`discounted_unit_price`**

Calculates the effective unit price after applying the recorded discount
percentage.

No target-derived feature was created.

------------------------------------------------------------------------

## 6. Preprocessing

The modelling workflow uses a stratified **80/20 train-test split**:

-   Training set: 20,000 sessions
-   Test set: 5,000 sessions
-   `random_state = 42`

Preprocessing is performed inside the modelling pipeline to avoid
leakage:

-   Numerical variables → median imputation + standardisation
-   Categorical variables → mode imputation + one-hot encoding

The transformations are fitted using the training data and then applied
to the test data.

------------------------------------------------------------------------

## 7. Exploratory Data Analysis

The EDA examines:

-   Purchase versus non-purchase distribution
-   Purchase rate across marketing channels
-   Browsing engagement by purchase outcome
-   Pages viewed
-   Time spent on site
-   Session and product context

The purchase class represents approximately **22.5%** of sessions.

Descriptive analysis showed:

  Measure                       No Purchase   Purchase
  --------------------------- ------------- ----------
  Median pages viewed                  13.0       13.0
  Median time on site (sec)           890.0      942.5

These are descriptive associations and should not be interpreted as
causal effects.

------------------------------------------------------------------------

## 8. Machine Learning

This project is formulated as a **binary classification** problem.

Two models were evaluated:

1.  Logistic Regression
2.  Random Forest

Logistic Regression provides a linear baseline, while Random Forest can
capture non-linear relationships and feature interactions.

The final model selection is based on the **held-out test-set
F1-score**.

------------------------------------------------------------------------

## 9. Final Model Results

### Test-set performance

  Model                   Accuracy   Precision   Recall      F1   ROC-AUC
  --------------------- ---------- ----------- -------- ------- ---------
  Logistic Regression        53.8%       26.5%    59.7%   36.7%     0.567
  Random Forest              72.9%       26.6%    11.8%   16.3%     0.532

**Selected model: Logistic Regression**

The Logistic Regression model was selected because it achieved the
higher held-out F1-score.

### Confusion matrix

For the selected Logistic Regression model:

-   True Negatives: 2,022
-   False Positives: 1,855
-   False Negatives: 453
-   True Positives: 670

### Overfitting assessment

Logistic Regression showed a small training-test accuracy difference:

-   Training accuracy: 53.2%
-   Test accuracy: 53.8%

Random Forest showed a substantially larger gap:

-   Training accuracy: 99.97%
-   Test accuracy: 72.9%

Therefore, the held-out test results are prioritised when interpreting
model performance.

------------------------------------------------------------------------

## 10. Key Findings

-   Purchase sessions represent approximately 22.5% of the supplied
    sessions.
-   Median pages viewed is the same for purchase and non-purchase
    sessions at 13 pages.
-   Purchase sessions have a higher median time on site than
    non-purchase sessions.
-   Marketing-channel purchase rates are relatively close to one another
    in the supplied data.
-   Removing outcome-linked variables substantially changes predictive
    performance and provides a more defensible evaluation.
-   The final model should be treated as a prioritisation aid rather
    than a causal explanation of purchasing behaviour.

------------------------------------------------------------------------

## 11. Limitations

-   The supplied dataset does not provide event timestamps for all
    fields, so the exact prediction point cannot always be established.
-   The analysis is observational and does not demonstrate causal
    relationships.
-   The random train-test split estimates same-source performance;
    time-based validation would provide a stronger assessment for future
    use.
-   Business costs for false positives and false negatives were not
    supplied, so the classification threshold was not optimised for a
    specific business objective.

------------------------------------------------------------------------

## 12. Recommendations and Future Work

1.  Collect timestamped event data so the prediction point can be
    defined precisely.
2.  Validate the model on later sessions before any operational use.
3.  Set the classification threshold according to the business cost of
    false positives and false negatives.
4.  Add pre-session customer-history features only when those features
    are known before the prediction point.
5.  Explore additional models and time-based validation after a stronger
    event-level data structure is available.

------------------------------------------------------------------------

## 13. Project Files

The submission folder contains:

``` text
IDRA Capstone - SARATHI R/
│
├── P10_Online_Shopping_Capstone_Report_FINAL_READY.pdf
├── P10_Online_Shopping_Purchase_Intent.ipynb
├── P_10_Ecommerce.csv
├── P10_Ecommerce_cleaned.csv
├── analysis_summary.json
├── top_feature_importance.csv
└── README.md
```

### File descriptions

  -------------------------------------------------------------------------------------------
  File                                                    Description
  ------------------------------------------------------- -----------------------------------
  `P10_Online_Shopping_Capstone_Report_FINAL_READY.pdf`   Final research-style capstone
                                                          report

  `P10_Online_Shopping_Purchase_Intent.ipynb`             Complete executable analysis
                                                          notebook

  `P_10_Ecommerce.csv`                                    Supplied raw Project 10 dataset

  `P10_Ecommerce_cleaned.csv`                             Cleaned/preprocessed supporting
                                                          dataset

  `analysis_summary.json`                                 Machine-readable summary of the
                                                          final analysis

  `top_feature_importance.csv`                            Feature-importance output from the
                                                          final modelling workflow

  `README.md`                                             Project documentation
  -------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 14. Reproducibility

The Jupyter notebook contains the complete workflow for:

-   Loading the dataset
-   Inspecting data quality
-   Cleaning and preprocessing
-   Performing the leakage audit
-   Engineering features
-   Conducting EDA
-   Training classification models
-   Evaluating the models
-   Generating feature-importance outputs
-   Producing the final analysis summary

The notebook should be run from beginning to end in order to reproduce
the analysis.

------------------------------------------------------------------------

## 15. Academic Integrity

This project was prepared as part of the **IDRA Data Science and AI
Training Program** using the supplied Project 10 dataset.

The report, notebook, analysis and interpretations are intended to
document the analytical workflow and findings of this capstone project.
External sources used for dataset context and software documentation are
acknowledged in the report references.

------------------------------------------------------------------------

## 16. Author

**SARATHI R**\
SRM UNIVERSITY AP\
IDRA Enrollment No.: **IDRA-2026-889462**

**Project:** Online Shopping Behaviour: Analysing Customer Activity and
Predicting Purchase Intent

**September 2026**
