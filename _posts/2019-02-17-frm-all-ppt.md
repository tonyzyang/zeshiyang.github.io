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



# var and ES for peaks over threholds approach
![](http://latex.codecogs.com/gif.latex?\\operatorname{VaR}=\\mathrm{u}+\\left(\\frac{\\beta}{\\xi}\\right)\\left\\{\\left[\\frac{\\mathrm{N}}{\\mathrm{n}}(1-Confidence_Level)\\right]^{-\\xi}-1\\right\\})<br>
u=loss threshold<br>
beta=scale<br>
n=number of obs exceed threshold<br>
N=number of obs<br>
![](http://latex.codecogs.com/gif.latex?\\xi=\\text(shape(tailindex)))<br>
![](http://latex.codecogs.com/gif.latex?ES=\frac{VaR}{1-\xi}+\frac{\beta-\xi\cdot\mu}{1-\xi})

##general var
### lognormal:
![](http://latex.codecogs.com/gif.latex?%5Clog%20N%5Ccdot%20V%5Cleft%281-e%5E%7B%5Cmu-z_%7B0%7D%5Ccdot%5Csigma%7D%5Cright%29)<br>
### normal
![](http://latex.codecogs.com/gif.latex?V%20%5Ccdot%20z_%7B%5Calpha%7D%20%5Csigma)<br>

# liquidity risk VaR
## constant spread approach
### lognormal
![](http://latex.codecogs.com/gif.latex?LVaR%3D%5Cleft%5B1-%5Cexp%20%5Cleft%28%5Cmu-%5Csigma%20%5Ctimes%20%5Cmathrm%7Bz%7D_%7B%5Calpha%7D%5Cright%29%5Cright%5D%20%5Ctimes%20%5Cmathrm%7BV%7D&plus;0.5%20%5Ctimes%20%5Ctext%20%7B%20spread%20%7D%20%5Ctimes%20%5Cmathrm%7BV%7D)

### normal
![](http://latex.codecogs.com/gif.latex?%5Cmathrm%7BLVaR%7D%3D%5Cleft%28%5Cmathrm%7BV%7D%20%5Ctimes%20%5Cmathrm%7Bz%7D_%7B%5Calpha%7D%20%5Ctimes%20%5Csigma%5Cright%29&plus;%5B0.5%20%5Ctimes%20%5Cmathrm%7BV%7D%20%5Ctimes%20%5Ctext%20%7B%20spread%20%7D%5D)

![](http://latex.codecogs.com/gif.latex?%5Ctext%20%7B%20spread%20%7D%3D%5Cfrac%7B%28%5Ctext%20%7B%20ask%20price%20%7D-%5Ctext%20%7B%20bid%20price%20%7D%29%7D%7B%28%5Ctext%20%7B%20ask%20price%20%7D&plus;%5Ctext%20%7B%20bid%20price%20%7D%29%20/%202%7D)

![](http://latex.codecogs.com/gif.latex?L%20C%3D0.5%20%5Ctimes%20V%20%5Ctimes%20%5Ctext%20%7B%20spread%20%7D)

## random spread approach
### log noraml
![](http://latex.codecogs.com/gif.latex?%5Cmathrm%7BLVaR%7D%3D%5Cmathrm%7BV%7D%20%5Ctimes%5Cleft%5C%7B%5Cleft%5B1-%5Cexp%20%5Cleft%28%5Cmu-%5Csigma%20%5Ctimes%20%5Cmathrm%7Bz%7D_%7B%5Calpha%7D%5Cright%29%5Cright%5D&plus;%5Cleft%5B0.5%20%5Ctimes%5Cleft%28%5Cmu_%7B%5Cmathrm%7BS%7D%7D&plus;%5Cmathrm%7Bz%7D_%7B%5Calpha%7D%5E%7B%5Cprime%7D%20%5Ctimes%20%5Csigma_%7B%5Cmathrm%7BS%7D%7D%5Cright%29%5Cright%5D%5Cright%5C%7D
)<br>
### noraml
![](http://latex.codecogs.com/gif.latex?%5Cmathrm%7BLVaR%7D%3D%5Cmathrm%7BVaR%7D&plus;0.5%20%5Ctimes%5Cleft%5B%5Cleft%28%5Cmu_%7B%5Cmathrm%7BS%7D%7D&plus;%5Cmathrm%7Bz%7D_%7B%5Calpha%7D%5E%7B%5Cprime%7D%20%5Ctimes%20%5Csigma_%7B%5Cmathrm%7Bs%7D%7D%5Cright%29%5Cright%5D%20%5Ctimes%20%5Cmathrm%7BV%7D)<br>






