---
layout: post
title: Backtesting a Portfolio
subtitle: Given the stocks, weights and time period, if my strategy worked in the past?
author: Caesar F. Yang
featured-image: /images/2017-09-01/header_portfolio.jpg
tags: [web scraping, data visualization, python, finance]
date-string: SEPTEMBER 01, 2017
---

```python
import datetime as dt
import numpy as np 
import pandas as pd 
import pandas_datareader.data as web 
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
tickers = ['AAPL','BABA', 'FB', 'NKE']

#Array of weights in portfolio
weight = np.array([[0.25, 0.25, 0.25, 0.25]])
```


```python
daily_return = get_return(tickers, start, end, log_return=True)
```


```python
daily_return['Portfolio'] = daily_return.dot(weight.T)
daily_return['SPY'] = get_return('SPY', start, end, log_return=True)
daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>FB</th>
      <th>NKE</th>
      <th>Portfolio</th>
      <th>SPY</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
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
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>-0.025379</td>
      <td>0.024982</td>
      <td>0.004977</td>
      <td>0.013882</td>
      <td>0.004616</td>
      <td>0.001690</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.019764</td>
      <td>-0.016671</td>
      <td>0.002333</td>
      <td>-0.014370</td>
      <td>-0.012118</td>
      <td>-0.012695</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.043121</td>
      <td>-0.061466</td>
      <td>-0.050287</td>
      <td>-0.027033</td>
      <td>-0.045477</td>
      <td>-0.024284</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.005274</td>
      <td>-0.026757</td>
      <td>-0.006044</td>
      <td>-0.016510</td>
      <td>-0.011009</td>
      <td>-0.011037</td>
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
      <th>BABA</th>
      <th>FB</th>
      <th>NKE</th>
      <th>Portfolio</th>
      <th>SPY</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-12-23</th>
      <td>1.100237</td>
      <td>1.080395</td>
      <td>1.105162</td>
      <td>0.832765</td>
      <td>1.040483</td>
      <td>1.137326</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>1.107203</td>
      <td>1.089691</td>
      <td>1.112114</td>
      <td>0.822759</td>
      <td>1.042879</td>
      <td>1.140144</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>1.102471</td>
      <td>1.087573</td>
      <td>1.101794</td>
      <td>0.818416</td>
      <td>1.037462</td>
      <td>1.130682</td>
    </tr>
    <tr>
      <th>2016-12-29</th>
      <td>1.102188</td>
      <td>1.087075</td>
      <td>1.096410</td>
      <td>0.819058</td>
      <td>1.036212</td>
      <td>1.130430</td>
    </tr>
    <tr>
      <th>2016-12-30</th>
      <td>1.093562</td>
      <td>1.093033</td>
      <td>1.084090</td>
      <td>0.815360</td>
      <td>1.031525</td>
      <td>1.126291</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure()
cum_daily_return.plot(grid = True, figsize=(14,10)).axhline(y = 1, color = "black", lw = 1)
plt.ylabel("Cumulative Returns")
plt.legend()
plt.show()
```


    <matplotlib.figure.Figure at 0x116e9cf28>



![png](/images/2017-09-01/output_6_2.png)



```python
cum_daily_return[['Portfolio','SPY']].plot(grid = True, figsize=(14,10)).axhline(y = 1, color = "black", lw = 1)
plt.ylabel("Cumulative Returns")
plt.legend()
plt.show()
```


![png](/images/2017-09-01/output_7_2.png)



