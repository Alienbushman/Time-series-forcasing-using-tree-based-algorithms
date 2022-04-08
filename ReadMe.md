# Introduction of project
### What is the project
This project is a quick overview of how to use Catboost or any tree based model to predict time series data by making use of a lag. The main advantage of this approach is that you are able to add aditional features to the dataset without relying on only the time series information such as with LSTM's.

### Overview of the data
For this example we have 2 sets of data for 15 minute time increments. 
- The number of sales that took place during a period, located in 'data/sales_data.csv' 
- The amount of trafic received during a period, located in  'data/traffic_data.csv' 

### Running the project
The project is currently existing out of 2 sets of jupyter notebooks that run by clicking run all
- Basic_visualisation.ipynb: This is a quick visualisation of the data used for initial testing
- Run_regression_algorithm.ipynb: Run a regression algorithms to predict how much traffic there will be or the number of sales. To filter between the datasets change the filename paramater to the correct location and change the target_column parameter to select the target column for the machine learning model



## Overview of the model
### Results
#### For sales 
Using the catboost model
- The mean square error is 66.27
- The root mean square error is 8.14
- The absolute is error is 3.49

Using the dummy model (predicting the average)
- The mean square error is 255.37
- The root mean square error is 15.98
- The absolute is error is 9.96

#### For traffic
Using the catboost model
- The mean square error is 2.49
- The root mean square error is 1.58
- The absolute is error is 1.06

Using the dummy model (predicting the average)
- The mean square error is 23.36
- The root mean square error is 4.83
- The absolute is error is 3.48

### Data used
The model makes use of 2 sets of information. 
- Historic performance: By making use of a lag we are using the previous 13 time steps as an indication of how the model will perform at the next step
- Date: We use a variety of information related to the date including the day of week, the month, the hour of day and so on as inputs to give the model some context for user behaviour during this time.

The model then processes these two sets of information as features for a Catboost regression model. 
The Catboost model then uses the features to predict what the performance will be for the next 15 minute increment.

### Sanity checks
We made use of two sets of safe guards to ensure that the model's performance can be trusted
- During the test train split: To ensure that none of the training data was in the test dataset, the dataset was split using the first 80% of the samples for the training data and using the last 20% of the data as test data. This makes sure that there are almost no sets of data which contains the lag of the training set and it demonstrates the performance in the real world due to the drift in dataset.
- Evaluating performance: To evalaute performance we benchmarked how well the model did against a dummy model. The dummy model is trained to predict the average perforance for every sample and it demonstrates how much better than an educated guess the model performs.

## Potential modifications to the model
### Adding additional data
One of the main advantages of this architecture is that it allows for great flexibility in which data can be used to improve the performance. For example the traffic data can be combined with the sales data in order to improve the predictions of both or you could add in a weather forcast, whether there is any temporary attractions in the area or any type of information.

### Previous day's performance
Currently the model makes use of a lag that starts immediately, however the lag can be using any timeframe beforehand, which would allow the system to be applied with wider applications, such as predicting what the trafic would be in 2 days time in order to adjust the number of staff needed for a specific time period.
