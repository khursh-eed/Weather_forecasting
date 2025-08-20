Air Quality Forecasting using Statistical & Deep Learning Models
📌 Project Overview
This project focuses on air quality forecasting using both classical time series models and deep learning models (LSTMs).
The dataset consists of endogenous (e.g., CO, C6H6) and exogenous meteorological features (Temperature, Relative Humidity, Absolute Humidity, etc.), with strong seasonality identified.

The goal was to build accurate forecasting models for multiple air quality indicators using:

Univariate ARIMA/SARIMA for endogenous series

Multivariate VARIMAX

Gradient Boosted Decision Trees (XGBoost)

Multivariate LSTM networks

📂 Dataset
Original dataset contained several pollutant and sensor time series.

Many columns had over 90% missingness, which were dropped.

Data frequency: Hourly

Missing gaps were imputed using time-based interpolation (instead of mean/mode).

Alternative interpolators like spline were tested but resulted in unrealistic values, so linear interpolation was retained.

Visualization of missingness was done via 3 diagnostic plots.

🔄 Data Preprocessing
Missing Values

Dropped columns with >90% missing values.

Handled missing values in retained features using time-based interpolation.

Stationarity Check

Conducted ADF test: Some series were non-stationary.

Applied first-order differencing to achieve stationarity.

Confirmed via ACF/PACF plots.

Seasonality Analysis

Seasonal Decomposition (additive & multiplicative).

Found 2 main seasonalities in exogenous features.

📊 Modeling Approaches
1️⃣ Classical Models: ARIMA / SARIMA

Used ARIMA on single endogenous variables with exogenous features + seasonality → SARIMA.

Multiple orders (p,d,q)(P,D,Q)s tested.

Example RMSEs:

T: RMSE = 1.04

RH: RMSE = 21.41

Another series: RMSE = 0.33

2️⃣ VARIMAX

Multivariate extension of ARIMA for multiple endogenous series (CO, NOx, Benzene).

Accuracy was worse than SARIMA.

AIC/BIC indicated very high lag orders → risk of overfitting.

Not ideal since dataset includes exogenous features → not pursued further.

3️⃣ Gradient Boosted Trees (XGBoost)

Created lag features for target variables.

Trained XGBoost regression for multiple pollutants.

Results (RMSE):

Feature 1: 0.286

Feature 2: 49.20

Feature 3: 12.35

4️⃣ Multivariate LSTM

Input: Scaled & sequenced data with 24-hour sliding window.

Model:

1 LSTM layer

1 Dense output layer

Activation: tanh

Optimizer: Adam

Captured both endogenous & exogenous dependencies.

Based on attached LSTM_results.xlsx, LSTM outperforms classical models for most pollutants.

📈 Results Summary
Model	Variables	Notes	RMSE (examples)
ARIMA / SARIMA	Univariate + Exogenous	Seasonal orders tuned	T: 1.04, RH: 21.41, Other: 0.33
VARIMAX	Multivariate	Too high lag, less accurate	Not used
XGBoost	With lag features	Some series high error	F1: 0.28, F2: 49.20, F3: 12.35
LSTM	Multivariate (window=24h)	Sequential DL, best performer	(Predictions vs Actual available in LSTM_results.xlsx)
➡️ LSTM provided the best overall generalization, especially compared to VARIMAX and XGBoost.
➡️ SARIMA still produced good results for individual pollutants with strong seasonal structure.

