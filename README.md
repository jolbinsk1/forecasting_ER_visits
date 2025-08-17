# ED Arrivals Time Series Analysis

## Introduction
This repository contains a **time series analysis and forecasting project** of Emergency Department (ED) arrivals. The project explores trends, seasonality, residuals, and applies multiple forecasting methods, including Exponential Smoothing, ARIMA, KNN, and XGBoost.

## Dataset
- The dataset `ed_time_series.csv` contains daily ED arrival counts.
- The index is a datetime column and the target variable is the number of arrivals per day.
- Summary statistics and initial plots are included to visualize the data.

## Exploratory Data Analysis (EDA)

### 1. Dataset Overview
- Inspected data types, missing values, and displayed summary statistics.
- Created initial line plot of the dataset, showing clear trends over time.

### 2. Autocorrelation Analysis
- Used `plot_correlations` from `sktime` to visualize ACF and PACF.
- Observed strong weekly (lag-7) autocorrelation.

### 3. Seasonal-Trend Decomposition
- Used `STLTransformer` with periods:
  - **7-day** (weekly)
  - **30-day** (monthly)
  - **365-day** (yearly)
- Extracted trend, seasonal, and residual components.
- **Conclusion:** 7-day periodicity preferred for forecasting.

## Forecasting Methods

### 1. Exponential Smoothing (ES)
- Applied with additive trend (`trend='add'`) and weekly seasonality (`sp=7`).
- Forecast horizons: 7-day and 30-day.
- Evaluation metric: **Mean Absolute Percentage Error (MAPE)**.
- Visualizations include actual vs predicted arrivals.
- Observations: upward trend confirmed, weak/no seasonality beyond weekly cycles.

### 2. ARIMA
- Applied for 7-day periodicity.
- Grid search over `p = 0-7`, `d = 0-2`, `q = 0-2`.
- Temporal train-test split with an additional validation set for hyperparameter tuning.
- MAPE used for selecting the best `(p,d,q)` combination.
- Also used AutoARIMA to determine most appropriate parameter values.
- Suppressed excessive warnings during fitting using `sys.stderr` redirection.

### 3. K-Nearest Neighbors (KNN) Regressor
- Applied as a time series regression method.
- Features: past lags of the target variable.
- Hyperparameter tuning using `GridSearchCV`.
- Forecast horizons: 7-day and 30-day.
- MAPE used for evaluation.

### 4. XGBoost Regressor
- Tree-based gradient boosting approach.
- Features: past lags of target.
- Hyperparameter tuning using `GridSearchCV`.
- Applied both 7-day and 30-day forecast horizons.
- MAPE used for evaluation and comparison with other methods.

## Summary of Observations
- Clear **upward trend** in ED arrivals across all periodicities.
- Weekly seasonality evident (lag-7 autocorrelation).
- Residuals analysis suggests 7-day periodicity is most appropriate for modeling.
- Forecasting performance:
  - KNN and XGBoost offer the most robust predictions.

## Visualizations
- Time series plots of ED arrivals.
- STL decomposition for 7-, 30-, and 365-day periods.
- Residuals scatter and histogram plots.
- ACF/PACF plots of residuals.
- Forecast vs actual plots for ES, ARIMA (including AutoARIMA), KNN, and XGBoost.

## Requirements
```text
pandas
numpy
matplotlib
sktime
scikit-learn
xgboost
```


## Usage
1. Load the dataset (ed_time_series.csv).
2. Run exploratory analysis (info(), describe(), plots, ACF/PACF).
3. Perform STL decomposition for chosen periodicities.
4. Split dataset into train and test sets.
5. Fit forecasting models:
- Exponential Smoothing
- ARIMA
- KNN Regressor
- XGBoost Regressor
6. Evaluate forecasts using MAPE and visualize predictions.
