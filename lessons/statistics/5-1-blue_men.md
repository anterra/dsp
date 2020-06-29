[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

> This is a classic example of hypothesis testing using the normal distribution. The effect size used here is the Z-statistic.

> In the BRFSS (see Section 5.4), the distribution of heights is roughly normal with parameters µ = 178 cm and σ = 7.7 cm for men, and µ = 163 cm and σ = 7.3 cm for women.

> In order to join Blue Man Group, you have to be male between 5’10” and 6’1” (see http://bluemancasting.com). What percentage of the U.S. male population is in this range? Hint: use scipy.stats.norm.cdf.

As part of the example code for this chapter, following the question prompt, the author provided the first few steps in approaching this problem using scipy. After importing scipy, he defined variables for mu (the median for men's heights) and sigma (the standard deviation for the same), and used ```scipy.stats.norm``` to generate a normal distribution representing men's heights. I have copied that code here: 

```
import scipy.stats

mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)
```
The parameters included in the function were loc, which defines where you want the distribution centered, here specified at the mean, and scale which sets the standard deviation. 

A ```scipy.stats.norm``` object has a ```.cdf``` function available. A CDF, provided a value, will return that value's percentile rank -- or, the fraction of values within the distribution that are less than or equal to the input value. For example, if given a value of mu, the median, a CDF returns 0.5 -- since 50% of the values will fall below the median (this is just one definition of median, but is convention for CDF). 

To find the percentage of men between 5'10" and 6'11" within this distribution, first I converted those values to centimeters: yielding 177.8cm and 185.4cm. 

I then plugged those each into the ```.cdf``` function and assigned them variables to represent the top and bottom of the range we want to investigate: 

```
top = dist.cdf(185.4)
top
>>> 0.8317337108107857

bottom = dist.cdf(177.8)
bottom 
>>> 0.48963902786483265
```
Subtracting these two values thus gives the fraction of men falling between those heights: 
```
top - bottom 
>>> 0.3420946829459531
```
So, 34.2% of the male population are eligible (at least by height) to be in the Blue Man Group. 
