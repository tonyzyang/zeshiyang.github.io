---
title:  translation and thinking about machine learning in social science
author: Tony Z. Yang	
tags: [reading and thinking]
date-string: Feberary 3, 2019
---

<iframe src="/images/A Guide to Solving Social Problems with Machine Learning.pdf" width="800" height="600"></iframe> 

<object width="800" height="600" data="/images/Social Problems with Machine Learning.pdf" type="application/pdf">
      <param name="src" value="/images/Social Problems with Machine Learning.pdf">
</object>


**How machine learning can improve public policy**<br>
Applying machine learning to a dataset of over one million bond court cases<br>
Using our algorithm’s predictions of risk instead of relying on judge intuition to reduce crimes committed by released defendants by up to 25% without having to jail any additional people. This can reduce jail populations by up to several hundred thousand people. <br>
Applying machine learning to policy problems<br>

**Look for policy problems that hinge on prediction**<br>
Many social-sector decisions do not hinge on a prediction.<br>some questions that hinge on understanding the causal
effect of something on the world.It cannot be addressed by machine learning prediction methods, instead need tools for causation, like randomized experiments.


**Make sure you’re comfortable with the outcome you’re predicting**<br>
we should set algorithm to treat every crime as equal.<br>
In bail, different forms of crime are correlated enough, so that an algorithm trained on just one type of crime winds up outpredicting judges on almost every measure of criminality we could construct, including violent crime. The outcome you select for your algorithm will define it. So you need to think carefully about what that outcome is and what else it might be leaving out.

**Check for bias**<br>
In the case of bail, we know from decades of research that those human predictions can be biased. <br>
An appropriate first benchmark for evaluating the effect of using algorithms is the the predictions and decisions already being made by humans.

Algorithms have a form of neutrality that the human mind struggles to obtain(neutrality), at least within their narrow area of focus. It is entirely possible, for algorithms to serve as a force for equity. We ought to pair our caution with hope. If the ultimate outcome is hard to measure, or involves a hard-to-define combination of outcomes, then it is not a good fit for machine learning. 

Like bail, sentencing of people who have been found guilty depends not only on recidivism risk, but also on things like society’s sense of retribution, mercy, and redemption, which cannot be directly measured.

