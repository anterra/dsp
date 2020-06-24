[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

## Exercise 2.4

"Using the variable totalwgt_lb, investigate whether first babies are lighter or heavier than others. Compute Cohen's d to quantify the difference between the groups. How does it compare to the difference in pregnancy length?"

My approach to this problem, in order to gain more practice, was to start from scratch with selecting the data I wanted to work with, and then to calculate Cohen's *d* myself based on its mathematical definition, rather than by using the author's pre-defined python method. 

I first imported the relevant packages: 
```
import numpy as np
import nsfg
```
Then, I used the author's method "ReadFemPreg" to read in the nsfg pregnancy data, which also cleans it, removes invalid entries, and defines the recode variable "totalwgt_lb" from the raw data's separate birthwgt_lb and birthwgt_oz variables. I then filtered the data to only look at pregnancies which ended in live births (outcome of value "1" indicates live birth):
```
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]
```
Next, I divided the live births DataFrame into subsets based on whether the pregnancy was a first or not. For each category, I created variables to store the mean birth weight and its standard deviation using numpy methods ```.mean()``` and ```.std()```, and also defined a variable 'diff' for the difference in mean weights between first borns and others: 
```
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]
firsts_wgt_mean = firsts.totalwgt_lb.mean()
others_wgt_mean = others.totalwgt_lb.mean()
diff = firsts_wgt_mean - others_wgt_mean
firsts_wgt_std = firsts.totalwgt_lb.std()
others_wgt_std = others.totalwgt_lb.std()
```
I decided to calculate Cohen's *d* based on its mathematical formulation defined in the textbook as:
![cohen's d](https://toptipbio.com/wp-content/uploads/2018/08/Cohens-d-formula.jpg)

where M1 and M2 are the means of group 1 and 2, and pooled standard deviation I looked up and found to be defined as: 

![pooled std](https://toptipbio.com/wp-content/uploads/2018/08/Pooled-standard-deviation-formula.jpg)

I plugged my standard deviation variables calculated above into the pooled standard deviation formula, then plugged that result into the Cohen's *d* formula, as follows: 
```
pooled_std_wgt = np.sqrt((firsts_wgt_std**2 + others_wgt_std**2)/2)
cohens = (diff)/pooled_std_wgt
```
Which finally yielded the following result: 
```
cohens
>>> -.08864367587767744
```
Verifying with the author's solution, this is the correct value! It can be interpreted as meaning that the difference in the mean values of birth weights for first borns vs. non-first borns is 0.089 standard deviations.

The significance of this value should be investigated. 

First of all, the negative sign, based on the order I substracted my mean weights in calculating Cohen's *d* (firsts - others, or mean(x1)-mean(x2) because the text book had it defined this way and I wanted to keep with the syntax) indicates that the mean weight of firsts must be the lower value, and hence that first born babies are, on average, lighter than others. This can be double-checked by printing the mean values directly and observing that the firsts is smaller: 
```
firsts_wgt_mean, others_wgt_mean
>>> (7.201094430437772, 7.325855614973262)
```
The difference in these means is: 
```
diff
>>> -0.12476118453549034
```
0.125 lbs, or 2 oz. As a percentage of total average birth weight, ```live.totalwgt_lb.mean()``` = 7.266 lb, this difference equates to only 1.72%. The pooled standard deviation for average weights, 1.407 lb, or the standard dev of total average birth weights, ```live.totalwgt_lb.std()``` = 1.408lb, indicates that the birth weight distribution is fairly decentralized, and deviations in birthweight of up to nearly 1.5 lbs are to be expected. As such, a difference of 0.125 pounds between first and other births falls well within the standard deviation.  

Observation of the total population of live births data this way helps to contextualize the computed value of Cohen's *d*. It is clear that a value of 0.089, or a difference of 0.125 lbs (even though it may sound significant when referred to as "2 oz"), when compared to the high variability within the groups of 1.407 lb, is not practically significant. 

This is a similar result to that for pregnancy lengths found in the textbook; there does exist a small difference in the average values for first and non-first pregnancies, but Cohen's *d* is very small, indicating that the difference seen is insignificant when compared to the how widely spread the distribution of values, reducing the difference practically to noise which indeed would probably go unnoticed if not looking at an extremely large dataset. 

