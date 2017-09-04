---
layout: post
title: Backtesting a Portfolio with Modern Portfolio Theory
subtitle: The Efficient Frontier of Markowitz portfolio in Python
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
import statsmodels.formula.api as smf
import matplotlib.pyplot as plt 
from matplotlib import style
%matplotlib inline

style.use('fivethirtyeight')
```


```python
start = dt.datetime(2017, 1, 1)
end = dt.datetime(2017, 6, 1)

# list of stocks in portfolio
tickers = ['AAPL', 'FB', 'MSFT', 'NKE']

# Number of assets in portfolio
noa = len(tickers)

# array of weights in portfolio
weight = np.array([[0.25, 0.25, 0.25, 0.25]])
```


```python
def get_return(tickers, start, end, log_return=True):
    '''Download the price from yahoo finance
       Calculate the log return and store them in to a dataframe
       replace NaN with 0, if any'''    
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
daily_return = get_return(tickers, start, end)
daily_return.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AAPL</th>
      <th>FB</th>
      <th>MSFT</th>
      <th>NKE</th>
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
      <th>2017-01-03</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2017-01-04</th>
      <td>-0.001120</td>
      <td>0.015538</td>
      <td>-0.004484</td>
      <td>0.020753</td>
    </tr>
    <tr>
      <th>2017-01-05</th>
      <td>0.005072</td>
      <td>0.016544</td>
      <td>0.000000</td>
      <td>-0.000188</td>
    </tr>
    <tr>
      <th>2017-01-06</th>
      <td>0.011087</td>
      <td>0.022453</td>
      <td>0.008630</td>
      <td>0.015893</td>
    </tr>
    <tr>
      <th>2017-01-09</th>
      <td>0.009118</td>
      <td>0.012001</td>
      <td>-0.003188</td>
      <td>-0.009880</td>
    </tr>
  </tbody>
</table>
</div>




```python
def rand_weights(noa):
    ''' Produces some random weights that sum to 1 '''
    w = np.random.random(noa) 
    w /= np.sum(w)
    return w

rand_weights(noa)
```




    array([ 0.24943071,  0.27557857,  0.14757955,  0.32741117])




```python
def random_portfolio(daily_return):
    ''' 
    Returns the mean and standard deviation of returns for a random portfolio
    '''
    P = np.asmatrix(daily_return.mean() * 252)
    W = np.asmatrix(rand_weights(noa))
    C = np.asmatrix(daily_return.cov() * 252)
    
    mu = W * P.T
    sigma = np.sqrt(W * C * W.T)
    
    return mu, sigma
```


```python
n_portfolios = 10000

means, stds = np.column_stack([
random_portfolio(daily_return) 
    for _ in range(n_portfolios)
])
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
      <th>FB</th>
      <th>MSFT</th>
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
      <th>2017-01-03</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2017-01-04</th>
      <td>-0.001120</td>
      <td>0.015538</td>
      <td>-0.004484</td>
      <td>0.020753</td>
      <td>0.007672</td>
      <td>0.005932</td>
    </tr>
    <tr>
      <th>2017-01-05</th>
      <td>0.005072</td>
      <td>0.016544</td>
      <td>0.000000</td>
      <td>-0.000188</td>
      <td>0.005357</td>
      <td>-0.000795</td>
    </tr>
    <tr>
      <th>2017-01-06</th>
      <td>0.011087</td>
      <td>0.022453</td>
      <td>0.008630</td>
      <td>0.015893</td>
      <td>0.014516</td>
      <td>0.003571</td>
    </tr>
    <tr>
      <th>2017-01-09</th>
      <td>0.009118</td>
      <td>0.012001</td>
      <td>-0.003188</td>
      <td>-0.009880</td>
      <td>0.002013</td>
      <td>-0.003306</td>
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
      <th>FB</th>
      <th>MSFT</th>
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
      <th>2017-05-26</th>
      <td>1.327102</td>
      <td>1.296646</td>
      <td>1.127979</td>
      <td>1.007640</td>
      <td>1.185813</td>
      <td>1.076692</td>
    </tr>
    <tr>
      <th>2017-05-30</th>
      <td>1.327620</td>
      <td>1.298775</td>
      <td>1.135212</td>
      <td>1.013943</td>
      <td>1.190171</td>
      <td>1.075757</td>
    </tr>
    <tr>
      <th>2017-05-31</th>
      <td>1.319735</td>
      <td>1.290910</td>
      <td>1.125984</td>
      <td>1.015283</td>
      <td>1.184576</td>
      <td>1.075489</td>
    </tr>
    <tr>
      <th>2017-06-01</th>
      <td>1.323359</td>
      <td>1.291506</td>
      <td>1.130168</td>
      <td>1.005430</td>
      <td>1.183753</td>
      <td>1.084008</td>
    </tr>
    <tr>
      <th>2017-06-02</th>
      <td>1.342826</td>
      <td>1.309114</td>
      <td>1.156619</td>
      <td>1.018419</td>
      <td>1.202890</td>
      <td>1.087610</td>
    </tr>
  </tbody>
</table>
</div>




```python
ndays = len(daily_return)
actual_return = (1 + daily_return).prod() - 1
annualized_return = (1 + actual_return) ** ( 252 / ndays ) - 1
annualized_stdev = daily_return.std() * np.sqrt(252)

results = smf.ols('Portfolio ~ SPY', data=daily_return).fit()

ra = annualized_return['Portfolio']
rm = annualized_return['SPY']
rf = 0.00 # you can change this number

beta = results.params[1]
alpha = ra - beta*(rm - rf)

sharpe_ratio = (ra-rf)/annualized_stdev['Portfolio']
```


```python
print('Portfolio_annualized_return: ' + str(annualized_return['Portfolio']))
print('Portfolio_annualized_stdev: ' + str(annualized_stdev['Portfolio']))
print('Benchmark_annualized_return: ' + str(annualized_return['SPY']))
print('Benchmark_annualized_stdev: ' + str(annualized_stdev['SPY']))
print('Portfolio Beta: ' + str(beta))
print('Alpha: ' + str(alpha))
print('Sharpe_ratio: ' + str(sharpe_ratio))
```

    Portfolio_annualized_return: 0.557910023127
    Portfolio_annualized_stdev: 0.0982440376708
    Benchmark_annualized_return: 0.223307484231
    Benchmark_annualized_stdev: 0.0703911215652
    Portfolio Beta: 0.880497354834
    Alpha: 0.361288373947
    Sharpe_ratio: 5.67881813853



```python
plt.figure(figsize=(12, 8)) 
plt.scatter(stds, means, c=means / stds, marker='o') 
plt.grid(True) 
plt.xlabel('Expected Volatility') 
plt.ylabel('Expected Return') 
plt.title('Return and Standard Deviation of Randomly Generated Portfolios')
plt.colorbar(label='Sharpe ratio')
# this red star is our portfolio
plt.scatter(annualized_stdev['Portfolio'],annualized_return['Portfolio'],marker=(5,1,0),color='r',s=1000)
# this green star is the benchmark, SPY
plt.scatter(annualized_stdev['SPY'],annualized_return['SPY'],marker=(5,1,0),color='g',s=1000)
```




    <matplotlib.collections.PathCollection at 0x12408bb70>




![png](/images/2017-09-01/output_11_1.png)



```python
cum_daily_return[['Portfolio','SPY']].plot(grid = True, figsize=(14,10)).axhline(y = 1, color = "black", lw = 1)
plt.ylabel("Cumulative Returns")
plt.legend()
plt.show()
```


![png](/images/2017-09-01/output_12_0.png)



```python
cum_daily_return.plot(grid = True, figsize=(14,10)).axhline(y = 1, color = "black", lw = 1)
plt.ylabel("Cumulative Returns")
plt.legend()
plt.show()
```


![png](/images/2017-09-01/output_13_0.png)



```python

```



![png](/images/2017-09-01/output_7_2.png)



