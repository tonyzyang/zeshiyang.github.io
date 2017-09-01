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
def get_return(tickers, start, end):
    data = web.DataReader(tickers, 'yahoo', start, end)['Adj Close']
    data = data.sort_index()
    data = np.round(data, decimals=2)
    daily_return = data.pct_change()
    daily_return.dropna(inplace=True)
    return daily_return
```


```python
start = dt.datetime(2016,1,1)
end = dt.datetime(2016, 12, 31)

#list of stocks in portfolio
tickers = ['AAPL','SPY']
```


```python
daily_return = get_return(tickers, start, end)
```


```python
daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
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
      <th>2016-01-05</th>
      <td>-0.025052</td>
      <td>0.001692</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.019549</td>
      <td>-0.012595</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.042240</td>
      <td>-0.024007</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.005258</td>
      <td>-0.010997</td>
    </tr>
    <tr>
      <th>2016-01-11</th>
      <td>0.016225</td>
      <td>0.001021</td>
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


![png](/images/2017-08-28/output_5_0.png)



```python
# Import the OLS model
# Set SPY as my dependent variable, AAPL return as my independent variables
# Print out my OLS model stats result
results = smf.ols('AAPL ~ SPY', data=daily_return).fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   AAPL   R-squared:                       0.324
    Model:                            OLS   Adj. R-squared:                  0.322
    Method:                 Least Squares   F-statistic:                     119.5
    Date:                Fri, 01 Sep 2017   Prob (F-statistic):           5.67e-23
    Time:                        13:24:47   Log-Likelihood:                 752.24
    No. Observations:                 251   AIC:                            -1500.
    Df Residuals:                     249   BIC:                            -1493.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept   1.967e-05      0.001      0.026      0.980      -0.001       0.002
    SPY            1.0238      0.094     10.930      0.000       0.839       1.208
    ==============================================================================
    Omnibus:                       49.134   Durbin-Watson:                   1.684
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              647.494
    Skew:                          -0.100   Prob(JB):                    2.50e-141
    Kurtosis:                      10.866   Cond. No.                         122.
    ==============================================================================
    
    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



```python
plt.figure(figsize=(14,8))
plt.xlabel('SPY daily return')
plt.ylabel('AAPL daily return')
plt.title('Linear Model & Scatter Plot')
plt.plot(daily_return['SPY'], daily_return['AAPL'], '.',
         daily_return['SPY'], results.predict(daily_return['SPY']), '-')
```




    [<matplotlib.lines.Line2D at 0x11d941b70>,
     <matplotlib.lines.Line2D at 0x11d941d30>]




![png](/images/2017-08-28/output_7_1.png)



```python
results.params
```




    Intercept    0.000020
    SPY          1.023777
    dtype: float64




```python
#In this case, I assume the risk free rate is 0, so the pure alpha is just the intercept 
#According to CAPM model, alpha = realized return - [beta(rm-rf)+rf] 
#Numbers would be different if the assumption of risk free rate changed
alpha = results.params[0]
beta = results.params[1]

print ('alpha: ' + str(alpha))
print ('beta: ' + str(beta))
```

    alpha: 1.96704048212e-05
    beta: 1.02377727949



```python
# Construct a portfolio with beta hedging
portfolio = -1*beta*daily_return['SPY'] + daily_return['AAPL']
portfolio.name = "AAPL + Beta Hedge"

daily_return['AAPL'].plot(figsize=(14,8)) 
daily_return['SPY'].plot(figsize=(14,8))
portfolio.plot(figsize=(14,8))
plt.ylabel("Daily Return")
plt.legend()
plt.show()
```


![png](/images/2017-08-28/output_10_0.png)



```python
daily_return['AAPL + Beta Hedge'] = portfolio
```


```python
daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AAPL</th>
      <th>SPY</th>
      <th>AAPL + Beta Hedge</th>
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
      <th>2016-01-05</th>
      <td>-0.025052</td>
      <td>0.001692</td>
      <td>-0.026784</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.019549</td>
      <td>-0.012595</td>
      <td>-0.006654</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.042240</td>
      <td>-0.024007</td>
      <td>-0.017663</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.005258</td>
      <td>-0.010997</td>
      <td>0.016517</td>
    </tr>
    <tr>
      <th>2016-01-11</th>
      <td>0.016225</td>
      <td>0.001021</td>
      <td>0.015181</td>
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
      <th>AAPL</th>
      <th>SPY</th>
      <th>AAPL + Beta Hedge</th>
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
      <td>1.130661</td>
      <td>1.146931</td>
      <td>0.982820</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>1.137833</td>
      <td>1.149803</td>
      <td>0.986535</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>1.133019</td>
      <td>1.140264</td>
      <td>0.990740</td>
    </tr>
    <tr>
      <th>2016-12-29</th>
      <td>1.132724</td>
      <td>1.140007</td>
      <td>0.990710</td>
    </tr>
    <tr>
      <th>2016-12-30</th>
      <td>1.123883</td>
      <td>1.135853</td>
      <td>0.986673</td>
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


![png](/images/2017-08-28/output_14_0.png)




