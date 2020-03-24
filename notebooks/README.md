# Prediction of customer churn

### J.Kuntze - March 24, 2020

This report highlights the steps taken to predict customer churn at a telecommunications company.

## The dataset

The dataset was obtained from Kaggle:
https://www.kaggle.com/blastchar/telco-customer-churn

From the context section on the Kaggle page: "Predict behavior to retain customers. You can analyze all relevant customer data and develop focused customer retention programs." [IBM Sample Data Sets]

### Dataset codebook
- customerID: Customer ID
- gender: Whether the customer is a male or a female
- SeniorCitizen: Whether the customer is a senior citizen or not (1, 0)
- Partner: Whether the customer has a partner or not (Yes, No)
- Dependents: Whether the customer has dependents or not (Yes, No)
- tenure: Number of months the customer has stayed with the company
- PhoneService: Whether the customer has a phone service or not (Yes, No)
- MultipleLines: Whether the customer has multiple lines or not (Yes, No, No phone service)
- InternetService: Customer’s internet service provider (DSL, Fiber optic, No)
- OnlineSecurity: Whether the customer has online security or not (Yes, No, No internet service)
- OnlineBackup: Whether the customer has online backup or not (Yes, No, No internet service)
- DeviceProtection: Whether the customer has device protection or not (Yes, No, No internet service)
- TechSupport: Whether the customer has tech support or not (Yes, No, No internet service)
- StreamingTV: Whether the customer has streaming TV or not (Yes, No, No internet service)
- StreamingMovies :Whether the customer has streaming movies or not (Yes, No, No internet service)
- Contract: The contract term of the customer (Month-to-month, One year, Two year)
- PaperlessBilling: Whether the customer has paperless billing or not (Yes, No)
- PaymentMethod: The customer’s payment method (Electronic check, Mailed check, Bank transfer (automatic), Credit card (automatic))
- MonthlyCharges: The amount charged to the customer monthly
- TotalCharges: The total amount charged to the customer
- Churn: Whether the customer churned or not (Yes or No)

## The use case

It would be useful to companies to know which customers are likely to cancel services and contracts ahead of time. This project seeks to predict customer churn.

## The solution to the use case

The solution is a report presenting a process to build a model to predict customer churn. The Jupyter Notebooks used to develop the model are available on github.

## Architectural choices

Data were stored in a CSV file. Enterprise data, streaming analytics, data integration, advanced data repository options, security, information governance and systems management were not required.

Data quality assessment, data pre-processing, feature engineering, model definition, model training and model evaluation were performed using Jupyter Notebooks, Python, pandas, Matplotlib, scikit-learn and Keras.

See the complete Architectural Decisions Document further below.

## Data quality assessment, data pre-processing and feature engineering

### Data quality assessment

- There were eleven entries showing empty values in the TotalCharges column.
- Emptyness: there were no empty strings in the customer ID column.
- Uniqueness: there was only one record for each customer ID.
- Set memberships: there were only allowed values listed for categorical variables.

Foreign key set memberships, regular expressions and cross-field validation did not apply to this dataset.

See the following notebooks for details:
- 01.customer_chrun.data_exp.jupyter_notebook.v0
- 03.customer_chrun.feature_eng.jupyter_notebook.v1

### Data pre-processing

- The rows showing empty string values in the TotalCharges column were deleted. 
- Numerical features were normalized and centered.

See the following notebook for details:
- 03.customer_chrun.feature_eng.jupyter_notebook.v1

### Feature engineering

- One hot encoding was applied to categorical features. 

See the following notebook for details:
- 03.customer_chrun.feature_eng.jupyter_notebook.v1

## Model performance indicators

True positive rate (also called recall) and the F1 score were used to evaluate model performance. Recall is relevant because it would be useful to flag customers showing higher churn risk. The indicators are given by the following equations:

recall True Positives/(True Positives + False Negatives)

precision = True Positives/(True Positives + False Positives)

F1_score = 2*precision*recall/(precision+recall)

- Recall is the number of positive predictions divided by the number of positive class values in the test data. It is also called sensitivity, true positive rate, or probability of detection. It measures the proportion of actual positives that are correctly identified as such.
- Precision is the number of positive predictions divided by the total number of positive class values predicted.
- The F1 score conveys the balance between the precision and the recall.

## Model algorithm

As described in the lightweight IBM Cloud Garage Method for data science process model, model definition, training and evaluation are highly iterative steps.

In the model definition step, data were split into training (64%), validation (21%) and testing (15%). Multiple algorithms were considered. See below a table showing the performance of various algorithms on the validation data.

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

**Logistic regression** was retained as the algorithm to create the model in the final training step because it showed the best recall performance, it's simple and it's easy to interpret. 

The table below shows the performance of the logistic regression model trained on a different split of training and testing data (80% / 20%):

dataset | Recall Churn = 1 | f1-score Churn = 1 | weighted f1-score |
--------|------|------|------|
training| 0.79 | 0.62 | 0.76 |  
testing | 0.78 | 0.62 | 0.76 | 

See the following notebooks for details:
- 04.customer_chrun.model_def.jupyter_notebook.v5
- 05.customer_chrun.model_train.jupyter_notebook.v1
- 06.customer_chrun.model_evaluate.jupyter_notebook.v1

# Architectural Decisions Document 
### Project: Predict customer churn
### J.Kuntze - March 24, 2020

This document presents the decisions made to create a system to predict customer churn at a telecommunications company.

This document is based on the following articles written by Romeo Kienzler:

- The Lightweight IBM Cloud Garage Method for Data Science
https://developer.ibm.com/technologies/artificial-intelligence/articles/the-lightweight-ibm-cloud-garage-method-for-data-science

- Architectural decisions guidelines
https://developer.ibm.com/technologies/artificial-intelligence/articles/data-science-architectural-decisions-guidelines


## 1.1 Data Source

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.1.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- Data stored in a CSV file will be used as data source.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.1.2 Justification
Please justify your technology choices here.
- The data set is small enough to be stored locally in a CSV file.


## 1.2 Enterprise Data

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.2.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- No enterprise data will be required in this project.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.2.2 Justification
Please justify your technology choices here.
- Not required because the data is stored locally.

## 1.3 Streaming analytics

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.3.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- Streaming will not be required; analyses will be performed in batches.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.3.2 Justification
Please justify your technology choices here.
- Streaming will not be required; analyses will be performed in batches.

## 1.4 Data Integration 

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.4.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- Additional tools for data integration (such as Apache Spark or SQL databases) are not required.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.4.2 Justification
Please justify your technology choices here.
- Additional tools for data integration are not required due to the limited amount of data.

## 1.5 Data Repository

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.5.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- Data will be stored locally in a CSV file.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.5.2 Justification
Please justify your technology choices here.
- Advanced data repository options are not required due to the limited amount of data.

## 1.6 Discovery and Exploration 

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.6.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- Jupyter Notebook, Python, pandas and Matplotlib.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.6.2 Justification
Please justify your technology choices here.
- Python and Matplotlib are sufficient to explore the data. Jupyter Notebook and Pandas make it easier to accomplish this task and document the findings.

## 1.7 Actionable Insights

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.7.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- Jupyter Notebook, Python, pandas, scikit-learn and Keras.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.7.2 Justification
Please justify your technology choices here.
- I am familiar with the technolopgy listed above. Limited amount data makes it feasible to complete the project using it.

## 1.8 Applications / Data Products

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.8.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- The data products will be a web page containing this project report and the architectural decision document, and a presentation. Jupyter Notebooks, slidify in R, HTML, CSS will be used.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.8.2 Justification
Please justify your technology choices here.
- An analysis report and a presentation are sufficient to describe this project. If the model were to be used in production, it could be part of a scheduled report to assist the customer services department to prioritize customers at risk of terminating their contracts. A dashboard could be provided to management on the percentage of customers at risk of terminating their contracts.

## 1.9 Security, Information Governance and Systems Management

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.9.1 Technology Choice
Please describe what technology you have defined here. Please justify below, why. In case this component is not needed justify below.
- For this project, only the data scientist will have access to the data. The data will be stored locally. 

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.9.2 Justification
Please justify your technology choices here.
- Limited amount of data and open source nature of the dataset do not require additional steps to limit access and manage security of the dataset and data product.

### Acknowledgement

Thank you to all instructors of the Advanced Data Science with IBM Specialization, specially to Romeo Kienzler for his enthusiasm, time and effort dedicated to the specialization community.

Last update on March 24, 2020.
