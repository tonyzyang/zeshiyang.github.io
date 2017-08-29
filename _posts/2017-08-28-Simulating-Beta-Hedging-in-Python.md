---
layout: post
title: Simulating Beta Hedging in Python
subtitle: Implementing and plotting the beta hedging strategy based on historical data
author: Caesar F. Yang
featured-image: /images/2017-08-28/header_hedge.jpg
tags: [web scraping, data visualization, python, finance]
date-string: AUGUST 28, 2017
---



```python
import datetime as dt
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import pandas as pd
import pandas_datareader as pdr
import statsmodels.formula.api as smf
```


```python
#we're setting a start and end datetime object
#this will be the range of dates that we're going to grab stock pricing information foR
start = dt.datetime(2016, 1, 1)
end = dt.datetime(2016, 12, 31)
```


```python
def get(tickers, startdate, enddate):
  def data(ticker):
    return (pdr.get_data_yahoo(ticker, start, end))
  datas = map (data, tickers)
  return(pd.concat(datas, keys=tickers, names=['Ticker', 'Date']))
```


```python
tickers = ['AAPL','SPY']
all_data = get(tickers, start, end)
```


```python
# Isolate the `Adj Close` values and transform the DataFrame
daily_close_px = all_data[['Adj Close']].reset_index().pivot('Date', 'Ticker', 'Adj Close')

daily_close_px.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Ticker</th>
      <th>AAPL</th>
      <th>SPY</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-04</th>
      <td>101.790649</td>
      <td>194.990707</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>99.239845</td>
      <td>195.320511</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>97.297760</td>
      <td>192.856674</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>93.191338</td>
      <td>188.229752</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>93.684120</td>
      <td>186.163651</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate the daily percentage change for `daily_close_px`
daily_pct_change = daily_close_px.pct_change()

# Replace NA values with 0
daily_pct_change.fillna(0, inplace=True)

daily_pct_change.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Ticker</th>
      <th>AAPL</th>
      <th>SPY</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-04</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>-0.025059</td>
      <td>0.001691</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.019570</td>
      <td>-0.012614</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.042205</td>
      <td>-0.023992</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.005288</td>
      <td>-0.010976</td>
    </tr>
  </tbody>
</table>
</div>




```python
daily_pct_change.plot(figsize=(14,8))
plt.ylabel("Daily Return")
plt.legend()
plt.show()
```


![png]( /images/2017-08-28/output_6_0.png)



```python
# Import the OLS model
# Set SPY as my dependent variable, AAPL return as my independent variables
# Print out my OLS model stats result
results = smf.ols('AAPL ~ SPY', data=daily_pct_change).fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   AAPL   R-squared:                       0.324
    Model:                            OLS   Adj. R-squared:                  0.321
    Method:                 Least Squares   F-statistic:                     119.9
    Date:                Mon, 28 Aug 2017   Prob (F-statistic):           4.76e-23
    Time:                        18:39:25   Log-Likelihood:                 755.67
    No. Observations:                 252   AIC:                            -1507.
    Df Residuals:                     250   BIC:                            -1500.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept   1.956e-05      0.001      0.026      0.980      -0.001       0.002
    SPY            1.0236      0.093     10.949      0.000       0.839       1.208
    ==============================================================================
    Omnibus:                       49.445   Durbin-Watson:                   1.704
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              656.733
    Skew:                          -0.100   Prob(JB):                    2.47e-143
    Kurtosis:                      10.906   Cond. No.                         123.
    ==============================================================================
    
    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



```python
results.params
```




    Intercept    0.000020
    SPY          1.023594
    dtype: float64




```python
plt.figure(figsize=(14,8))
plt.xlabel('SPY daily return')
plt.ylabel('AAPL daily return')
plt.title('Linear Model & Scatter Plot')
plt.plot(daily_pct_change['SPY'], daily_pct_change['AAPL'], '.',
         daily_pct_change['SPY'], results.predict(daily_pct_change['SPY']), '-')
```




    [<matplotlib.lines.Line2D at 0x11e554e80>,
     <matplotlib.lines.Line2D at 0x11ebcdef0>]




![png]( /images/2017-08-28/output_9_1.png)



```python
alpha = results.params[0]
beta = results.params[1]
```


```python
print ('alpha: ' + str(alpha))
print ('beta: ' + str(beta))
```

    alpha: 1.95600722021e-05
    beta: 1.0235943318



```python
# Construct a portfolio with beta hedging
portfolio = -1*beta*daily_pct_change['SPY'] + daily_pct_change['AAPL']
portfolio.name = "AAPL + Beta Hedge"

daily_pct_change['AAPL'].plot(figsize=(14,8)) 
daily_pct_change['SPY'].plot(figsize=(14,8))
portfolio.plot(figsize=(14,8))
plt.ylabel("Daily Return")
plt.legend()
plt.show()
```


![png]( /images/2017-08-28/output_12_0.png)

