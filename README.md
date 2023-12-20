# Comparison of methods for predicting electricity consumption of a large non-residential building.
<img src="https://github.com/CyrilleSmid/load-prediction-research-notebook/blob/main/results/images/time_series.png" width="800">

## Problem
Problem class: Time series regression </br>
[Input Data:](https://figshare.com/articles/dataset/CU-BEMS_Smart_Building_Electricity_Consumption_and_Indoor_Environmental_Sensor_Datasets/11726517/4) Time series of electricity consumption by sensor, categorised by zone and consumption type </br>
Output data: Prediction of total electricity consumption of the building </br>

## Goals and Objectives

### Goals:
- To compare the performance of a range of classic and ML models
- To determine the feasibility of designing an ensemble of models by floor or zone compared to a single whole building consumption model.

### Objectives:
- Pre-process the dataset.
- Analyse the data
- Build an array of classical and ML models
- Evaluate the performance of the models
- Analyse the viability of data partitioning
- Provide practical recommendations

## Data Preprocessing
- There are missing data and extreme values. 
- Extreme values are removed by the Hampel filter method based on the deviation from the median of the nearest values. 
- Minute-by-minute data is gouped into daily consumption.
- Sensors are summarized into total consumption, consumption by zone or consumption by type. 

## Time series analysis
- Autocorrelation and partial autocorrelation plots are constructed for each series, each indicating weekly seasonality.
- Tests for the presence of stationarity were performed. The only non-stationary series is the socket consumption series, which was further integrated.

## Testing method
Testing models on the entire test set at once is not optimal, as we are often interested in the accuracy of prediction for a specific time interval: the next day, week, month. </br>
We chose a week and divided the test set into 37 weeks, which will be predicted step by step. </br>
Testing is done in the following way: first only on the training set with the prediction of the first week, then on the training set with real data of the first week, on the basis of which the prediction of the second week is made, and so on. </br>

## Used models
- ARIMA
- SARIMAX
- LightGBM
- LSTM
- Transformer

## Results
### Table
<img src="https://github.com/CyrilleSmid/load-prediction-research-notebook/blob/main/results/images/results_table.png" width="500">

### Models MAPE and MAE performance chart
<img src="https://github.com/CyrilleSmid/load-prediction-research-notebook/blob/main/results/images/best_models_chart.png" width="800">

## Best model - LSTM on Total consumption
<img src="https://github.com/CyrilleSmid/load-prediction-research-notebook/blob/main/results/images/best_model_overview.png" width="800">

## Data partitioning viability analysis
Based on the available data, it was found that data partitioning does not improve prediction and furthermore confuses machine learning models by the increased dimensionality of the data. A different approach in designing neural networks for that use case is needed.

## Practical recommendations
- The best performing models, LSTM and Transformers, were sensitive to the increased dimensionality of the data and probably require a different approach to modelling multiple series simultaneously. 
- Data partitioning improved the prediction performance of the classical models only.
