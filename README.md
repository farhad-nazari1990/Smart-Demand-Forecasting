Taxi Demand Forecasting System

An intelligent forecasting framework for predicting taxi demand across urban zones using historical trip records and machine learning models.

The goal of this project is to build a model that can predict the number of taxi trips starting in each city zone for the next hour. While initially designed for taxi demand, the framework can be applied to a wide range of demand forecasting problems in other industries.

🎯 Project Objective

Input: Historical trip data from January–May 2017, covering 73 urban zones, plus weather data and zone adjacency information.

Output: Forecast of the number of trips originating from each zone for the upcoming hour.

Evaluation Metric: Mean Absolute Error (MAE), both overall and zone-wise.

📂 Dataset Description

Trip demand files (2017-xx_1H_zone.csv):

PUZone: Origin zone ID (0–72).

Count: Number of trips per hour.

PUTime: Date and time of the trip.

weather.csv: Daily weather information (details in weather_description.pdf).

zone_neighbors.json: List of adjacent zones for each region.

test.pkl.gz: Contains test set with:

dt: Timestamp of prediction.

demand: Matrix (73×720) with last 30 days of hourly demand.

weather: Last 30 days of weather features.

neighbors: Mapping of each zone to its neighbors.

test_answer.pkl.gz: Ground truth values for evaluating predictions.

🔍 Exploratory Data Analysis (EDA)

Strong 24-hour periodicity observed in demand.

Peak hours:

Weekdays: 6–10 AM and 4–8 PM.

Weekends: 4–8 PM only.

Weak correlation between neighboring zones.

Weather effects were minor (snow and fog had some influence).

Certain zones consistently showed higher demand.

✨ Feature Engineering

Lagged demand features (last 24 hours, rolling averages over past 30 days).

Day type (weekday vs. weekend).

Peak-hour indicator (combination of hour + day type).

High-demand zone flag (zones historically busier).

Weather and neighbor features were tested but excluded due to low impact.

🤖 Modeling Approach

Training strategy:

First 4 months → training set.

Last month → validation set.

Used time-series cross-validation.

Models evaluated:

Classical time-series: ARIMA, Prophet → poor performance.

Machine learning regressors: LightGBM, XGBoost, Random Forest, Multilayer Perceptron (MLP).

Final solution: Ensemble model (Voting Regressor) combining LightGBM, XGBoost, RF, and MLP.

📊 Results
Model	MAE	Notes
Ensemble (XGB + LGB + RF + MLP)	14.63	Best performance
Multilayer Perceptron	15.05	High accuracy but slow training
Random Forest	15.19	Strong but memory-intensive
XGBoost	15.17	Balanced and stable
LightGBM	15.37	Very fast, decent accuracy
Prophet, Ridge, SVR, etc.	>Baseline	Weak performance
🚀 Conclusion

This project demonstrates how historical demand patterns + machine learning can be leveraged to forecast near-term demand with high accuracy. While focused on taxi trips, the framework can be generalized to other industries such as retail, logistics, restaurants, and online services where anticipating customer demand is crucial.
