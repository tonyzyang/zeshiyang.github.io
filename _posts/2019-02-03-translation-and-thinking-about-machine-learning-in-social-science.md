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


      
***Summary***<br>
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
In the case of bail, human predictions can be biased. <br>
An appropriate first benchmark for evaluating the effect of using algorithms is the the predictions and decisions already being made by humans.

Algorithms have a form of neutrality that the human mind struggles to obtain(neutrality), at least within their narrow area of focus. It is entirely possible, for algorithms to serve as a force for equity. We ought to pair our caution with hope. If the ultimate outcome is hard to measure, or involves a hard-to-define combination of outcomes, then it is not a good fit for machine learning. 

Like bail, sentencing of people who have been found guilty depends not only on recidivism risk, but also on things like society’s sense of retribution, mercy, and redemption, which cannot be directly measured.


**Verify your algorithm in an experiment on data it hasn’t seen**<br>

For machine learning to be useful for policy, it must accurately predict “out-ofsample.” <br>
That means it should be trained on one set of data, then tested on a dataset it hasn’t seen before.<br>
For many applications, are inherently flawed. Current practice is to report how well one’s algorithm predicts only among those cases where we can observe the outcome.<br>
This makes it hard to evaluate whether any new machine learning tool can actually improve outcomes relative to the existing decision-making system.<br>
an algorithm predicts well on the part of the test data where we can observe labels doesn’t mean it will make good predictions in the real world. <br>
To do a randomized controlled trial of the sort. Then we could directly compare whether bail decisions made using machine learning lead to better outcomes than those made on comparable cases using the current system of judicial decision-making.
finding a “natural experiment” to evaluate the tool.---random select cases<br>
Smart users refuse to use any prediction tool that does not take this evaluation challenge more seriously.<br>

**Remember there’s still a lot we don’t know**<br>

it is hard to imagine moving to a world in which the algorithms actually make the decisions;<br>
For algorithms to add value, we need people to actually use them; <br>

policymakers need to know when they should override the algorithm.
For people to know when to override, they need to understand their comparative advantage over the algorithm <br>

But often it’s only the human who can see the extenuating circumstance in a given case, since it may be based on factors not captured in the data on which the algorithm was trained.<br>
 
 **Pair caution with hope**<br>
Encourage the spread of machine learning to help solve the most challenging social problems in order to improve
the lives of many. But also be reminded to be mindful, and to wear our seatbelts.




<p style="color:#000000;">这是黑色的字</p>
