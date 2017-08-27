---
layout: post
title: Stock Data Processing and visualizations (Multiple Stocks)
subtitle: Multiple stocks data processing and visualizations in python
author: Caesar F. Yang
featured-image: /images/2017-08-27/header_multiple.jpg
tags: [data visualization, python, finance]
date-string: August 27, 2017
---


```python
import datetime as dt
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import style
import pandas as pd
import pandas_datareader.data as web
import pandas_datareader as pdr
```


```python
#we're setting a visulization style
style.use('ggplot')
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
tickers = ['AAPL', 'TSLA', 'BABA', 'GOOG']
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
      <th>BABA</th>
      <th>GOOG</th>
      <th>TSLA</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-04</th>
      <td>101.790649</td>
      <td>76.690002</td>
      <td>741.840027</td>
      <td>223.410004</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>99.239845</td>
      <td>78.629997</td>
      <td>742.580017</td>
      <td>223.429993</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>97.297760</td>
      <td>77.330002</td>
      <td>743.619995</td>
      <td>219.039993</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>93.191338</td>
      <td>72.720001</td>
      <td>726.390015</td>
      <td>215.649994</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>93.684120</td>
      <td>70.800003</td>
      <td>714.469971</td>
      <td>211.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate the daily percentage change for `daily_close_px`
daily_pct_change = daily_close_px.pct_change()

# Replace NA values with 0
daily_pct_change.fillna(0, inplace=True)

# Plot the distributions
daily_pct_change.hist(bins=50, sharex=True, figsize=(12,8))

# Show the resulting plot
plt.show()
```


![png](/images/2017-08-27/output_6_0.png)



```python
daily_pct_change.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Ticker</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>GOOG</th>
      <th>TSLA</th>
    </tr>
    <tr>
      <th>Date</th>
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
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>-0.025059</td>
      <td>0.025297</td>
      <td>0.000998</td>
      <td>0.000089</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.019570</td>
      <td>-0.016533</td>
      <td>0.001400</td>
      <td>-0.019648</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.042205</td>
      <td>-0.059615</td>
      <td>-0.023170</td>
      <td>-0.015477</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.005288</td>
      <td>-0.026403</td>
      <td>-0.016410</td>
      <td>-0.021563</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate the cumulative daily returns
cum_daily_return = (1 + daily_pct_change).cumprod()
cum_daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Ticker</th>
      <th>AAPL</th>
      <th>BABA</th>
      <th>GOOG</th>
      <th>TSLA</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-04</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>0.974941</td>
      <td>1.025297</td>
      <td>1.000998</td>
      <td>1.000089</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>0.955861</td>
      <td>1.008345</td>
      <td>1.002399</td>
      <td>0.980440</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>0.915520</td>
      <td>0.948233</td>
      <td>0.979173</td>
      <td>0.965266</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.920361</td>
      <td>0.923197</td>
      <td>0.963105</td>
      <td>0.944452</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plot the cumulative daily returns
cum_daily_return.plot(figsize=(12,8))

# Show the plot
plt.show()
```


![png](/images/2017-08-27/output_9_0.png)
