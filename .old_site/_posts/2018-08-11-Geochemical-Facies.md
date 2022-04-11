---
layout: post
title: Geochemical Facies Analysis notes
date: 2018-08-11 16:24:58
tags: python machine-learning
---

Some notes on analyzing XRF data using scikit-learn for personal use.

**PREPARE THE DATA** <br>
The data we are analyzing consists of X-ray fluorescence (XRF) measurements of cuttings from a lateral section of an unconventional well. It is given in a comma-separated .csv file.

<img src="https://i.imgur.com/0zKB87v.png" alt="table" width="600px"/>
[source](https://csegrecorder.com/articles/view/geochemical-facies-analysis-using-unsupervised-machine-learning)

One of the most important transformations needed for the data is feature scaling. Machine learning algorithms don't perform well if the different features have very different scales. Note that the depth is on an increasing scale.

```
from sklearn.preprocessing import scale
data = geochem_df.iloc[:, 2:] #dataframe op
data = scale(data)
```

In geochemistry, certain ratios of elements can be used to glean information about physical, chemical or biological effects. In this example, the three ratios we focused on are: Si/Zr, Si/Al, and Zr/Al.

```
geochem_df['Si/Zr'] = geochem_df['SiO2'] / geochem_df['Zr']
geochem_df['Si/Al'] = geochem_df['SiO2'] / geochem_df['Al2O3']
geochem_df['Zr/Al'] = geochem_df['Zr'] / geochem_df['Al2O3']
```

**DIMENSIONALITY REDUCTION**

Determine the most influential factors and reduce the number of features to only those that contribute the most. Some features are highly corralated while others are unimportant for machine learning.

1. Extract and scale data.
2. Determine number of factors using Principle Component Analysis / Common Factor Analysis.
3. Determine k-number of clusters.

<br>
[Jupyter Notebook](https://github.com/Jyang772/jyang772.github.io/blob/master/notebooks/xrf.ipynb)


