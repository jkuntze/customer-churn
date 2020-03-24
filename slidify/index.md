---
title       : Prediction of customer churn
subtitle    : Project for the the course Advanced Data Science Capstone offered by IBM via Coursera.
author      : JKuntze
job         : 
framework   : revealjs        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

# Prediction of 
# Customer churn
<br>
**Advanced Data Science Capstone**
**offered by IBM via Coursera**
<br>
<br>
<br>
JKuntze

---

## The dataset

The dataset was obtained from <a href="https://www.kaggle.com/blastchar/telco-customer-churn" target="_blank" rel="noopener noreferrer">Kaggle</a>
<br>
<br>
From the context section on the Kaggle page: "Predict behavior to retain customers. You can analyze all relevant customer data and develop focused customer retention programs." <br>
[IBM Sample Data Sets]
<br>
<br>
It contains 7043 rows and 21 columns (including the dependent variable).

---

## The use case

Customer churn = loss of clients or customers
<br>
<br>
Key metric for telephone service companies, internet service providers, cable TV firms, insurance firms
<br>
<br>
**WHY?**
<br>
Cost of retaining an existing customer << Cost of acquiring a new one.
<br>
<br>
It would be useful to companies to know which customers are likely to cancel services and contracts ahead of time. This project seeks to predict customer churn.

---

## The solution to the use case

The solution is a report presenting a process to build a model to predict customer churn.
<br>
<br>
The report can be found <a href="https://jkuntze.github.io/customer-churn/" target="_blank" rel="noopener noreferrer">here</a>.
<br>
<br>
The Jupyter Notebooks used to develop the model are available on 
<a href="https://github.com/jkuntze/customer-churn/" target="_blank" rel="noopener noreferrer">github</a>.

---

## Architectural choices

Data were stored in a CSV file. 
<br>
<br>
Data quality assessment, data pre-processing, feature engineering, model definition, model training and model evaluation were performed using Jupyter Notebooks, Python, pandas, Matplotlib, scikit-learn and Keras.
<br>
<br>
Data products: 
- report - Jupyter Notebooks, HTML
- presentation - R and slidify, HTML, CSS
<br>
<br>
The complete Architectural Decisions Document can be found
<a href="https://jkuntze.github.io/customer-churn/#Architectural-Decisions-Document" target="_blank" rel="noopener noreferrer"> here</a>.

---

## Data quality assessment

The dataset codebook can be found <a href="https://jkuntze.github.io/customer-churn/#Dataset-codebook" target="_blank" rel="noopener noreferrer"> here</a>.
<br>
- There were eleven entries showing empty values in the TotalCharges column.
- There were no issues regarding emptyness, uniqueness or set memberships.
- Foreign key set memberships, regular expressions and cross-field validation did not apply to this dataset.

---

## Data pre-processing

- The rows showing empty values in the TotalCharges column were deleted.
- Numerical features were normalized and centered.
<br>
<br>
<br>

## Feature engineering

- One hot encoding was applied to categorical features.

---

## Model performance indicators

Indicators: True positive rate (recall) + F1 score 
<br>
**Recall**: useful to flag customers showing higher churn risk. 
<br>
<br>
<div>$recall = \frac{True Positives}{True Positives + False Negatives}$</div>
<br>
<div>$precision = \frac{True Positives}{True Positives + False Positives}$</div>
<br>
<div>$F_1score = 2*\frac{precision*recall}{precision+recall}$</div>

---

## Model algorithm

<style>
#slide-9 table {
  font-size: 22px;
}
</style>

| Algorithm | Scaled numerical features | Recall Churn = 1 | f1-score Churn = 1 | weighted f1-score |
|------------------------|------|------|------|------|
| Logistic regression    | No   | 0.55 | 0.57 | 0.77 |
| Neural network         | No   | 0.37 | 0.49 | 0.77 |       
| Logistic regression    | Yes  | 0.81 | 0.62 | 0.75 |       
| Neural network         | Yes  | 0.54 | 0.59 | 0.80 | 
| Deep Neural network    | Yes  | 0.54 | 0.59 | 0.80 |
| k-nearest neighbors    | Yes  | 0.62 | 0.61 | 0.79 |
| Support vector machine | Yes  | 0.51 | 0.59 | 0.80 |
| Gradient boosting      | Yes  | 0.58 | 0.62 | 0.81 |

<br>
<p>Logistic regression: best recall performance, simple and easy to interpret.</p>

---

## Model performance

**Logistic regression**

<style>
#slide-10 table {
  font-size: 22px;
}
</style>

dataset | Recall Churn = 1 | f1-score Churn = 1 | weighted f1-score |
--------|------|------|------|
training| 0.79 | 0.62 | 0.76 |  
testing | 0.78 | 0.62 | 0.76 | 

---

## Acknowledgement

Thank you to all instructors of the <a href="https://www.coursera.org/specializations/advanced-data-science-ibm" target="_blank" rel="noopener noreferrer"> Advanced Data Science with IBM Specialization</a>, specially to Romeo Kienzler for his enthusiasm, efforts and time dedicated to the specialization community.





