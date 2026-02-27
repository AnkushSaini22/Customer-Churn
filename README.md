# Customer Churn Data Processing and Analysis Pipeline

## Overview
This repository contains a comprehensive data processing, feature engineering, and Exploratory Data Analysis (EDA) pipeline designed to prepare complex relational business data for Customer Churn Prediction. The project demonstrates how to take raw data spread across multiple business domains (transactions, support, demographics) and transform it into a flattened, machine-learning-ready master dataset.

## Dataset Structure
The project utilizes a business dataset spread across five distinct tables (Excel sheets), connected via a unique `CustomerID`:
* **Customer_Demographics**: The primary table containing customer attributes (Age, Gender, Marital Status, Income Level).
* **Transaction_History**: Granular records of customer purchases, product categories, and amounts spent.
* **Customer_Service**: Logs of customer support interactions, interaction types (e.g., complaints, inquiries), and resolution statuses.
* **Online_Activity**: User engagement metrics, such as login frequencies.
* **Churn_Status**: The target variable indicating whether a customer has churned (1) or retained (0).

## Key Methodology and Workflow

### 1. Data Integration
* Loaded individual sheets from a unified Excel workbook into separate pandas DataFrames.
* Implemented `Left Joins` to merge transaction, support, and activity logs into the primary demographics table. This ensures that customers with no recent activity or support tickets are retained in the dataset rather than dropped.

### 2. Feature Engineering & Aggregation
Machine learning models require flat, 1-row-per-customer data. To achieve this, the pipeline performs:
* **Spend Aggregation**: Grouped transactional records by customer to calculate a new `TotalSpend` feature.
* **Categorical Encoding**: Applied One-Hot Encoding (`pd.get_dummies`) to event-level categorical features like `ProductCategory`, `InteractionType`, and `ResolutionStatus`.
* **Frequency Mapping**: Aggregated the encoded features using `groupby().sum()`. This converts individual events into customer-level frequency metrics (e.g., predicting churn based on the total number of unresolved complaints a user has filed).

### 3. Data Cleaning
* Dropped non-predictive identifier columns (`TransactionID`, `InteractionID`, Dates) to reduce noise and prevent model overfitting.
* Applied logical imputation for missing values resulting from the joins. For example, replacing `NaN` values in the customer service columns with `0`, correctly implying that the customer made zero complaints.

### 4. Exploratory Data Analysis (EDA)
* **Target Distribution**: Identified a realistic class imbalance, with approximately 79.6% retained customers and 20.4% churned customers.
* **Bivariate Analysis**: Generated normalized cross-tabulations to evaluate churn proportions across categorical variables (Gender, Marital Status, Income Level).
* **Visualizations**: Plotted boxplots to identify the relationship between continuous variables (Age, Login Frequency) and the likelihood of churn, as well as to detect outliers.

## Prerequisites
To run this notebook locally, ensure you have Python 3.x installed along with the following libraries:

```bash
pip install numpy pandas matplotlib seaborn openpyxl
