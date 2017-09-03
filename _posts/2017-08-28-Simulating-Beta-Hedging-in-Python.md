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
import pandas as pd 
import pandas_datareader.data as web 
import statsmodels.formula.api as smf
import matplotlib.pyplot as plt 
from matplotlib import style
%matplotlib inline

style.use('ggplot')
```


```python
def get_return(tickers, start, end, log_return=True):
    data = web.DataReader(tickers, 'yahoo', start, end)['Adj Close']
    data = data.sort_index()
    
    if log_return:
        daily_return = np.log(data.pct_change()+1)
    else:
        daily_return = data.pct_change()
    
    daily_return.fillna(0, inplace=True)

    return daily_return
```


```python
start = dt.datetime(2016,1,1)
end = dt.datetime(2016, 12, 31)

#list of stocks in portfolio
tickers = ['AMZN','SPY']
```


```python
daily_return = get_return(tickers, start, end, log_return=True)
```


```python
daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AMZN</th>
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
      <td>-0.005036</td>
      <td>0.001690</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.001800</td>
      <td>-0.012695</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.039841</td>
      <td>-0.024284</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>-0.001465</td>
      <td>-0.011037</td>
    </tr>
  </tbody>
</table>
</div>




```python
daily_return.plot(figsize=(14,8))
plt.ylabel("Daily Return")
plt.legend()
plt.show()
```


![png](/images/2017-08-28/output_5_2.png)



```python
# Import the OLS model
# Set SPY as my dependent variable, AAPL return as my independent variables
# Print out my OLS model stats result
results = smf.ols('AMZN ~ SPY', data=daily_return).fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   AMZN   R-squared:                       0.230
    Model:                            OLS   Adj. R-squared:                  0.226
    Method:                 Least Squares   F-statistic:                     74.47
    Date:                Sat, 02 Sep 2017   Prob (F-statistic):           7.29e-16
    Time:                        22:50:25   Log-Likelihood:                 684.04
    No. Observations:                 252   AIC:                            -1364.
    Df Residuals:                     250   BIC:                            -1357.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept      0.0001      0.001      0.105      0.917      -0.002       0.002
    SPY            1.0706      0.124      8.629      0.000       0.826       1.315
    ==============================================================================
    Omnibus:                       67.661   Durbin-Watson:                   2.012
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):             2053.016
    Skew:                          -0.070   Prob(JB):                         0.00
    Kurtosis:                      16.982   Cond. No.                         122.
    ==============================================================================
    
    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



```python
plt.figure(figsize=(14,8))
plt.xlabel('SPY daily return')
plt.ylabel('AMZN daily return')
plt.title('Linear Model & Scatter Plot')
plt.plot(daily_return['SPY'], daily_return['AMZN'], '.',
         daily_return['SPY'], results.predict(daily_return['SPY']), '-')
```




    [<matplotlib.lines.Line2D at 0x1145f80b8>,
     <matplotlib.lines.Line2D at 0x1145f8278>]




![png](/images/2017-08-28/output_7_2.png)



```python
results.params
```




    Intercept    0.000106
    SPY          1.070623
    dtype: float64




```python
alpha = results.params[0]
beta = results.params[1]

print ('alpha: ' + str(alpha))
print ('beta: ' + str(beta))
```

    alpha: 0.00010619636426
    beta: 1.07062267236



```python
# Construct a portfolio with beta hedging
portfolio = -1*beta*daily_return['SPY'] + daily_return['AMZN']
portfolio.name = "AMZN + Beta Hedge"

daily_return['AMZN'].plot(figsize=(14,8)) 
daily_return['SPY'].plot(figsize=(14,8))
portfolio.plot(figsize=(14,8))
plt.ylabel("Daily Return")
plt.legend()
plt.show()
```


![png](/images/2017-08-28/output_10_2.png)



```python
daily_return['AMZN + Beta Hedge'] = portfolio
```


```python
daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AMZN</th>
      <th>SPY</th>
      <th>AMZN + Beta Hedge</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-04</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>-0.005036</td>
      <td>0.001690</td>
      <td>-0.006846</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.001800</td>
      <td>-0.012695</td>
      <td>0.011791</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.039841</td>
      <td>-0.024284</td>
      <td>-0.013842</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>-0.001465</td>
      <td>-0.011037</td>
      <td>0.010352</td>
    </tr>
  </tbody>
</table>
</div>




```python
cum_daily_return = (1 + daily_return).cumprod()
cum_daily_return.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AMZN</th>
      <th>SPY</th>
      <th>AMZN + Beta Hedge</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-12-23</th>
      <td>1.145322</td>
      <td>1.137326</td>
      <td>0.998392</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>1.161486</td>
      <td>1.140144</td>
      <td>1.009833</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>1.162584</td>
      <td>1.130682</td>
      <td>1.019761</td>
    </tr>
    <tr>
      <th>2016-12-29</th>
      <td>1.152027</td>
      <td>1.130430</td>
      <td>1.010744</td>
    </tr>
    <tr>
      <th>2016-12-30</th>
      <td>1.128788</td>
      <td>1.126291</td>
      <td>0.994317</td>
    </tr>
  </tbody>
</table>
</div>




```python
cum_daily_return.plot(grid = True, figsize=(14,10)).axhline(y = 1, color = "black", lw = 1)
plt.ylabel("Cumulative Returns")
plt.legend()
plt.show()
```


![png](/images/2017-08-28/output_14_2.png)





