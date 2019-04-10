---
title: FRM PPT
subtitle: quantitive
author: Tony Z. Yang
tags: [EXAM]
date-string: Feberary 17, 2019
---

<center>
   <embed src="/images/frmL1-quantitive.pdf" width="800" height="600">
</embed>
</br>
<a href="/images/frmL1-quantitive.pdf">frmL1-quantitive</a><br>
<a href="/images/1-market-risk.pdf">frmL2-market-risk</a><br>
<a href="/images/2-portfolio.pdf">frmL2-portfolio</a>
<blockquote>
  <p>
import pandas as pd <br/>
import numpy as np <br/>
nyc=pd.read_csv('/Users/zeshiyang/Documents/rutgers/bap/ny.csv',index_col=False)<br/>
nyc.shape</p>
</blockquote>



# 
![](http://latex.codecogs.com/gif.latex?\\operatorname{VaR}=\\mathrm{u}+\\left(\\frac{\\beta}{\\xi}\\right)\\left\\{\\left[\\frac{\\mathrm{N}}{\\mathrm{n}}(1-Confidence_Level)\\right]^{-\\xi}-1\\right\\})
u=loss threshold<br>
beta=scale<br>
n=number of obs exceed threshold<br>
N=number of obs<br>
![](http://latex.codecogs.com/gif.latex?\\xi=\\text(shape(tailindex)))
![](http://latex.codecogs.com/gif.latex?ES=\frac{VaR}{1-\xi}+\frac{\beta-\xi\cdot\mu}{1-\xi})


