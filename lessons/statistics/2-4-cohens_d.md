[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

## Exercise 2.4

"Using the variable totalwgt_lb, investigate whether first babies are lighter or heavier than others. Compute Cohen's d to quantify te difference between the groups. How does it compare to the difference in pregnancy length?"

My approach to this problem, in order to gain more practice, was to start from scratch with selecting the data I wanted to work with, and then to calculate Cohen's *d* myself based on its mathematical definition, rather than by using the author's pre-defined python method. 

I first imported the relevant packages: 
```
import numpy as np
import nsfg
```
Then, I used the author's method "ReadFemPreg" to read in the nsfg pregnancy data, which also cleans it, removes invalid entries, and defines the recode variable "totalwgt_lb" from the raw data's separate birthwgt_lb and birthwgt_oz variables. I then filtered the data to only look at only pregnancies which ended in live births (outcome of value "1" indicates live birth):
```
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]
```
Next, I divided the live births DataFrame into subsets based on whether the pregnancy was a first or not. For each category, I created variables to store the mean birth weight and its standard deviation using numpy methods ```.mean()``` and ```.std()```: 
```
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]
firsts_wgt_mean = firsts.totalwgt_lb.mean()
others_wgt_mean = others.totalwgt_lb.mean()
firsts_wgt_std = firsts.totalwgt_lb.std()
others_wgt_std = others.totalwgt_lb.std()
```
I decided to calculate Cohen's *d* based on its mathematical formulation defined in the textbook as:
![cohen's d](https://toptipbio.com/wp-content/uploads/2018/08/Cohens-d-formula.jpg)

where M1 and M2 are the means of group 1 and 2. That formula was dependent upon a pooled standard devation, which I looked up and found defined as:


![pooled std](https://toptipbio.com/wp-content/uploads/2018/08/Pooled-standard-deviation-formula.jpg)

I plugged my standard deviation variables calculated above into the pooled standard deviation formula, then plugged that result into the Cohen's *d* formula, as follows: 
```
pooled_std_wgt = np.sqrt((firsts_wgt_std**2 + others_wgt_std**2)/2)
cohens = (firsts_wgt_mean - others_wgt_mean)/pooled_std_wgt
```
Which finally yielded the following result: 
```
cohens
>>> -.08864367587767744
```
Verifying with the author's solution, this is the correct value! It can be interpreted as meaning that the difference in the mean values of birth weights for first borns vs. non-first borns is 0.089 standard deviations. The negative sign, based on the order I substracted my mean weights in calculating Cohen's *d* (firsts - others, or mean(x1)-mean(x2) because the text book had it defined this way and I wanted to keep with the syntax) indicates that the mean weight of firsts must be the lower value, and hence that first born babies are, on average, lighter than others. This can be double checked by printing the mean values directly and observing that the firsts is smaller: 
```
firsts_wgt_mean, others_wgt_mean
>>> (7.201094430437772, 7.325855614973262)
```
The value, 0.089, should be analyzed for statistical significance. In looking at the standard deviations 
