# prediciting_car_pricing_using_regression_models
In this project, I train, tune and compare several regression models with the goal of finding the best model for accurately predicting used car prices.

## Overview

- Used car sales is a $153.1bn market as of 2021 and grew by about 8% in 2021*
- As more people are turning to online resources for researching and purchasing, websites like KBB, Edmunds, Carvana, Shift and Vroom are experiencing rapid growth**
- Theses platforms rely on accurate pricing models for buying and selling used cars - fortunately for them, they have robust data sets that can be used for price modeling

## Question

- Given the growth in the online car sales, it seems likely that competitors will want to to enter the market
- Competitors will also need accurate pricing models but limited access to data could be a barrier to entry. 
- *Is it possible to build an accurate pricing model from limited data scraped from sales advertisements?*

## The Data

I found an interesting dataset on Kaggle titled "Car Sales Advertisements" which contains data for more than 9.5K cars sold in Ukraine. 
Dataset contains 9576 rows and 10 variables with essential meanings:

- car: manufacturer brand
- price: seller’s price in advertisement (in USD)
- body: car body type   
- mileage: as mentioned in ad
- engV: rounded engine volume (‘000 cubic cm)
- engType: type of fuel (“Other = NA)
- registration: whether car is registered in the Ukraine
- year: year of production
- model: specific model name
- drive: drive type

A full overview of the data can be found here: https://www.kaggle.com/antfarol/car-sale-advertisements

## Method

- Explore the car sales data
- Clean data: remove or transform missing data, remove or transform outliers
- Select and create features 
- Fit and tune several regression models 
- Compare performance of models
- Select the best model for predicting used car pricing 

## Data Exploration

- Data contains a mix of categorical and continuous data (objects, floats and integers)
- Two variables (engV and drive) have null values
- There are also some observations that contain “Other” engType 

## Data Cleaning

Observations containing null and “Other” values make up a small percentage of the total dataset so they were dropped
There were also some outliers that I decided to drop:
- Classic cars - old cars with relatively high price
- Long haul cars - high mileage with relatively high price
- Ultra luxury/sport cars - super high price

## Feature Engineering

The categorical variable “model” contained 795 unique values. To extract some value from the variable, without having to use all 795,  I created a rank called ‘model_class’ for each model within each brand of car using the engType variable. The thinking here is that, in general, within each brand of car, the larger the average engine size of the model,  the nicer the model. 
- 3 = Top class 
- 2 = Standard class
- 1 = Low class

## The Target

- The target for this model is the price variable
- The price variable contained some outliers but by investigating further, I also found the prices are very positively skewed
- I performed a log transformation to normalize the distribution

## Feature Selection

I used a correlation matrix to identify features correlated to the target

## The Models

Regressions are frequently used models for predicting continuous variables like car prices
I trained the following models:
- OLS Regression
- Ridge Regression
- Lasso Regression
- Elastic-Net Regression
- Support Vector Regression
- Random Forest Regression
- Gradient Boost Regression

## Comparing the Results

For each model, I performed a 10 fold Cross-Validation and measured the following: 
- mean CV score
- CV variance
- MAE
- MSE
- RMSE
- MAPE

## Model Selection
I chose the Random Forest regression
- It has the highest mean cross-validation score (.929). However, all the models were pretty close so this wasn’t the only important factor
- It has the lowest MAE, MSE, RMSE and MAPE
- The variance of the cross-validation is okay compared to the other models

## Interpreting the Results
- While the evaluation metrics used work for comparison sake, they do not tell us much about the predictions in terms of actual $$ because the target was log transformed
- I needed to take the inverse of the log on predictions and actuals to see the metrics in the real units
- In this case, the Mean Absolute Error (MAE) converts to about $2809

## Model Usage
- This model could realistically be used to predict used car pricing
- A model like this could be used on platforms like KBB, Edmunds, Carvana
- On the consumer side, this model could be used as a tool for consumers to research car values prior to buying or selling a vehicle
- On the business side, this model could be used as a service for the above consumer, and also as predictive model for obtaining inventory and projecting sales.
- The market for online car sales is growing - startups in the space will need to develop accurate models to compete with existing platforms

## Shortcomings of the Model
- The model over predicts lower prices and under predicts high prices
- The mean model error is about $2800 - this may or may not be a problem depending on total price
- Random Forest models don’t predict beyond the range in the training data
- The model would need to be regularly maintained and trained with new data to keep up with different prices and models outside the range in the initial training set. Another solution would be to create several different variations of the model for different types of cars. For example, for the purpose of this model I dropped classic cars because they were outliers to the larger dataset. It may be possible to have a classic cars version of the model specifically for those cars.
- While you can see feature importance, you cannot get as much detail about the random forest as other regression models - This can make them harder to tune and interpret
