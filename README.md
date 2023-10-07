## Introduction

Forecasting plays a huge role in estimating power production from renewable energy sources and its importance has been significantly increased over the past few years. Wind forecasting can be caried out based on physical model or purely statistical model however, the best way is to combine the two techniques and this report discusses how to do this efficiently. 

## Methodology
### Models
Three machine learning algorithms were used to perform wind power forecasting: Linear Regression, Random Forest, and XGBoost. 

###	Evaluation Metrics
Four types of evaluation metrics are used for this assignment. MAE, MSE, RMSE, and R2 score (coefficient of determination). MAE provides the information about the mean error present in our result. It does not consider the presence of outliers in the data which is typical for wind speed data. On the other hand, MSE punishes the outliers but it gives the result in terms of squares so it cannot be interpreted directly. RMSE combines the good attributes of the two metrics. It is basically the square root of MSE, so the results can be easily understood, and it punishes the outliers as well. R2 score quantifies the proportion of variance in the dependent variable. It ranges from 0-1, and it is a positively oriented score (the more the better), all the other metrics discussed above are negatively oriented.

### Forecast Evaluation
The following steps were taken to carry out this prediction assignment.

#### 1. Exploratory Data Analysis:
The training dataset consists of u and v components of wind speed at 10m and 100 m height for an offshore wind power plant located in western Denmark. The dataset has a total of 27,048 rows and only the power (target variable) column has some missing values. Two strategies were tested to tackle the missing value situation persistence and rolling mean. The mean value of power, when imputed using the former technique, was closer to the mean power without any imputation, so this strategy was selected to fill null values. 
Some additional features were added to feed some more meaningful data to the model. Horizontal (u) and Vertical (v) components are provided in the dataset, so the magnitude and direction of wind could be calculated using physical laws. Hellman’s formula (power law) was used to calculate wind speeds at 30m, 50m, and 70m. Again, two strategies were tested, one is to use a constant value for alpha (0.11 for offshore in Denmark) and the other value is variable, and it considers the temporal and spatial variability as well.
Then the outliers were dealt with  using newly formed features, and  wind magnitude at different heights. The respective percentiles replaced all the values lower than the first percentile and higher than the 99th percentile to achieve a more uniform distribution. Finally, the rolling mean and lagging features were added. There exists a strong correlation between the current values and the past values, so it makes sense to create some rolling means and lagging features.

#### 2. Data Modelling:
All the features discussed in the previous step are compiled in one function called `pipeline_func` so it automates our process, and we do not need to perform each task independently. This function takes a dataset and applies the relevant functions automatically.

#### 3. Model Testing:
For model testing a cross-validation function was created. As this problem is a time series problem and the future values depend on the past values,s therefore, we cannot split randomly. Hence, a  time series split was used to split data as shown in the notebook. As the testing data is for one month the split was also made for one month and the rest was training data.

