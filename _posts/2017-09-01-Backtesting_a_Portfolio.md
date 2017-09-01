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
tickers = ['AAPL','BABA', 'FB', 'NKE']

#Array of weights in portfolio
weight = np.array([[0.25, 0.25, 0.25, 0.25]])
```


```python
daily_return = get_return(tickers, start, end)
```


```python
daily_return['Portfolio'] = daily_return.dot(weight.T)
daily_return['SPY'] = get_return('SPY', start, end)
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
      <th>2016-01-05</th>
      <td>-0.025052</td>
      <td>0.025297</td>
      <td>0.004989</td>
      <td>0.013956</td>
      <td>0.004798</td>
      <td>0.001692</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.019549</td>
      <td>-0.016533</td>
      <td>0.002336</td>
      <td>-0.014255</td>
      <td>-0.012000</td>
      <td>-0.012595</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.042240</td>
      <td>-0.059615</td>
      <td>-0.049043</td>
      <td>-0.026596</td>
      <td>-0.044374</td>
      <td>-0.024007</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.005258</td>
      <td>-0.026403</td>
      <td>-0.006025</td>
      <td>-0.016393</td>
      <td>-0.010891</td>
      <td>-0.010997</td>
    </tr>
    <tr>
      <th>2016-01-11</th>
      <td>0.016225</td>
      <td>-0.012429</td>
      <td>0.001849</td>
      <td>0.011458</td>
      <td>0.004276</td>
      <td>0.001021</td>
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
      <td>1.130661</td>
      <td>1.131699</td>
      <td>1.147231</td>
      <td>0.853962</td>
      <td>1.076797</td>
      <td>1.146931</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>1.137833</td>
      <td>1.141479</td>
      <td>1.154471</td>
      <td>0.843662</td>
      <td>1.079283</td>
      <td>1.149803</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>1.133019</td>
      <td>1.139262</td>
      <td>1.143807</td>
      <td>0.839342</td>
      <td>1.073744</td>
      <td>1.140264</td>
    </tr>
    <tr>
      <th>2016-12-29</th>
      <td>1.132724</td>
      <td>1.138740</td>
      <td>1.138231</td>
      <td>0.839841</td>
      <td>1.072402</td>
      <td>1.140007</td>
    </tr>
    <tr>
      <th>2016-12-30</th>
      <td>1.123883</td>
      <td>1.144999</td>
      <td>1.125514</td>
      <td>0.836185</td>
      <td>1.067620</td>
      <td>1.135853</td>
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


    <matplotlib.figure.Figure at 0x11d448e10>



![png](/images/2017-09-01/output_6_1.png)



```python
cum_daily_return[['Portfolio','SPY']].plot(grid = True, figsize=(14,10)).axhline(y = 1, color = "black", lw = 1)
plt.ylabel("Cumulative Returns")
plt.legend()
plt.show()
```


![png](/images/2017-09-01/output_7_0.png)

