---
layout: post
title: Random Forest Regression on Stock Prices 
date: 2018-08-31 16:28:00
tags: finance python machine-learning
---

In this example, we will use Scikit-Learn's RandomForestRegressor to generate trading signals on Quantopian. 
Using previous day's volume changes, we will predict the current volume change for today. We will generate a new model at the start of end of every week, to be used at the start of the next week. At every minute, we will use the predictions from the four models to trade.

<script src="https://gist.github.com/Jyang772/2bce14fa9685d99b5a2e55d1a9050dd3.js?file=s1.py"></script>


**Input and Output Variables**

The input variable to the models will be the prior __n__ days' volume changes.
The output variable will be the current day's volume change.


**Predictions**

For each model, we run model.predict() and sum up all of the predictions. A positive sum means the volume is predicted to rise, and a negative sum means the volume is predicted to fall. We buy shares of the asset when it is positive, and short the asset when negative. 

Note: volume_changes needs to be reshaped in order to work with zipline-live

<script src="https://gist.github.com/Jyang772/2bce14fa9685d99b5a2e55d1a9050dd3.js?file=s2.py"></script>

<img src="https://i.imgur.com/0sA84cZ.png" alt="Portfolio_Value" width="500"/>


ToDo: upload tearsheet


