# Superstore-Order-Return-Analysis-Dashboard
This project analyzes order return patterns within the sample Superstore dataset. Product returns represent a significant cost and operational challenge for businesses. Understanding the drivers behind returns allows for targeted interventions. This project aims to:

1.  Identify characteristics associated with returned orders using Exploratory Data Analysis (EDA).
2.  Build a predictive model to estimate the probability of an order being returned.
3.  Generate a list of high-risk orders for potential review.
4.  Provide a framework for visualizing return insights using a Power BI dashboard.

The analysis is performed using Python and common data science libraries within a Jupyter Notebook.

- Dataset

The analysis uses the following CSV files derived from the sample Superstore dataset:

1.  `sample_-_superstore.xlsx - Orders.csv`: Contains detailed information about each order line item, including customer details, product information, sales figures, and dates.
2.  `sample_-_superstore.xlsx - Returns.csv`: Contains information linking Order IDs to their return status.

- Important Data Limitation:** The provided `Returns` data only indicates if an entire Order ID was associated with a return. It does not specify which product(s) within the order were returned, the date of return, or the reason for return. Therefore, all analysis and prediction in this project are performed at the Order Level.

The `People.csv` file, containing regional manager assignments, was not used in this specific return analysis.

## Methodology

The project follows these key steps within the `Project python file.ipynb` notebook:

1.  Data Loading & Integration:
    -- Loaded the Orders and Returns CSV files using Pandas (handling potential `latin1` encoding).
    -- Merged the datasets based on `Order ID` to create a binary `IsReturned` flag (1 for returned, 0 otherwise) on the Orders data.
2.  Data Cleaning & Feature Engineering:
    -- Converted `Order Date` and `Ship Date` columns to datetime objects.
    -- Calculated `DaysToShip` (Ship Date - Order Date) as a feature.
    -- Handled potential missing/invalid date entries and ensured `DaysToShip` was non-negative.
3.  Exploratory Data Analysis (EDA):
    -- Calculated the overall order return rate.
    -- Analyzed and visualized how return rates vary across key dimensions like `Category`, `Sub-Category`, `Region`, `Segment`, and `Ship Mode` using bar charts (via Matplotlib/Seaborn).
4.  Preprocessing & Feature Selection:
    -- Selected relevant features for modeling (e.g., `Segment`, `Ship Mode`, `Region`, `Category`, `Sub-Category`, `Sales`, `Quantity`, `Discount`, `Profit`, `DaysToShip`). *Note: 'Manufacturer' was considered but excluded as it was not present in the specific data files used.
    -- Separated features (X) and the target variable (y = `IsReturned`).
    -- Applied preprocessing using Scikit-learn's `ColumnTransformer`:
        --- `StandardScaler` for numerical features.
        --- `OneHotEncoder` (handling unknown values) for categorical features.
5.  Model Training:
    -- Split the data into training and testing sets (stratified by `IsReturned`).
    -- Trained a `LogisticRegression` model using Scikit-learn. Incorporated `class_weight='balanced'` to mitigate potential class imbalance issues.
6.  Model Evaluation:
    -- Evaluated the trained model on the test set using:
        --- Classification Report (Precision, Recall, F1-score).
        --- Confusion Matrix.
        --- ROC AUC Score and Curve.
7.  Prediction & Output Generation:
    -- Used the trained model pipeline to predict the probability of return (`PredictedReturnProbability`) for all orders in the dataset.
    -- Filtered orders based on a defined probability threshold (e.g., > 0.6) to identify high-risk orders.
    -- Saved the details of these high-risk orders into the `high_risk_orders.csv` file.

8.  Dashboard Design
    -- Guidance was provided on how to load the results (or raw data) into Power BI.
    -- Instructions included creating DAX measures (`Total Orders`, `Total Returns`, `Overall Return Rate`).
    -- Suggested visuals (KPIs, charts by category/region, tables) and interactive elements (slicers, drill-through filters) for monitoring return patterns.

- Tools & Libraries
  -- Python 3.12
  -- Pandas: Data manipulation and analysis.
  -- NumPy: Numerical computations.
  -- Scikit-learn: Machine learning (preprocessing, modeling, evaluation).
  -- Matplotlib & Seaborn: Data visualization.
  -- Jupyter Notebook: Interactive code development and documentation.
  -- Power BI: (Recommended) For creating the interactive dashboard based on the analysis outputs.

- Results
  -- EDA revealed variations in return rates across different product categories, sub-categories, and regions within the Superstore dataset.
  -- The Logistic Regression model provides a tool to assign a return risk probability score to each order based on its features.
  -- The `high_risk_orders.csv` file provides an actionable list of orders identified by the model as having a higher likelihood of return.
