---
layout: page
title: Power Outage Project
description: Exploration and Analysis of Power Outages in the Continental United States
nav_order: 3
---

# Exploration and Analysis of Power Outages in the Continental United States

by Tom Hocquet and [Julia Ma](https://github.com/Julia1014)

<img src="Power-Outages.gif" alt="GIF" width="180" height="auto">

## Introduction

---

The topic of electricity is important to understand, as it is something that is now essential to daily life. Electricity is used to maintain machinery, electronics, public transportation systems, etc. Electricity also serves as a basis for satisfying fundamental human needs, such as food production, clean water, sanitation, education services, health care, and social services. When power outages occur, these fundamental human needs become at risk. 

In this project, we explored a data set that reports "major outages witnessed by different states in the continental U.S. during January 2000–July 2016." ([source](https://www.sciencedirect.com/science/article/pii/S2352340918307182#bib6).) We specifically want to examine **what attributes are correlated with longer duration of major power outages in the U.S.?** Understanding this question will be crucial in finding out how to minimize the number of future power outages. 

### Dataset Introduction

The dataset we will be using provides information on the major power outages that occured in the U.S. from January 2000 to July 2016. Stats-wise, our raw dataset contains 1535 rows, which represent the nunber of major power outage reports, and 56 columns, which represent the number of power outage properties that were recorded. 

The following columns will be revelant to our dataset:

Note: descriptions were taken from [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S2352340918307182#t0005)

| Column Names                       | Description                                                        |
| ---------------------------------- | ------------------------------------------------------------------ |
| `YEAR`                             | Indicates the year when the outage event occurred                  |
| `MONTH`                            | Indicates the month when the outage event occurred                 |
| `U.S._STATE`                       | Represents all the states in the continental U.S.                  |
| `POSTAL.CODE`                      | Represents the postal code of the U.S. states                      | 
| `NERC.REGION`                      | The North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| `ANOMALY.LEVEL	`                | This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W)                      |
| `CLIMATE.CATEGORY	`    | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)                      |
| `OUTAGE.START`                     | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region) (in Timedelta) |
| `OUTAGE.RESTORATION.DATE`          | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)                  |
| `OUTAGE.RESTORATION`   | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region) (in TimeDelta)                   |
| `CAUSE.CATEGORY`                   | Categories of all the events causing the major power outages                      | 
| `CAUSE.CATEGORY.DETAIL`            | Detailed description of the event categories causing the major power outages                     |
| `OUTAGE.DURATION`                  | Duration of outage events (in minutes)                    |
| `CUSTOMERS.AFFECTED`               | Number of customers affected by the power outage event                      |
| `TOTAL.PRICE`                      | Average monthly electricity price in the U.S. state (cents/kilowatt-hour)                     |

## Cleaning and EDA

---

The data features 1535 rows and 55 columns. That means that their were 1,534 power outages in the continental U.S. in between the dates of January 2000 and July 2016. A lot of rows have some missing values depending what caused the power outages (for example the column `HURRICANE.NAMES` is mostly filled with np.nan as only 72 of the power outages were caused by hurricanes which is about ≈4.6% of the data).

> Dropping unneccessary columns

Some columns, such as the `OBS` and `variables` column, did not contain any information on the actual properties of major power outages, so they were dropped as a result. Some of the outages were also missing the time in which they started and ended. Due to our central question being around the outage duration and their only being 9 rows with missing dates, we decided to drop those rows. 

> Converting datatypes 

Cleaning also included making sure each of the columns were the right type, and if they were not, updating them to their intended types. For instance, the `YEAR` and `MONTH` columns were originally floats, but through cleaning they were converted to ints. 

> Converting to correct datetimes

Certain columns such as the ones regarding `.PRICE` or `PCT_` needed to be converted from objects to floats  

Below shows some of the changed data types of the newly cleaned dataframe:

|                         | 0       |
|:------------------------|:--------|
| YEAR                    | int64   |
| MONTH                   | int64   |
| U.S._STATE              | object  |
| POSTAL.CODE             | object  |
| RES.PRICE               | float64 |
| COM.PRICE               | float64 |
| IND.PRICE               | float64 |
| TOTAL.PRICE             | float64 |
| RES.SALES               | float64 |
| PCT_LAND                | float64 |
| PCT_WATER_TOT           | float64 |
| PCT_WATER_INLAND        | float64 |
| SQUARE.MILES.AFFECTED   | float64 |


Below is the head of the DataFrame of the 5 most relevant columns:

|    |   YEAR |   MONTH | U.S._STATE   | OUTAGE.START.DATE   |   OUTAGE.DURATION |
|---:|-------:|--------:|:-------------|:--------------------|------------------:|
|  0 |   2011 |       7 | Minnesota    | 2011-07-01 00:00:00 |              3060 |
|  1 |   2014 |       5 | Minnesota    | 2014-05-11 00:00:00 |                 1 |
|  2 |   2010 |      10 | Minnesota    | 2010-10-26 00:00:00 |              3000 |
|  3 |   2012 |       6 | Minnesota    | 2012-06-19 00:00:00 |              2550 |
|  4 |   2015 |       7 | Minnesota    | 2015-07-18 00:00:00 |              1740 |

## Exploratory Data Analysis

---

### Univariate Analysis

For the univariate analysis, we decided to look at the columns that were relevent to our research question. In this graph, we see the total number of outages based on their duration.

Note: each grouping of the histogram represents a 12 hour period.

<iframe src="assets/outage-duration-hist4.png" width=800 height=600 frameBorder=0></iframe>

We also looked at whether in different months we see outage of different durations. Here we graphed the mean duration of the outages based on the month they occured in. We can see that outages in the summer last longer on average than the spring and the fall.

<iframe src="assets/outage_duration_month.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

For the bivariate analysis, we looked once more at the outage duration but in relation to their location in the U.S. Below is a map that shows each of the states color coded depending on the average duration.

Note that the bins are not even to better represent the data.

<iframe src="assets/map.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

The following aggregate has `CAUSE.CATEGORY` as the index and `CAUSE.CATEGORY.DETAIL` as the columns. `DETAIL_MISSING = False` represents how many NaNs were proportionality not missing in each `CAUSE.CATEGORY`. `DETAIL_MISSING = False` represents how many NaNs were proportionality missing in each `CAUSE.CATEGORY`. Each column is a separate distribution that adds to 1. This aggregate is significant in investigating whether or not the missingness in`CAUSE.CATEGORY.DETAIL` depends on `CAUSE.CATEGORY`, which will be explored in the later section assessing the missingness in the data.

| CAUSE.CATEGORY                |   DETAIL_MISSING = False |   DETAIL_MISSING = True |
|:------------------------------|-------------------------:|------------------------:|
| equipment failure             |                0.0418288 |               0.0267857 |
| fuel supply emergency         |                0.0243191 |               0.0290179 |
| intentional attack            |                0.347276  |               0.102679  |
| islanding                     |              nan         |               0.0982143 |
| public appeal                 |              nan         |               0.154018  |
| severe weather                |                0.551556  |               0.395089  |
| system operability disruption |                0.0350195 |               0.194196  |

## Assessment of Missingness

### NMAR Analysis

---

One column that could have NMAR data is the "CUSTOMERS.AFFECTED" column. This column measures the number of customers affected by the power outage event and contains 420 NaNs. This missingness could be due to customers not reporting their affectedness by the power outage event. If the severity of power outage cause was small, then there would be less damage done and thus customers would feel less affected and less inclined to report on their affectedness, explaining the NaNs.

If we wanted to explain the missingness, information on how the customers were affected (i.e. through a survey or head count of the area) would be needed. Additional information on the type and severity of the power outage cause could be correlated with how the customers affected was reported, thus making the missingness MAR. 

### Missingness Dependency

---

> Picking a column with non-trivial missingness 

To find a column with non-trivial missingness, we must first define what non-trivial missingness is. In this project, we defined a column to have non-trivial missingness as having 20% or more missing data values in that respective column. The following dataframe shows the top 5 columns that have a the greatest proportion of missing values: 

| column name             |   missing data amount |
|:------------------------|----------------------:|
| HURRICANE.NAMES         |                  0.99 |
| DEMAND.LOSS.MW          |                  0.48 |
| CAUSE.CATEGORY.DETAIL   |                  0.32 |
| CUSTOMERS.AFFECTED      |                  0.3  |
| OUTAGE.RESTORATION.DATE |                  0.04 |

After analyzing the table, the column we decided to base our missingness analysis on was the `CAUSE.CATEGORY.DETAIL` column, which has non-trivial missingness of approximately 32%.

Next, we will use permutation tests to analyze the dependency of the missingness in the `CAUSE.CATEGORY.DETAIL` column against the following columns: `CAUSE.CATEGORY` and `POPULATION` 


> CAUSE.CATEGORY.DETAIL and CAUSE.CATEGORY (MAR)

Null hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `CAUSE.CATEGORY` 

Alternative hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does depend on `CAUSE.CATEGORY` 

Observed test-statistic: Total Variation Distance (TVD)

To recall, `CAUSE.CATEGORY` contains categories of all the events causing the major power outages, and `CAUSE.CATEGORY.DETAIL` contains detailed description of the event categories causing the major power outages. 

The vertical barplot below displays the distributions of `CAUSE.CATEGORY` with and without the missingness in `CAUSE.CATEGORY.DETAIL`. Analyzing the barplot, we notice the distributions are very different. 

<iframe src="assets/missigness1.html" width=800 height=600 frameBorder=0></iframe>

Next, we continue to conduct a permutation test using the Total Variation Distance (TVD) as our observed test statistic.

<iframe src="assets/dist-tvd.html" width=800 height=600 frameBorder=0></iframe>

After 500 permutations of shuffling the `CAUSE.CATEGORY` column and simulating the TVD results, the p-value comes to be 0.0. Our p-value of 0.0 is less than our significance level of 0.01, therefore **we reject the null hypthesis stating that the missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `CAUSE.CATEGORY`**, thus making it **MAR**. 

> CAUSE.CATEGORY.DETAIL and IND.PRICE (MCAR)

Null hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `IND.PRICE` 

Alternative hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does depend on `IND.PRICE` 

Observed test-statistic: Total Variation Distance (TVD)

To recall, `IND.PRICE` contains the monthly electricity price in the industrial sector (cents/kilowatt-hour), and `CAUSE.CATEGORY.DETAIL` contains detailed description of the event categories causing the major power outages. 

**Note*: Since `IND.PRICE` contains numerical data, we decided to bin the prices into 5 categories to make it categorical, and thus be able to use the Total Variation Distance to run our permutation tests. 

| QcutBin                    |   DETAIL_MISSING = False |   DETAIL_MISSING = True |
|:---------------------------|-------------------------:|------------------------:|
| (3.1990000000000003, 5.48] |               0.224708   |               0.162946  |
| (5.48, 6.3]                |               0.197471   |               0.1875    |
| (6.3, 7.32]                |               0.190661   |               0.212054  |
| (7.32, 9.114]              |               0.189689   |               0.209821  |
| (9.114, 27.85]             |               0.191634   |               0.214286  |
| nan                        |               0.00583658 |               0.0133929 |

The vertical barplot below displays the distributions of `IND.PRICE` with and without the missingness in `CAUSE.CATEGORY.DETAIL`. Analyzing the barplot, we notice the distributions are more similar. 

<iframe src="assets/customers-missingness.html" width=800 height=600 frameBorder=0></iframe>

Next, we continue to conduct a permutation test using the Total Variation Distance (TVD) as our observed test statistic.

<iframe src="assets/dist-tvd2.html" width=800 height=600 frameBorder=0></iframe>

After 500 permutations of shuffling the `IND.PRICE` column and simulating the TVD results, the p-value comes to be 1.0. Our p-value of 1.0 is greater than our significance level of 0.01, therefore **we fail to reject the null hypthesis stating that the missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `IND.PRICE`**, thus making it **MCAR**. 

## Hypothesis Testing

--

For our hypothesis testing we have chosen two variables that appear to have a correlation with out question to test whether that is true or not. We looked to see if any of our columns had a graphical correlation with `OUTAGE.DURATION`. After many, *many* graphs, which all seemed to have very little correlation, we started to make different statistics from the columns we have. One such statistic we made is `SQUARE.MILES.AFFECTED`, made from dividing `CUSTOMERS.AFFECTED` by `POPDEN_UC`

Here is a graph of the two variables, with the line of best fit graphed on it as well. 

<iframe src="assets/hypo1.png" width=800 height=600 frameBorder=0></iframe>

From this we constructed the following:

Null Hypothesis (H0): There is no correlation between the two variables.

Alternative Hypothesis (HA): There is a correlation between the two variables.

To test for this, we used the Pearson correlation coefficient to measure our hypothesis. This test the linear relationsip between two numerical variables. We set the alpha to equal 0.01

```py
p_value = (np.abs(permuted_correlations) >= np.abs(observed_correlation)).mean()
```
This returned a p-value of 1e-05

### Permutation Test

---

<iframe src="assets/hypo2.html" width=800 height=600 frameBorder=0></iframe>

We ran the permutation test 100,000 times. The blue stack is our distribution and the red line is the observed value.

### Hypothesis Testing Conclusion

---

The P-Value for this hypothesis test is of 0.000, which is less than our alpha of 0.01. This means that we can reject the null hypothesis.

This could be explained by a mutlitude of factors and unknown variables, so we cannot draw any conclusion other that the variables are correlated. 

# Power Outage Model

## Framing the Problem

---

> Introduction 

Electricity is essential for daily life, as it is used to maintain many modern-day necessities such as machinery, electronics, public transportation systems, etc. It serves as a basis for food production, clean water, sanitation, education services, health care, social services and other fundamental human needs.  When power outages occur, these fundamental human needs become at risk and poses a great issue. 

In this project, we use this [dataset](https://www.sciencedirect.com/science/article/pii/S2352340918307182#bib6) that reports on all the major power outages in the U.S. from 2000 to 2016. Specifically, we want to use the dataset to create a classification model that will aid us in predicting power outages, specifically predicting the cause type of the power outage.

> Classification Model 

We want to attempt to predict the `CAUSE.CATEGORY` using other features found in the dataset (more information on the specific features will be revealed in later parts). Since we are predicting discrete, categorical variables in the `CAUSE.CATEGORY` column, we will be using classification to model our predictions. 

> Multiclass Classification

We will be performing a multiclass classification, since the observations in `CAUSE.CATEGORY` can be classified into 7 different categories (as shown in the value counts series below). 

> Response Variable

The response variable we chose to predict is `CAUSE.CATEGORY`, because it had the most complete data to work with, with 0 missing values. We were also more interested in using classification rather than regression. 

> Model Metric 

The F1-score is the metric we used to evaluate our model. Since the `CAUSE.CATEGORY` column is less balanced and more skewed (as shown in the value counts below), the F1-score is more suitable to evaluate the potential false postives and negatives. The F1-score also considered precision and recall

#### Data Cleaning

> Converting OUTAGE.START and OUTAGE.RESTORATION.DATE

`OUTAGE.START.DATE`, `OUTAGE.START.TIME`, `OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME` are all datetime columns. In order to incorporate them into our model, we decided to convert `OUTAGE.START.TIME` and `OUTAGE.RESTORATION.TIME` to only include the hour the power outage started and was restored. These are now int columns, and the columns were respectively renamed to `START.HOUR` and `END.HOUR`. We also converted `OUTAGE.START.DATE` and `OUTAGE.RESTORATION.DATE` to only include the day of the month the power outage started and was restored. These are also now int columns, and the columns were respectively renamed to `START.DAY` and `END.DAY`. The new columns are shown below:

|    |   START.DAY |   START.HOUR |   END.DAY |   END.HOUR |
|---:|------------:|-------------:|----------:|-----------:|
|  0 |          01 |           17 |        03 |         20 |
|  1 |          11 |           18 |        11 |         18 |
|  2 |          26 |           20 |        28 |         22 |
|  3 |          19 |           04 |        20 |         23 |
|  4 |          18 |           02 |        19 |         07 |

> Filtering Features 

Since we can't choose features that we wouldn't know during the "time of prediction", which in our case means during the time the outage's `CAUSE.CATEGORY` was reported, we must do additional data cleaning to take out those features from the dataset we'll use to train our models. 

The following features cannot be used with the following reasons:

- `OBS`: This column is redundant to the index column. 
- `POSTAL.CODE`: Since we've decided to include the `U.S._STATE`, `POSTAL.CODE` is redundant. 
- `CAUSE.CATEGORY.DETAIL`: This feature can only be recorded after reporting the `CAUSE.CATEGORY`.
- `HURRICANE.NAMES`: This feature can only be recorded after reporting the `CAUSE.CATEGORY`.

> Filtering Column NaNs

In the dataset, we decided to convert the NaNs in every column except `CLIMATE.REGION`, `ANOMALY.LEVEL`, and `CLIMATE.CATEGORY` to -1 to analyze if the missingness in some columns will affect the outcome of our classiifcation model. We decided to use the value of -1 to fill in the NaNs, since every column except `CLIMATE.REGION`, `ANOMALY.LEVEL`, and `CLIMATE.CATEGORY` do not contain negative values, since it wouldn't make sense in the context of the columns. 

Finally, we removed the rows that contained NaNs from `CLIMATE.REGION`, `ANOMALY.LEVEL`, and `CLIMATE.CATEGORY` columns. This removed 14 rows, which is less than 1% of our total rows. Our dataset has no NaNs now!

> Final Cleaned Dataset

Below shows the head of the dataset:

|    |   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND |   START.DAY |   START.HOUR |   END.DAY |   END.HOUR |
|---:|-------:|--------:|:-------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|------------:|-------------:|----------:|-----------:|
|  0 |   2011 |       7 | Minnesota    | MRO           | East North Central |            -0.3 | normal             | severe weather     |              3060 |               -1 |                70000 |       11.6  |        9.18 |        6.81 |          9.28 |     2332915 |     2114774 |     2113291 |       6562520 |      35.5491 |      32.225  |      32.2024 |     2.30874e+06 |          276286 |           10673 |       2.5957e+06  |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |  5.34812e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |          01 |           17 |        03 |         20 |
|  1 |   2014 |       5 | Minnesota    | MRO           | East North Central |            -0.1 | normal             | intentional attack |                 1 |               -1 |                   -1 |       12.12 |        9.71 |        6.49 |          9.28 |     1586986 |     1807756 |     1887927 |       5284231 |      30.0325 |      34.2104 |      35.7276 |     2.34586e+06 |          284978 |            9898 |       2.64074e+06 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |  5.45712e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |          11 |           18 |        11 |         18 |
|  2 |   2010 |      10 | Minnesota    | MRO           | East North Central |            -1.5 | cold               | severe weather     |              3000 |               -1 |                70000 |       10.87 |        8.19 |        6.07 |          8.15 |     1467293 |     1801683 |     1951295 |       5222116 |      28.0977 |      34.501  |      37.366  |     2.30029e+06 |          276463 |           10150 |       2.58690e+06 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |  5.3109e+06  |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |          26 |           20 |        28 |         22 |
|  3 |   2012 |       6 | Minnesota    | MRO           | East North Central |            -0.1 | normal             | severe weather     |              2550 |               -1 |                68200 |       11.79 |        9.25 |        6.71 |          9.19 |     1851519 |     1941174 |     1993026 |       5787064 |      31.9941 |      33.5433 |      34.4393 |     2.31734e+06 |          278466 |           11010 |       2.60681e+06 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |  5.38044e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |          19 |           04 |        20 |         23 |
|  4 |   2015 |       7 | Minnesota    | MRO           | East North Central |             1.2 | warm               | severe weather     |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 |     2028875 |     2161612 |     1777937 |       5970339 |      33.9826 |      36.2059 |      29.7795 |     2.37467e+06 |          289044 |            9812 |       2.67353e+06 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |  5.48959e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |          18 |           02 |        19 |         07 |


## Baseline Model

---

> Model Description

Our baseline model uses K-Nearest Neighbors (KNN) algorithm to analyze the following features on `CAUSE.CATEGORY`: `OUTAGE.DURATION`, `YEAR`, `MONTH`, `U.S._STATE`, `TOTAL.PRICE`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`, `AREAPCT_URBAN`. 

> Feature Descriptions 

Here are the descriptions for each feature (as taken from the dataset [website](https://www.sciencedirect.com/science/article/pii/S2352340918307182)):
- `OUTAGE.DURATION`: Duration of outage events (in minutes)
- `YEAR`: Indicates the year when the outage event occurred
- `MONTH`: Indicates the month when the outage event occurred
- `U.S._STATE`: Represents all the states in the continental U.S.
- `TOTAL.PRICE`: Average monthly electricity price in the U.S. state (cents/kilowatt-hour)
- `TOTAL.SALES`: Total electricity consumption in the U.S. state (megawatt-hour)
- `TOTAL.CUSTOMERS`: Annual number of total customers served in the U.S. state
- `AREAPCT_URBAN`: Percentage of the land area of the U.S. state represented by the land area of the urban areas (in %)

Here are the labels for the data found in each feature: 
- Quantitative: `OUTAGE.DURATION`, `TOTAL.PRICE`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`, `AREAPCT_URBAN` 
- Nominal: `U.S._STATE`
- Ordinal: `YEAR`, `MONTH` 

_*Note_: Although `YEAR` and `MONTH` can argue to be either quantative or categorical, in the context of our classifier model, we are treating `YEAR` and `MONTH` as categorical variables, therefore they are considered to be categorical and ordinal.

Overall, there are 5 quantitative variables, 1 nominal variable, and 2 ordinal variables. 

> Feature Transformations

For this model, we One-Hot Encoded the categorical features `U.S._STATE`, `YEAR`, and `MONTH` to convert them to numerical form using `OneHotEncoder()`. One-Hot Encoding is when we convert each unique value in the categorical feature into a binary, with 1 representing the feature is present and 0 representing the feature is not.

We also used standardized the quantiative feature `TOTAL.SALES` using `StandardScaler()`. This standardizes the total electricity consumption reported in `TOTAL.SALES` by using the z-score (a.k.a. removing the mean and scaling to a unit variance). We decided to standarize because we believe the distribution of `TOTAL.SALES` is not normal. 

> Model Performance

To assess our KNN model performance, we decided to use a Confusion Matrix to analyze the amount of false positives and false negatives our model got. Overall, our model had a 66.32% training accuracy and a 63.95% testing accuracy.

Based on our training and testing accuracy, our current model is not that good. Both the training and testing accuracies average around 63 to 66%, which means our model uses the given features to predict the correct `CAUSE.CATEGORY` only a bit more than half the time. We decided that a model with 80% accuracy or above is considered to be good, so our current model falls below the threshold and is not considered adequate enough. 

To describe more on the Confusion Matrix shown below, our Confusion Matrix reported our model to have a precision score of 0.638 and a recall score of 0.639. There are 243 total true positives (TP) and 108 false positives (FP).

<iframe src="assets/connf_matrix.png" width=800 height=600 frameBorder=0></iframe>

## Final Model

---

> Model Description

For our final model, we chose to use the RandomForestClassifier. In order to improve our accuracy, we want to fine-tune the hyperparameters in our model with much detail, and we found the RandomForestClassifier does the best in allowing for a lot of hyperparameters to be tuned (as compared to a regular linear regression). Since we are trying to predict the categorical feature `CAUSE.CATEGORY`, this was another reason why we decided to use the classification method that is RandomForestClassifier.

We used a hyperparameter search, and it resulted in a depth of 15 and in the grid search we also found that using entropy gave us better results.

Below is a graph of F1 train error and F1 test error based on max_depth. We picked a max_depth of 15 as it has the lowest validation error.
<iframe src="assets/max_depth.html" width=800 height=600 frameBorder=0></iframe>

> Feature Descriptions

Here are the descriptions and reasoning for each feature (descriptions taken from the dataset [website](https://www.sciencedirect.com/science/article/pii/S2352340918307182)): 

- `CUSTOMERS.AFFECTED`: Represents the number of customers affected by the power outage event. If the number of customers affected is too high or too low, this could provide insight into what type of power outage causes that specific amount of customers to be affected. 
- `TOTAL.REALGSP`: Represents the real gross state product (GSP) contributed by all industries (total) (measured in 2009 chained U.S. dollars). If we know how much GSP each industry contributes, coupled with knowing where those industries are located in the U.S., this knowledge could influence certain non weather-based cause categories to happen more or less (i.e. `intentional attack`, `equipment failure`). 
- `TOTAL.PRICE`: Represents the average monthly electricity price in the U.S. state (cents/kilowatt-hour). If the electricity price is lower or higher, then non weather-based power outages (i.e. `intentional attack`, `equipment failure`) could be influenced.  
- `ANOMALY.LEVEL`: Represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. El Nino and La Nina are extreme weather phenomenons that happen around every 3 years. Knowing when they occur can influence what specific weather-based causes of power outages happen.  
- `OUTAGE.DURATION`: Represents the duration of outage events (in minutes). If the duration of the outage is longer or shorter, then that can influence the type of power outage cause. For instance, if a duration is shorter, then it's probably not associated with an extreme-weather based cause.  
- `RES.PERCEN`: Represents the percentage of residential electricity consumption compared to the total electricity consumption in the state (in %). We wanted to not only focus on the generla electricity consumption, but were rather curious what the residential percentage of electricity consumption can influence the causes. The amount of residential electricity consumption can influence non weather-based causes.  
- `MONTH`: Represents the month when the outage event occurred. Certain power outage causes, especially those that are weather-based, can be correlated with the month the power outage was in, because the month can tell us the whether at the time of the month too. 

> Feature Transformations 

Here are the feature transformations used: 

- One-Hot Encode: `U.S._STATE`, `CLIMATE.CATEGORY`
- Standard Scalar: `TOTAL.PRICE`, `OUTAGE.DURATION`

> Model Performance

Overall, our final model performed much better than our baseline model, with a new training accuracy of 100% (which is 1 - our training error of 0.0) and a new testing accuracy of 81.84% (which is 1 - our testing error of 0.18157894736842106). Our model metric, F1-score, reported to have a training error of 0.0 and testing error of 0.156349037818392. This is already a drastic improvement compared to our baseline model's testing and training accuracies, which landed in the 60% range. However something to take note is that our training accuracy is 100% and our F1-score training error is 0%, which could indicate that we overfit the model.


## Fairness Analysis

For our fairness analysis we decided to go with F1 score as it incoporates the fact that our data is uneavenly distributed. We decided to use `CUSTOMERS.AFFECTED` as the column to split into two groups. We split our columns into two groups, group 1 being less than 10,000 people affected and group 2 being more than 10,000. The null hypothesis is that our model is fair and the f1 score for both groups of `CUSTOMER.AFFECTED` is the same. The alternate hypothesis is that our model is unfair and the f1 score for both groups of `CUSTOMER.AFFECTED` is greater for group 2. We picked a significance level of 0.01. We ran the permutation 1000 times and it resulted in a p-value of 0.713. Hence we fail to reject the null hypothesis. We cannot say for sure that our test is fair as the results could be due to random chance. Below is a graph to display our distribution.

<iframe src="assets/permutation2.html" width=800 height=600 frameBorder=0></iframe>

(Note: the graph behaved weirdly as the distribution of values is very small and very close to 0)
