---
layout: post
title: Getting Stock Price Data from Yahoo Finance
subtitle: Some basic stock data processing and visualizations in python
author: Caesar F. Yang
featured-image: /images/2017-08-24/header_stock.jpg
tags: [data visualization, python, finance]
date-string: August 24, 2017
---


```python
import datetime as dt
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import style
import pandas as pd
import pandas_datareader.data as web
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
#Select the ticker and source of stock data
df = web.DataReader('AAPL', "yahoo", start, end)
```


```python
#Adj Close is helpful, since it accounts for future stock splits, and gives the relative price to splits
#For this reason, the adjusted prices are the prices you're most likely to be dealing with.
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Adj Close</th>
      <th>Volume</th>
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
      <td>102.610001</td>
      <td>105.370003</td>
      <td>102.000000</td>
      <td>105.349998</td>
      <td>101.790649</td>
      <td>67649400</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>105.750000</td>
      <td>105.849998</td>
      <td>102.410004</td>
      <td>102.709999</td>
      <td>99.239845</td>
      <td>55791000</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>100.559998</td>
      <td>102.370003</td>
      <td>99.870003</td>
      <td>100.699997</td>
      <td>97.297760</td>
      <td>68457400</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>98.680000</td>
      <td>100.129997</td>
      <td>96.430000</td>
      <td>96.449997</td>
      <td>93.191338</td>
      <td>81094400</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>98.550003</td>
      <td>99.110001</td>
      <td>96.760002</td>
      <td>96.959999</td>
      <td>93.684120</td>
      <td>70798000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#We can save them easily to a csv in our wd
df.to_csv('AAPL.csv')
```

![png](/images/2017-08-24/wd.png)

```python
#We can either read data from DataFrame or from a CSV file into a DataFrame:
df = pd.read_csv('AAPL.csv', parse_dates=True, index_col=0)
```


```python
##Now, we can graph it
df.plot()
plt.show()
```


![png](/images/2017-08-24/output_7_0.png)



```python
#Now, we can graph one specific column in the DataFrame
df['Adj Close'].plot(grid=True)
plt.show()
```


![png](/images/2017-08-24/output_8_0.png)



```python
#Call one or more columns in the DataFrame
df[['High','Low']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>High</th>
      <th>Low</th>
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
      <td>105.370003</td>
      <td>102.000000</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>105.849998</td>
      <td>102.410004</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>102.370003</td>
      <td>99.870003</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>100.129997</td>
      <td>96.430000</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>99.110001</td>
      <td>96.760002</td>
    </tr>
    <tr>
      <th>2016-01-11</th>
      <td>99.059998</td>
      <td>97.339996</td>
    </tr>
    <tr>
      <th>2016-01-12</th>
      <td>100.690002</td>
      <td>98.839996</td>
    </tr>
    <tr>
      <th>2016-01-13</th>
      <td>101.190002</td>
      <td>97.300003</td>
    </tr>
    <tr>
      <th>2016-01-14</th>
      <td>100.480003</td>
      <td>95.739998</td>
    </tr>
    <tr>
      <th>2016-01-15</th>
      <td>97.709999</td>
      <td>95.360001</td>
    </tr>
    <tr>
      <th>2016-01-19</th>
      <td>98.650002</td>
      <td>95.500000</td>
    </tr>
    <tr>
      <th>2016-01-20</th>
      <td>98.190002</td>
      <td>93.419998</td>
    </tr>
    <tr>
      <th>2016-01-21</th>
      <td>97.879997</td>
      <td>94.940002</td>
    </tr>
    <tr>
      <th>2016-01-22</th>
      <td>101.459999</td>
      <td>98.370003</td>
    </tr>
    <tr>
      <th>2016-01-25</th>
      <td>101.529999</td>
      <td>99.209999</td>
    </tr>
    <tr>
      <th>2016-01-26</th>
      <td>100.879997</td>
      <td>98.070000</td>
    </tr>
    <tr>
      <th>2016-01-27</th>
      <td>96.629997</td>
      <td>93.339996</td>
    </tr>
    <tr>
      <th>2016-01-28</th>
      <td>94.519997</td>
      <td>92.389999</td>
    </tr>
    <tr>
      <th>2016-01-29</th>
      <td>97.339996</td>
      <td>94.349998</td>
    </tr>
    <tr>
      <th>2016-02-01</th>
      <td>96.709999</td>
      <td>95.400002</td>
    </tr>
    <tr>
      <th>2016-02-02</th>
      <td>96.040001</td>
      <td>94.279999</td>
    </tr>
    <tr>
      <th>2016-02-03</th>
      <td>96.839996</td>
      <td>94.080002</td>
    </tr>
    <tr>
      <th>2016-02-04</th>
      <td>97.330002</td>
      <td>95.190002</td>
    </tr>
    <tr>
      <th>2016-02-05</th>
      <td>96.919998</td>
      <td>93.690002</td>
    </tr>
    <tr>
      <th>2016-02-08</th>
      <td>95.699997</td>
      <td>93.040001</td>
    </tr>
    <tr>
      <th>2016-02-09</th>
      <td>95.940002</td>
      <td>93.930000</td>
    </tr>
    <tr>
      <th>2016-02-10</th>
      <td>96.349998</td>
      <td>94.099998</td>
    </tr>
    <tr>
      <th>2016-02-11</th>
      <td>94.720001</td>
      <td>92.589996</td>
    </tr>
    <tr>
      <th>2016-02-12</th>
      <td>94.500000</td>
      <td>93.010002</td>
    </tr>
    <tr>
      <th>2016-02-16</th>
      <td>96.849998</td>
      <td>94.610001</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2016-11-17</th>
      <td>110.349998</td>
      <td>108.830002</td>
    </tr>
    <tr>
      <th>2016-11-18</th>
      <td>110.540001</td>
      <td>109.660004</td>
    </tr>
    <tr>
      <th>2016-11-21</th>
      <td>111.989998</td>
      <td>110.010002</td>
    </tr>
    <tr>
      <th>2016-11-22</th>
      <td>112.419998</td>
      <td>111.400002</td>
    </tr>
    <tr>
      <th>2016-11-23</th>
      <td>111.510002</td>
      <td>110.330002</td>
    </tr>
    <tr>
      <th>2016-11-25</th>
      <td>111.870003</td>
      <td>110.949997</td>
    </tr>
    <tr>
      <th>2016-11-28</th>
      <td>112.470001</td>
      <td>111.389999</td>
    </tr>
    <tr>
      <th>2016-11-29</th>
      <td>112.029999</td>
      <td>110.070000</td>
    </tr>
    <tr>
      <th>2016-11-30</th>
      <td>112.199997</td>
      <td>110.269997</td>
    </tr>
    <tr>
      <th>2016-12-01</th>
      <td>110.940002</td>
      <td>109.029999</td>
    </tr>
    <tr>
      <th>2016-12-02</th>
      <td>110.089996</td>
      <td>108.849998</td>
    </tr>
    <tr>
      <th>2016-12-05</th>
      <td>110.029999</td>
      <td>108.250000</td>
    </tr>
    <tr>
      <th>2016-12-06</th>
      <td>110.360001</td>
      <td>109.190002</td>
    </tr>
    <tr>
      <th>2016-12-07</th>
      <td>111.190002</td>
      <td>109.160004</td>
    </tr>
    <tr>
      <th>2016-12-08</th>
      <td>112.430000</td>
      <td>110.599998</td>
    </tr>
    <tr>
      <th>2016-12-09</th>
      <td>114.699997</td>
      <td>112.309998</td>
    </tr>
    <tr>
      <th>2016-12-12</th>
      <td>115.000000</td>
      <td>112.489998</td>
    </tr>
    <tr>
      <th>2016-12-13</th>
      <td>115.919998</td>
      <td>113.750000</td>
    </tr>
    <tr>
      <th>2016-12-14</th>
      <td>116.199997</td>
      <td>114.980003</td>
    </tr>
    <tr>
      <th>2016-12-15</th>
      <td>116.730003</td>
      <td>115.230003</td>
    </tr>
    <tr>
      <th>2016-12-16</th>
      <td>116.500000</td>
      <td>115.650002</td>
    </tr>
    <tr>
      <th>2016-12-19</th>
      <td>117.379997</td>
      <td>115.750000</td>
    </tr>
    <tr>
      <th>2016-12-20</th>
      <td>117.500000</td>
      <td>116.680000</td>
    </tr>
    <tr>
      <th>2016-12-21</th>
      <td>117.400002</td>
      <td>116.779999</td>
    </tr>
    <tr>
      <th>2016-12-22</th>
      <td>116.510002</td>
      <td>115.639999</td>
    </tr>
    <tr>
      <th>2016-12-23</th>
      <td>116.519997</td>
      <td>115.589996</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>117.800003</td>
      <td>116.489998</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>118.019997</td>
      <td>116.199997</td>
    </tr>
    <tr>
      <th>2016-12-29</th>
      <td>117.110001</td>
      <td>116.400002</td>
    </tr>
    <tr>
      <th>2016-12-30</th>
      <td>117.199997</td>
      <td>115.430000</td>
    </tr>
  </tbody>
</table>
<p>252 rows × 2 columns</p>
</div>




```python
# Assign `Adj Close` to `daily_close`
daily_close = df[['Adj Close']]

# Daily returns
daily_pct_change = daily_close.pct_change()

# Replace NA values with 0
daily_pct_change.fillna(0, inplace=True)

# Inspect daily returns
print(daily_pct_change)
```

                Adj Close
    Date                 
    2016-01-04   0.000000
    2016-01-05  -0.025059
    2016-01-06  -0.019570
    2016-01-07  -0.042205
    2016-01-08   0.005288
    2016-01-11   0.016192
    2016-01-12   0.014513
    2016-01-13  -0.025710
    2016-01-14   0.021871
    2016-01-15  -0.024015
    2016-01-19  -0.004839
    2016-01-20   0.001345
    2016-01-21  -0.005063
    2016-01-22   0.053167
    2016-01-25  -0.019523
    2016-01-26   0.005531
    2016-01-27  -0.065706
    2016-01-28   0.007172
    2016-01-29   0.034541
    2016-02-01  -0.009349
    2016-02-02  -0.020222
    2016-02-03   0.019793
    2016-02-04   0.008035
    2016-02-05  -0.026708
    2016-02-08   0.010530
    2016-02-09  -0.000211
    2016-02-10  -0.007580
    2016-02-11  -0.006046
    2016-02-12   0.003095
    2016-02-16   0.028194
    ...               ...
    2016-11-17  -0.000364
    2016-11-18   0.001000
    2016-11-21   0.015174
    2016-11-22   0.000626
    2016-11-23  -0.005098
    2016-11-25   0.005035
    2016-11-28  -0.001968
    2016-11-29  -0.000986
    2016-11-30  -0.008434
    2016-12-01  -0.009320
    2016-12-02   0.003745
    2016-12-05  -0.007188
    2016-12-06   0.007699
    2016-12-07   0.009823
    2016-12-08   0.009817
    2016-12-09   0.016322
    2016-12-12  -0.005704
    2016-12-13   0.016681
    2016-12-14   0.000000
    2016-12-15   0.005469
    2016-12-16   0.001295
    2016-12-19   0.005777
    2016-12-20   0.002658
    2016-12-21   0.000941
    2016-12-22  -0.006578
    2016-12-23   0.001978
    2016-12-27   0.006351
    2016-12-28  -0.004264
    2016-12-29  -0.000257
    2016-12-30  -0.007796
    
    [252 rows x 1 columns]



```python
# Daily log returns
daily_log_returns = np.log(daily_close.pct_change()+1)

# Replace NA values with 0
daily_log_returns.fillna(0, inplace=True)

# Print daily log returns
print(daily_log_returns)
```

                Adj Close
    Date                 
    2016-01-04   0.000000
    2016-01-05  -0.025379
    2016-01-06  -0.019764
    2016-01-07  -0.043121
    2016-01-08   0.005274
    2016-01-11   0.016063
    2016-01-12   0.014409
    2016-01-13  -0.026046
    2016-01-14   0.021635
    2016-01-15  -0.024308
    2016-01-19  -0.004851
    2016-01-20   0.001344
    2016-01-21  -0.005075
    2016-01-22   0.051802
    2016-01-25  -0.019716
    2016-01-26   0.005516
    2016-01-27  -0.067965
    2016-01-28   0.007146
    2016-01-29   0.033958
    2016-02-01  -0.009393
    2016-02-02  -0.020429
    2016-02-03   0.019599
    2016-02-04   0.008003
    2016-02-05  -0.027071
    2016-02-08   0.010475
    2016-02-09  -0.000211
    2016-02-10  -0.007609
    2016-02-11  -0.006065
    2016-02-12   0.003090
    2016-02-16   0.027804
    ...               ...
    2016-11-17  -0.000364
    2016-11-18   0.001000
    2016-11-21   0.015060
    2016-11-22   0.000626
    2016-11-23  -0.005111
    2016-11-25   0.005022
    2016-11-28  -0.001970
    2016-11-29  -0.000986
    2016-11-30  -0.008469
    2016-12-01  -0.009363
    2016-12-02   0.003738
    2016-12-05  -0.007214
    2016-12-06   0.007669
    2016-12-07   0.009775
    2016-12-08   0.009769
    2016-12-09   0.016190
    2016-12-12  -0.005721
    2016-12-13   0.016544
    2016-12-14   0.000000
    2016-12-15   0.005454
    2016-12-16   0.001294
    2016-12-19   0.005761
    2016-12-20   0.002654
    2016-12-21   0.000940
    2016-12-22  -0.006600
    2016-12-23   0.001976
    2016-12-27   0.006331
    2016-12-28  -0.004273
    2016-12-29  -0.000257
    2016-12-30  -0.007826
    
    [252 rows x 1 columns]



```python
# Plot the distribution of `daily_pct_change`
daily_pct_change.hist(bins=50)

# Show the plot
plt.show()

# Pull up summary statistics
print(daily_pct_change.describe())
```


![png](/images/2017-08-24/output_12_0.png)


            Adj Close
    count  252.000000
    mean     0.000571
    std      0.014702
    min     -0.065706
    25%     -0.005742
    50%      0.000823
    75%      0.007724
    max      0.064963



```python
# Calculate the cumulative daily returns
cum_daily_return = (1 + daily_pct_change).cumprod()

# Print `cum_daily_return`
print(cum_daily_return)
```

                Adj Close
    Date                 
    2016-01-04   1.000000
    2016-01-05   0.974941
    2016-01-06   0.955861
    2016-01-07   0.915520
    2016-01-08   0.920361
    2016-01-11   0.935263
    2016-01-12   0.948837
    2016-01-13   0.924442
    2016-01-14   0.944661
    2016-01-15   0.921974
    2016-01-19   0.917513
    2016-01-20   0.918747
    2016-01-21   0.914096
    2016-01-22   0.962696
    2016-01-25   0.943901
    2016-01-26   0.949122
    2016-01-27   0.886759
    2016-01-28   0.893118
    2016-01-29   0.923968
    2016-02-01   0.915330
    2016-02-02   0.896820
    2016-02-03   0.914571
    2016-02-04   0.921919
    2016-02-05   0.897296
    2016-02-08   0.906745
    2016-02-09   0.906554
    2016-02-10   0.899682
    2016-02-11   0.894243
    2016-02-12   0.897010
    2016-02-16   0.922301
    ...               ...
    2016-11-17   1.066885
    2016-11-18   1.067952
    2016-11-21   1.084157
    2016-11-22   1.084836
    2016-11-23   1.079305
    2016-11-25   1.084739
    2016-11-28   1.082604
    2016-11-29   1.081537
    2016-11-30   1.072416
    2016-12-01   1.062421
    2016-12-02   1.066400
    2016-12-05   1.058734
    2016-12-06   1.066885
    2016-12-07   1.077364
    2016-12-08   1.087941
    2016-12-09   1.105698
    2016-12-12   1.099391
    2016-12-13   1.117730
    2016-12-14   1.117730
    2016-12-15   1.123843
    2016-12-16   1.125299
    2016-12-19   1.131800
    2016-12-20   1.134808
    2016-12-21   1.135876
    2016-12-22   1.128404
    2016-12-23   1.130636
    2016-12-27   1.137816
    2016-12-28   1.132965
    2016-12-29   1.132673
    2016-12-30   1.123843
    
    [252 rows x 1 columns]



```python
# Plot the cumulative daily returns
cum_daily_return.plot(figsize=(12,8))

# Show the plot
plt.show()
```


![png](/images/2017-08-24/output_14_0.png)

