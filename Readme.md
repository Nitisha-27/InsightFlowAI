# InsightFlow AI

InsightFlow AI is an end-to-end e-commerce data analytics and machine learning project developed using the Brazilian E-Commerce Public Dataset by Olist. The project focuses on transforming raw transactional data into meaningful business insights through data engineering, SQL analytics, business intelligence and predictive customer retention modelling.

The project was developed as a multi-stage data pipeline, starting from raw data collection and ending with analytical reports and machine learning predictions.

## Technologies Used

- **Programming:** Python
- **Data Processing:** Pandas, NumPy
- **Database:** SQLite
- **Query Language:** SQL
- **Machine Learning:** Scikit-learn, XGBoost
- **Hyperparameter Optimization:** Optuna
- **Visualization:** Matplotlib
- **Storage Formats:** CSV, Parquet
- **Development Environment:** Google Colab
- **Data Source:** Kaggle

## Dataset

**Brazilian E-Commerce Public Dataset by Olist**

The dataset contains multiple interconnected CSV files representing different aspects of an e-commerce business.

The following datasets were used:

- Customers
- Orders
- Order Items
- Products
- Sellers
- Payments
- Reviews
- Geolocation
- Product Category Translation

Dataset: [Brazilian E-Commerce Dataset – Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

## How the Project Was Built

### 1. Data Acquisition and Exploration

The project began by downloading the Olist dataset from Kaggle using the Kaggle API in Google Colab.

- Extracted the downloaded ZIP archive.
- Loaded individual CSV files into Pandas DataFrames.
- Examined the structure, dimensions and relationships between datasets.
- Identified primary identifiers and common joining keys.

This helped establish how customers, orders, products, sellers and payments were connected.

### 2. Data Profiling and Cleaning

Before integrating the datasets, individual data quality checks were performed.

The following operations were implemented:

- Checked missing values across all datasets.
- Identified and handled duplicate records.
- Inspected numerical and categorical columns.
- Corrected data types, particularly date and timestamp columns.
- Standardized product category names.
- Translated Portuguese product categories into English.
- Prepared cleaned datasets for integration.

The processed datasets were stored in Parquet format for efficient downstream processing.

### 3. Data Integration and Warehouse Development

Multiple datasets were integrated using their relational keys.

For example:
- Customer information was connected with order records.
- Orders were linked with order items.
- Order items were connected with product and seller information.
- Payment records were associated with their respective orders.
- Reviews were connected to orders.
- Geolocation data was used to enrich geographical information.

Pandas merge and transformation operations were used to combine these datasets into a consolidated analytical warehouse.

The resulting warehouse was used as the foundation for business analytics and machine learning.

### 4. Star Schema and Database Development

To organize the integrated data for analytical querying, a Star Schema was designed.

**Fact Table:**
- `fact_sales`

**Dimension Tables:**
- `dim_customers`
- `dim_products`
- `dim_sellers`
- `dim_reviews`
- `dim_payments`
- `dim_locations`
- `dim_status`
- `dim_date`

The fact table stores transactional sales information, while the dimension tables provide additional descriptive information.

These tables were loaded into an SQLite database named `warehouse.db`.

This created a structured analytical database that could be queried independently of the original CSV files.

### 5. Enterprise SQL Analytics

After creating the database, SQL was used to perform business-oriented analysis.

Queries were developed to investigate:

- Overall sales and revenue.
- Monthly business performance.
- Customer purchasing behaviour.
- Product and category performance.
- Seller performance.
- Payment methods and transaction patterns.
- Delivery performance and delays.
- Customer reviews and satisfaction.
- Geographic sales distribution.

The SQL query results were converted into Pandas DataFrames and exported as CSV reports for further analysis.

### 6. Business Intelligence and Automated Insights

A dedicated Business Intelligence layer was developed using Python, Pandas and SQLite.

The engine connects directly to the analytical warehouse and generates multiple intelligence outputs.

These include:

- Executive KPI Scorecard.
- Customer Intelligence.
- Product Intelligence.
- Seller Intelligence.
- Geographic Intelligence.
- Delivery Intelligence.
- Payment Intelligence.
- Review and Customer Experience Analysis.
- Monthly Business Performance.
- Customer Segment Summary.

Business metrics such as total revenue, average order value, delivery duration, review scores and other performance indicators were calculated.

The generated outputs were saved in both CSV and Parquet formats.

### 7. Customer Retention Dataset Development

The next stage focused on developing a machine learning pipeline to predict future customer repeat purchases.

Instead of creating a simple static dataset, a rolling temporal approach was implemented.

The configuration used:

- **Observation Window:** 180 days.
- **Prediction Window:** 90 days.
- **Snapshot Frequency:** 30 days.

For every snapshot date, historical customer behaviour within the observation window was analysed.

The target variable `RepeatPurchase90D` was created by checking whether the customer made another purchase during the following 90 days.

The resulting dataset contained:

- 402,002 customer-snapshot records.
- 82,780 unique customers.
- 17 temporal snapshots.
- Approximately 0.8% positive target records.

This approach helped preserve the time-based nature of customer purchasing behaviour.

### 8. Feature Engineering

Customer-level features were developed from historical transaction data to represent purchasing behaviour.

A total of 19 features were prepared for model development.

Some important features include:

- Total Orders
- Total Revenue
- Average Order Value
- Maximum Order Value
- Average Basket Size
- Total Items
- Average Freight
- Average Delivery Days
- Average Review Score
- Total Profit Proxy
- Recency Days
- Customer Lifetime Days
- Purchase Frequency
- Order Velocity
- Freight Ratio
- Category Diversity
- Seller Diversity
- Weekend Purchase Ratio

These features were used to help the models identify patterns associated with repeat purchasing.

### 9. Temporal Data Splitting

Since customer retention is a time-dependent problem, a chronological splitting strategy was implemented instead of a random train-test split.

The 17 snapshots were divided into:

- Training: 10 snapshots.
- Validation: 3 snapshots.
- Testing: 4 snapshots.

This ensures that the model is trained on earlier customer activity and evaluated on later time periods.

It also helps reduce the risk of using future information while predicting historical customer behaviour.

### 10. Machine Learning Model Development

Two classification approaches were explored.

**Random Forest Classifier**

- Developed using Scikit-learn.
- Trained on the engineered customer behavioural features.
- Used class weighting to address the highly imbalanced target variable.
- Tuned using Optuna.
- Evaluated using multiple classification metrics.

**XGBoost Classifier**

- Developed using the XGBoost framework.
- Used class imbalance handling through `scale_pos_weight`.
- Optimized hyperparameters using Optuna.
- Evaluated and compared with the Random Forest approach.

### 11. Hyperparameter Optimization and Evaluation

Optuna was used to search for better model configurations.

For Random Forest, parameters such as the following were optimized:

- Number of estimators.
- Maximum tree depth.
- Minimum samples split.
- Minimum samples per leaf.
- Maximum features.
- Class weights.

PR-AUC was selected as the primary optimization metric because the repeat-purchase target was highly imbalanced.

The models were evaluated using:

- ROC-AUC
- PR-AUC
- Precision
- Recall
- F1-score
- Confusion Matrix

The evaluation helped assess how effectively the models identified customers likely to make repeat purchases.

## Key Project Outcomes

- Developed an integrated e-commerce data warehouse.
- Implemented Star Schema-based dimensional modelling.
- Built an SQLite analytical database.
- Performed multi-dimensional SQL business analysis.
- Automated business intelligence report generation.
- Created a rolling temporal customer retention dataset.
- Engineered 19 customer behavioural features.
- Developed Random Forest and XGBoost classification pipelines.
- Applied Optuna for hyperparameter optimization.
- Implemented chronological model validation.

## Project Structure

```text
InsightFlow_AI/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── warehouse/
│
├── reports/
│   ├── sql_results/
│   ├── phase8_business_intelligence/
│   └── phase10b_retention/
│
├── models/
│
├── warehouse.db
│
└── InsightFlowAI.ipynb
```

## Future Improvements

- Develop an interactive business intelligence dashboard.
- Improve customer retention prediction performance.
- Implement SHAP for model explainability.
- Automate data pipeline execution.
- Develop a deployment interface for business users.

## Author

**Nitisha Mishra**

B.Tech – Electronics and Communication Engineering (Biomedical Specialization)

VIT Vellore

GitHub: [Nitisha-27](https://github.com/Nitisha-27)
