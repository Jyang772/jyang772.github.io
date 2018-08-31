---
layout: post
title: Random Forest Regression on Stock Prices 
date: 2018-08-11 16:24:58
tags: finance python machine-learning
---

In this example, we will use Scikit-Learn's RandomForestRegressor to generate trading signals on Quantopian. 
Using previous day's volume changes, we will predict the current volume change for today. We will generate a new model at the start of end of every week, to be used at the start of the next week. At every minute, we will use the predictions from the four models to trade.

```
from sklearn.ensemble import RandomForestRegressor
import numpy as np

def initialize(context):
    #context.security = sid(8554) # Trade SPY
    context.security = symbol('GNW')
    context.model1 = RandomForestRegressor()
    context.model2 = RandomForestRegressor()
    context.model3 = RandomForestRegressor()
    context.model4 = RandomForestRegressor()

    context.lookback = 5 # Look back 5 days, was 3 days
    context.history_range = 200 # Only consider the past 200 days' history, was 400

    # Generate a new model every week
    schedule_function(create_models, date_rules.week_end(), time_rules.market_close(minutes=10))

    # Trade 1 minute after the start of every day
    schedule_function(trade, date_rules.every_day(), time_rules.market_open(minutes=1))
```


****Input and Output Variables****

The input variable to the models will be the prior __n__ days' volume changes.
The output variable will be the current day's volume change.


**Predictions**

For each model, we run model.predict() and sum up all of the predictions. A positive sum means the volume is predicted to rise, and a negative sum means the volume is predicted to fall. We buy shares of the asset when it is positive, and short the asset when negative. 

Note: volume_changes needs to be reshaped in order to work with zipline-live

```
def create_models(context, data):
    # Get the relevant daily prices
    ##Changed to volume
    recent_volumes = data.history(context.security, 'volume', context.history_range, '1d').values
    
    # Get the price changes
    # Volume here too
    volume_changes = np.diff(recent_volumes).tolist()

    X = [] # Independent, or input variables
    Y = [] # Dependent, or output variable
    
    # For each day in our history
    for i in range(context.history_range-context.lookback-1):
        X.append(volume_changes[i:i+context.lookback]) # Store prior volume changes
        Y.append(volume_changes[i+context.lookback]) # Store the day's volume change

    print("X: ",X)
    print("Y: ",Y)
    context.model1.fit(X, Y) # Generate our models
    context.model2.fit(X, Y)
    context.model3.fit(X, Y)
    context.model4.fit(X, Y)
    
def trade(context, data):
    if context.model1 and context.model2 and context.model3 and context.model4: # Check if our models are generated
        
        # Get recent prices
        recent_volumes = data.history(context.security, 'volume', context.lookback+1, '1d').values
        
        # Get the price changes
        volume_changes = np.diff(recent_volumes).tolist()
        
        volume_changes = np.reshape(volume_changes,(1,-1))
        
        # Predict using our models and the recent prices
        # Apparently 5 is the maximum here
        prediction1 = context.model1.predict(volume_changes)
        record(prediction1 = prediction1)
        prediction2 = context.model2.predict(volume_changes)
        record(prediction2 = prediction2)
        prediction3 = context.model3.predict(volume_changes)
        record(prediction3 = prediction3)
        prediction4 = context.model4.predict(volume_changes)
        record(prediction4 = prediction4)
        
        
        prediction = prediction1 + prediction2 + prediction3 + prediction4
        
        # Go long if we predict the volume will rise, otherwise short
        if prediction > 0:
            order_target_percent(context.security, 1.0)
        else:
            order_target_percent(context.security, -1.0)
        
```

<img src="https://i.imgur.com/0sA84cZ.png" alt="Portfolio_Value" width="200"/>


