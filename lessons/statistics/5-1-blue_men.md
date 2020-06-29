[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

> This is a classic example of hypothesis testing using the normal distribution. The effect size used here is the Z-statistic.

> In the BRFSS (see Section 5.4), the distribution of heights is roughly normal with parameters µ = 178 cm and σ = 7.7 cm for men, and µ = 163 cm and σ = 7.3 cm for women.

> In order to join Blue Man Group, you have to be male between 5’10” and 6’1” (see http://bluemancasting.com). What percentage of the U.S. male population is in this range? Hint: use scipy.stats.norm.cdf.

As part of the example code for this chapter, following the question prompt, the author provided the first few steps in approaching this problem using scipy. After importing scipy, he defined variables for mu (the mean for men's heights) and sigma (the standard deviation for the same), and used ```scipy.stats.norm``` to generate a normal distribution representing men's heights. I have copied that code here: 

```
import scipy.stats

mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)
```
The parameters included in the function were loc, which defines where you want the distribution centered, here specified at the mean, and scale which sets the standard deviation. 

A ```scipy.stats.norm``` object has a ```.cdf``` function available. CDF 
