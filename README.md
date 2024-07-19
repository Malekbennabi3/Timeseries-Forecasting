# Objectives of the project:
The main goal of this project is to use time series data to forecast two critical parameters: oil temperature and exchange rates. Our research is grounded in the analysis of electricity consumption patterns, which serve as a rich
source of time series data. We employ machine learning techniques to identify patterns and trends in this data, which are then used to predict oil temperature and exchange rates.


# Description
This project is divided into 2 parts:

- **Univariate timeseries analysis** :to predict the Oil temperature we will only use one variable.
- **Multivariable timeseries analysis** : to predict the exchange rate of a country we will use other countries exchange rate


# Datasets
We used 2 datasets to perform this work:
-  **ETTh1 dataset**: it consists of three columns (Id, Date and OT) and contains 17,320 times series, spanning from *01/07/2016 00:00* to *22/06/2018 15:00* and There are no missing values.
-  **Exchange Rate dataset**: comprising 7489 daily currency exchange rate time series, each of which represents a different data point (columns [0-6] and OT). However, 25% of the data is missing (33283 missing values out of the original 59912).

# Experimental protocol:

- The first step is to train a predictor to accurately predict the next 100 values, which is a simple univariate prediction problem. To do this, we’ll need to preprocess the data and split the dataset into a subset of subsequences of the form (input,target) for training. The training data consists of a univariate time series with 1 feature.

- The second step is to train a predictor to predict the next 100 values of the 6th variable based on the 7 other variables, so it is a multiivariate prediction problem. In order to overcome the problem of missing values,
 three methods of completion were tried:
  - Mean Completion: Each of the missing values are replaced with the mean of the column df.fillna(df.mean())
  - Median Completion: The missing values are replaced with the mean of the column. df.fillna(df.median())
  - Interpolation Completion: The missing values are filled based on a second order (quadratic) polynomial interpolation. df.interpolate(method=’polynomial’, order=2).fillna(method=’ffill’), we used the second degree polynom to avoid data overfitting.
 
 # Methods description

 - **Prophet**: A Prophet model is created and fitted to the training data. The script then adjusts the predicted values by fitting a quadratic function to the data and generating new y values using this function. These new y values are then assigned back to the ‘yhat’ column in the forecast DataFrame. The script then saves the adjusted predictions and the original predictions to CSV files.

 - **LSTM (Long Short-Term Memory)**: Once the Exchange Rates dataset has been completed by the preceding techniques, we proceeded to analyse the ’6’ column.
In order to ensure that the values were within the appropriate range for LSTM models, we applied a MinMax scaler to manipulate them between 0 and 1. In order to avoid having imbalanced sets, we then split the data
into 65% Train and 35% Test. Subsequently, the input was reshaped to a format of [samples, time steps, features], which is the requisite format for LSTM.
The Sequential model was then created using Keras, which is a type of Recurrent Neural Network (RNN) that employs Long Short-Term Memory (LSTM) units to detect dependecies.

- **Multivariate Prophet** : We employed the previously cited prophet model in a multivariate approach.The dataset, which had been completed by the interpolation method, was imported and the start and end
dates of the training and testing sets were defined. We plotted the different data distribution on the same figure to observe the relation between the variables visually.

![fig21](https://github.com/Malekbennabi3/Timeseries-Forecasting/blob/main/img/fig21.png).


And to understand the relation between the different variables we calculated the correlation between each variable and the ’6’ variable.
![fig22](https://github.com/Malekbennabi3/Timeseries-Forecasting/blob/main/img/fig22.png)


 We notice that the best matching variables are the ’3’, ’OT’ and ’0’.These values are close to 1, indicating that there is a high correlation between the three columns and the ’6’ column.
We fitted the train/test sets into the Prophet model and we initiated the training process, then we defined a forecast time range of 100.
Finally, in order to enhance the predictive capabilities of our model, we incorporated an additional regressor comprising the columns ’0’, ’3’ and ’OT’. We standardised the input values within the regressor and tested the model’s predictions
for the subsequent 100 values on each case.


# Evaluation

To evaluate the different solution’s performance we used the MAE (Mean Absolute Error) metric:

$$ MAE=\frac{1}{N}\sum_{i=0}^{N}\left | Y_{i}-f(X_{i}) \right | $$


# Results

To summarize the previous experiments, we have collected all the results in the next table.
It is worth noting that the use of other correlated variables enhances the accuracy of our prediction, but we should also be careful to not use too many variables to avoid every column’s estimation error (More columns=more error per column).

![Table de resultats](https://github.com/Malekbennabi3/Timeseries-Forecasting/blob/main/img/tab.png)
