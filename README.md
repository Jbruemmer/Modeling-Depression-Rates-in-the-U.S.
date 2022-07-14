# Modeling-Depression-Rates-in-the-U.S.

## Overview

The goal of this project is to model depression rates in the U.S. This is useful not only for government agencies, but also to better understand what else is correlated with depression. Having a better forecast for depression will allow the government to allocate additional funds to fight the mental health crises. The final project is useful to any mental health organization or government agency that can advocate or provide support for those with depression. 

## Business Understanding

Depression rates in the U.S. have grown significantly in the past two years during Covid. This has a tremendous impact on not only those who experience this disease, it affects those around them, and the Economy as a whole. According to a 2018 study by *[PharmaEconomics](https://link.springer.com/article/10.1007/s40273-021-01019-4)*, depression cost the U.S. economy $326 billion in 2018. This number is rising, not only because of rising rates but because those with the disease are not willing or in many cases unable to receive proper care. 

By modeling Depression Rates more accurately, the CDC will be able to more confidently secure increased funding to help improve the lives of those with this disease and therefore ease the economic and psychological burden of U.S. citizens. 


## Data Understanding

The data used for this analysis is a yearly survey of those diagnosed with Depression. It comes from the World Health Organization from a 2020 report on the Global Burden of Disease from 1990-2019. Additionally, other mental illnesses were tracked during this time. This could prove useful for further analysis of exogenous variables moving forward. 



### EDA
Looking at the graph of mental illness in the U.S. there is little fluctuation between rates during this time period. Additionally, rates are lower than suspected and reported by other disease tracking agencies. This is possibly due to limitations with the survey method and reliant on Doctor's abilities to accurately diagnose these diseases. 

Even still there is some fluctuation in Depression rates in the U.S. that can be modeled.

```
![US Depression Rates Over Time](images/US_Depression_Rates_Over_Time.png "US Depression Rates Over Time")
``` 


## Data Preparation

Attempts were made to make the time series data stationary by using various techniques. These included log and root transformations and various differencing. After attempting multiple times to make the time series stationary, the lowest score achieved on the Dickey-Fuller test was an alpha of .086 meaning I was unable to reject the null hypothesis that the data was not stationary. 

Thankfully, ARIMA models do not require the time series to be stationary, so I was able to move forward with my analysis. 

Before Modelling, I separated the time series so the first 80% of my data (1990-2013) would be a training set and the other part could be used as a testing set (2015-2019. Additionally, a validating set was used to for grid searching

## Data Modeling

The baseline model for this project was a Naive Bayes model that uses the previous years value as a prediction for the next year. This model had an RMSE of .013.

![Baseline_Model](images/Baseline_Model.png "Baseline Model")

At first an ARIMA model was used, and different parameters (values of P, D, and Q) were tested. AIC is a metric that determines how well a model is able to capture the dataset. After attempting a couple of different values, ACF and PACF tests were used to better understand the data. 



The ACF seemed to drop around three while the PACF seemed to be best at 1. While working through some other models that are not in the final notebook, I noticed a trend in the PACF that there was a significant correlation every four years. This interested me because something important and potentially stressful happens every four years. Presidential elections can be a stressful thing for people who are worried about their security and the economy and potentially trigger depression. 

For more research in this topic I suggest reading these articles:
[Headspace](https://www.headspace.com/articles/election-anxiety)
[Washington Post](https://www.washingtonpost.com/lifestyle/wellness/stress-detox-election-anxiety/2020/11/09/96e5974c-1fa7-11eb-90dd-abd0f7086a91_story.html)

As a result of this finding, I wanted to use a SARIMA model that could incorporate a seasonality component in the model.

## Model Iteration

I used a function that would allow me to grid search through a bunch of different parameters in my SARIMAX model in order to find the best one. The models were evaluated by their Akaike information criterion (AIC). This allowed me to determine the best model for this dataset. 

The best model I found was with an order of (2, 1, 1) and a seasonal order of (0, 0, 1, 4)

## Model Evaluation
After cross-validating my final model, I evaluated it on the test set and it returned an RMSE of .0007 as compared to the baseline model which was .013. This is a significant improvement in error reduction by 95%.

Here is a graph of the model and a forecast of eight years past the data set. 


![Final_Model](images/Final_Model.png)

![Forecasted_Rates](images/Forecast.png)

## Recommendations

According to my model we can continue to expect Depression rates to grow over the next 8 years. This is important to the CDC because it shows that the prevalence of this disease is growing. This will allow them to better advocate for resources and expand outreach in order to curb it's spread. Additionally, more resources should be spent around election years because the model sees a trend in depression rates every four years. 

## Next Steps

In order to further assist the CDC, I would like to look at two more aspects of this dataset that could be useful to target outreach in communities that suffer from depression the most. 

Although this data looked at the U.S. population as a whole, it is also broken down into gender and age. Depression in youth is growing faster ([Pew Research](https://www.pewresearch.org/fact-tank/2019/07/12/a-growing-number-of-american-teenagers-particularly-girls-are-facing-depression/#:~:text=The%20total%20number%20of%20teenagers,than%20for%20boys%20%2844%25)) and will have a bigger effect on those who suffer and the economy as a whole. This is due to higher lifetime treatment costs and lower economic potential.

Additionally, women are more likely to suffer from this disorder, and examining men and women separately may provide new insight into why. 

Finally, other disorders are linked to depression so including drug abuse, anxiety disorders, may farther improve the model's accuracy. 

##


