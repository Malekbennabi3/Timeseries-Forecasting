# Objectives of the project:
The main goal of this project is to use time series data to forecast two critical parameters: oil temperature and exchange rates. Our research is grounded in the analysis of electricity consumption patterns, which serve as a rich
source of time series data. We employ machine learning techniques to identify patterns and trends in this data, which are then used to predict oil temperature and exchange rates.


# Description
This project is divided into 2 parts:

- **Univariate timeseries analysis** :to predict the Oil temperature we will only use one variable.
- **Multivariable timeseries analysis** : to predict the exchange rate of a country we will use other countries exchange rate


# Datasets
We used 2 datasets to perform this work:
-  **OT dataset**: it consists of three columns (Id, Date and OT) and contains 17,320 times series, spanning from *01/07/2016 00:00* to *22/06/2018 15:00* and There are no missing values.
-  **Exchange Rate dataset**: comprising 7489 daily currency exchange rate time series, each of which represents a different data point (columns [0-6] and OT). However, 25% of the data is missing (33283 missing values out of the original 59912).

# Experimental protocol:

- The first step is to train a predictor to accurately predict the next 100 values, which is a simple univariate prediction problem. To do this, we’ll need to preprocess the data and split the dataset into a subset of subsequences of the form (input,target) for training. The training data consists of a univariate time series with 1 feature.

- The second step is to train a predictor to predict the next 100 values of the 6th variable based on the 7 other variables, so it is a multiivariate prediction problem. In order to overcome the problem of missing values,
 three methods of completion were tried:
  - Mean Completion: Each of the missing values are replaced with the mean of the column df.fillna(df.mean())
  - Median Completion: The missing values are replaced with the mean of the column. df.fillna(df.median())
  - Interpolation Completion: The missing values are filled based on a second order (quadratic) polynomial interpolation. df.interpolate(method=’polynomial’, order=2).fillna(method=’ffill’), we used the second degree polynom to avoid data overfitting.
