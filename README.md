# Predict with Zillow the top 5 US Postcodes to invest in the next 10 years from 2017 -> 2027

The Main Goal of this project is to provide analysis and predictions for investing in U.S real estate given the investor's risk and investment profile.

The Data Analyzed is provided by Zillow.com and represents U.S monthly sales data by region, state and zipcode for the periods 1996-04 - 2018-04.

The Target is to predict for the real estate investor the top 5 postcodes to invest in from 2018-04
![header](time-series/mod4_ts0.png)

The Pre-requisites being the real estate investor would like to focus on investing in the city of Chicago. The subset of zipcodes to invest in are those zipcodes that

1] Did not decrease in value by more than the average mean ROI for Chicago zipcodes during the U.S property market crash environ January 2009.

2] Further had the highest ROI in the period following the crash.

3] The real estate investor, according to their risk profile, believe that this would help reduce the risk that any steep increase in property price following the crash did not reflect a steep decrease during the crash period.

## Main Project Files
![header](time-series/mod4_ts19.png)

## Visualise Data
![header](time-series/mod4_ts3.png)

## Zipcode Selection
Examine Data - Property Market - Post Crash
take the top 5 zipcodes for highest ROI% in data_pre_crash_chicago (those zipcodes that were stable
according to the investor pre-requisite, having a ROI during the crash > overall mean for Chicago in this period)

![header](time-series/mod4_ts2.png)

## Model Example Zipcode: 60642

### Rolling Statistics
Plot the zipcode's returns with their respective rolling mean and rolling standard deviation.
Visually test for stationarity.

![header](time-series/mod4_ts10.png)

### Validate Stationarity
Check for stationarity, important for forecasting.  Let's quickly re-articulate what makes a time series non-stationary. There are two major reasons behind non-stationarity of a time series:

Trend: Varying mean over time

Seasonality: Certain variations at specific time-frames

ADFuller test p-value for zipcode: 60642
p-value: 0.7710955432911607
p-value: (-0.9499014278779617, 0.7710955432911607, 6, 45, {'1%': -3.584828853223594, '5%': -2.9282991495198907, '10%': -2.6023438271604937}, 723.7310545354525)
Fail to reject the null hypothesis. Data is not stationary.

The Data is not stationary, attempt to remove trends.

### Remove Trends
Take a look at rolling mean and rolling std now after differencing with Exponentially weighted rolling mean.  Weights are assigned to all the previous values with an exponential decay factor.

![header](time-series/mod4_ts11.png)

After attempting to remove trends through differencing, subtracting the mean the data is not stationary. Use SARIMA model setting enforce_stationarity=False

### Seasonal Decomposition
![header](time-series/mod4_ts17.png)

Here we can see that the trend and seasonality are separated from data and we can model the residuals.
Using time-series decomposition makes it easier to quickly identify a changing mean or variation in the data. The plot above shows the upward trend of our data, along with its yearly seasonality. These can be used to understand the structure of the time series. The intuition behind time series decomposition is important, as many forecasting methods are built upon this concept of structured decomposition to produce forecasts.

### Seasonal Plot

![header](time-series/mod4_ts18.png)



## ACF/PACF
![header](time-series/mod4_ts12.png)

Behaviour of the ACF and PACF for ARMA Models

ACF

Tails off at AR(p)
Cuts off after lag q MA(q)
Tails off ARMA (p,q)
PACF

Cuts off after lag p AR(p)
Tails off MA(q)
Tails off ARMA (p,q)

Auto-Regressive (p) -> Number of autoregressive terms.
Integrated (d) -> Number of nonseasonal differences needed for stationarity.
Moving Average (q) -> Number of lagged forecast errors in the prediction equation.

## Fit SARIMA model and get results
The Auto Regressive Integrated Moving Average (ARIMA) model has the following hyperparameters to account for seasonality, trend, and noise in the data:

Number of auto regressive terms (p): the effects of past values
Number of differences (d): the amount of differencing (see above)
Number of moving average terms (q): the error of the model as a linear combination of the error values observed at previous time points in the past
(P, D, Q) follow the same definition but are applied to the seasonal component of the time series.

The optimal values for these hyperparameters were selected based on prior steps and a gridsearch. The model was then built.

![header](time-series/mod4_ts13.png)

## Train Predicted Results
Validate Train-Test split for Actual vs Predicted results

![header](time-series/mod4_ts14.png)

## Test Predicted Results
![header](time-series/mod4_ts15.png)

## Forecast Model
![header](time-series/mod4_ts16.png)

Forecast ahead from 2017 -> 2027:

Predicted property mean value at initial investment date 2017-05: 589678.76

Predicted property mean value in 1 year: 640020.38 is 8.54 %ROI

Predicted property mean value in 3 years: 739309.23 is 25.37 %ROI

Predicted property mean value in 5 years: 838673.62 is 42.23 %ROI

Predicted property mean value in 10 years: 1087181.87 is 84.37 %ROI


## Final Analysis of 5 Zipcodes
![header](time-series/mod4_ts1.png)


## Recommendations
The results were based on the sales data of zipcodes extracted to suit the pre-requisites of the investors risk profile This analysis may be adjusted to analyse data of a number of permutations of risk profiles including for example:

- Overall country data, by city, state or region code

- At different time periods including the full time period

- Analysis of the seasonal patterns according to the rolling mean can be used to decide the entry point of investment, for example, some zipcodes showed a strong investment entry strategy at the beginning of each year and some showed more than one strong investment entry point at the beginning and mid year. All 5 postcodes showed a seasonality on monthly rolling period of 365 days.

The monthly rolling mean period can be extended to n days, n months, n years, depending on the amount of data being analysed versus prediction power, in order to further remove trends and increase stationarity of data.


 / Flatiron Data Science Course / September 2020
