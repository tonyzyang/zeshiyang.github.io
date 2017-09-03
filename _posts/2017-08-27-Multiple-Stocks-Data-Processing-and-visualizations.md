---
layout: post
title: Stock Data Processing and visualizations (Multiple Stocks)
subtitle: Multiple stocks data processing and visualizations in python
author: Caesar F. Yang
featured-image: /images/2017-08-27/header_multiple.jpg
tags: [web scraping, data visualization, python, finance]
date-string: August 27, 2017
---


```python
import datetime as dt
import numpy as np 
import pandas as pd 
import pandas_datareader.data as web 
import seaborn as sns
import matplotlib.pyplot as plt 
from matplotlib import style
%matplotlib inline

style.use('fivethirtyeight')
```


```python
def get_price(tickers, start, end):
    data = web.DataReader(tickers, 'yahoo', start, end)['Adj Close']
    daily_price = data.sort_index()

    return daily_price
```


```python
start = dt.datetime(2016,1,1)
end = dt.datetime(2016, 12, 31)

#list of stocks in portfolio
tickers = ['AAPL', 'BRK-B', 'BTI', 'COP', 'GS', 'LUV', 'NKE', 'PFE', 'TSLA']
```


```python
daily_price = get_price(tickers, start, end)
daily_price.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AAPL</th>
      <th>BRK-B</th>
      <th>BTI</th>
      <th>COP</th>
      <th>GS</th>
      <th>LUV</th>
      <th>NKE</th>
      <th>PFE</th>
      <th>TSLA</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
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
      <td>101.790649</td>
      <td>130.750000</td>
      <td>50.554924</td>
      <td>44.941711</td>
      <td>172.800156</td>
      <td>41.327766</td>
      <td>60.192345</td>
      <td>29.891121</td>
      <td>223.410004</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>99.239845</td>
      <td>131.250000</td>
      <td>50.662033</td>
      <td>45.440212</td>
      <td>169.824875</td>
      <td>41.820232</td>
      <td>61.033783</td>
      <td>30.106300</td>
      <td>223.429993</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>97.297760</td>
      <td>131.330002</td>
      <td>50.559578</td>
      <td>43.474976</td>
      <td>165.678986</td>
      <td>42.204353</td>
      <td>60.162991</td>
      <td>29.573030</td>
      <td>219.039993</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>93.191338</td>
      <td>129.479996</td>
      <td>49.418652</td>
      <td>42.238308</td>
      <td>160.586899</td>
      <td>41.317917</td>
      <td>58.558380</td>
      <td>29.376560</td>
      <td>215.649994</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>93.684120</td>
      <td>128.330002</td>
      <td>48.743401</td>
      <td>41.500145</td>
      <td>159.923523</td>
      <td>41.574001</td>
      <td>57.599533</td>
      <td>29.002338</td>
      <td>211.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate the daily log return for `daily_price`
daily_return = np.log(daily_price.pct_change()+1)

# Replace NA values with 0
daily_return.fillna(0, inplace=True)

# View the first 5 rows
daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AAPL</th>
      <th>BRK-B</th>
      <th>BTI</th>
      <th>COP</th>
      <th>GS</th>
      <th>LUV</th>
      <th>NKE</th>
      <th>PFE</th>
      <th>TSLA</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
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
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>-0.025379</td>
      <td>0.003817</td>
      <td>0.002116</td>
      <td>0.011031</td>
      <td>-0.017368</td>
      <td>0.011846</td>
      <td>0.013882</td>
      <td>0.007173</td>
      <td>0.000089</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>-0.019764</td>
      <td>0.000609</td>
      <td>-0.002024</td>
      <td>-0.044212</td>
      <td>-0.024716</td>
      <td>0.009143</td>
      <td>-0.014370</td>
      <td>-0.017872</td>
      <td>-0.019844</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>-0.043121</td>
      <td>-0.014187</td>
      <td>-0.022824</td>
      <td>-0.028858</td>
      <td>-0.031217</td>
      <td>-0.021227</td>
      <td>-0.027033</td>
      <td>-0.006666</td>
      <td>-0.015598</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>0.005274</td>
      <td>-0.008921</td>
      <td>-0.013758</td>
      <td>-0.017631</td>
      <td>-0.004140</td>
      <td>0.006179</td>
      <td>-0.016510</td>
      <td>-0.012821</td>
      <td>-0.021799</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plot the frequency distributions
daily_return.hist(bins=40, sharex=True, figsize=(12,8), facecolor='r', alpha=0.75)
plt.show()
```


![png](/images/2017-08-27/output_5_0.png)



```python
# calculate the correlation matrix
corr = daily_return.corr()

# plot the heatmap
fig, ax = plt.subplots(figsize=(10,10))
sns.heatmap(corr, 
            xticklabels=corr.columns, 
            yticklabels=corr.columns, 
            annot=True, 
            linewidths=.5)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11f3fda90>




![png](/images/2017-08-27/output_6_1.png)



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
      <th>BRK-B</th>
      <th>BTI</th>
      <th>COP</th>
      <th>GS</th>
      <th>LUV</th>
      <th>NKE</th>
      <th>PFE</th>
      <th>TSLA</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
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
      <td>1.250541</td>
      <td>1.049368</td>
      <td>1.025744</td>
      <td>1.332540</td>
      <td>1.155339</td>
      <td>0.832765</td>
      <td>1.037307</td>
      <td>0.887124</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>1.107203</td>
      <td>1.247739</td>
      <td>1.053289</td>
      <td>1.030327</td>
      <td>1.335799</td>
      <td>1.156255</td>
      <td>0.822759</td>
      <td>1.038903</td>
      <td>0.912497</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>1.102471</td>
      <td>1.238628</td>
      <td>1.041533</td>
      <td>1.013619</td>
      <td>1.330757</td>
      <td>1.148903</td>
      <td>0.818416</td>
      <td>1.033138</td>
      <td>0.913370</td>
    </tr>
    <tr>
      <th>2016-12-29</th>
      <td>1.102188</td>
      <td>1.233322</td>
      <td>1.050843</td>
      <td>1.009016</td>
      <td>1.317028</td>
      <td>1.149590</td>
      <td>0.819058</td>
      <td>1.037599</td>
      <td>0.892091</td>
    </tr>
    <tr>
      <th>2016-12-30</th>
      <td>1.093562</td>
      <td>1.232792</td>
      <td>1.053645</td>
      <td>1.000999</td>
      <td>1.324031</td>
      <td>1.141316</td>
      <td>0.815360</td>
      <td>1.037280</td>
      <td>0.887968</td>
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


    <matplotlib.figure.Figure at 0x11f712cc0>



![png](/images/2017-08-27/output_8_1.png)



