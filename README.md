# M5 Sales Forecasting

This project forecasts 28 days of daily Walmart sales for item `FOODS_1_018`, summed across four California stores. It compares a seasonal naive forecast, a SARIMA baseline, and a SARIMAX model with holiday features.

## Data

The notebook uses daily sales, weekly prices, and calendar data from the [M5 Forecasting – Accuracy competition on Kaggle](https://www.kaggle.com/competitions/m5-forecasting-accuracy/data).

## Approach

* Check for weekly seasonality and select a SARIMA baseline using expanding windows and residual diagnostics.
* Screen event, SNAP, and month features by removing one feature block at a time from a Ridge model.
* Compare SARIMAX candidates on 13 expanding windows with 28 days of validation in each. Only models that converge on all 13 folds are eligible.
* Evaluate the selected model and both baselines on the final 28 days.

## Results

The selected SARIMAX model uses Christmas, Easter, and Thanksgiving features. It had lower mean squared error (MSE) than SARIMA in 10 of 13 validation folds. The exploratory paired t-test gave `p = 0.1830`.

|Model|Final 28-day MSE|
|-|-:|
|Seasonal naive|242.8571|
|SARIMA baseline|139.7080|
|SARIMAX with holiday features|139.2832|

SARIMAX had 0.30% lower holdout MSE than SARIMA. Both models had about 42.5% lower MSE than the seasonal naive forecast. The selected holiday features were zero throughout the holdout.

## How to run

1. Install pandas, NumPy, Matplotlib, scikit-learn, SciPy, statsmodels, and Jupyter Notebook.
2. Download `sales_train_evaluation.csv`, `sell_prices.csv`, and `calendar.csv` from Kaggle. Put them in `Data/` and create `Results/` if needed.
3. Open [M5 Forecasting.ipynb](Jupyter/M5%20Forecasting.ipynb) with `Jupyter/` as the working directory and run all cells.

`LOAD_TRAIN_RESULTS = True` reuses saved results from `Results/` and computes missing entries. Set it to `False` to recompute them.

