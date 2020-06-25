[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

## Exercise 3.1

>"This problem presents a robust example of actual vs biased data. As a data scientist, it will be important to examine not only the data that is available, but also the data that may be missing but highly relevant. You will see how the absence of this relevant data will bias a dataset, its distribution, and ultimately, its statistical interpretation."

>"Something like the class size paradox appears if you survey children and ask how many children are in their family. Families with many children are more likely to appear in your sample, and families with no children have no chance to be in the sample.

>Use the NSFG respondent variable numkdhh to construct the actual distribution for the number of children under 18 in the respondents' households. Now compute the biased distribution we would see if we surveyed the children and asked them how many children under 18 (including themselves) are in their household. Plot the actual and biased distributions, and compute their means."

To complete this problem, I followed the structure of the chapter's prior examples. This began with reading in the respondent data, and creating a probability mass function for the number of kids in each respondent's household which I called "kids":

```
resp = nsfg.ReadFemResp()
kids = thinkstats2.Pmf(resp.numkdhh, label="actual")
```
```
kids
>>> Pmf({0: 0.466178202276593, 1: 0.21405207379301322, 2: 0.19625801386889966, 3: 0.08713855815779145, 
4: 0.025644380478869556, 5: 0.01072877142483318}, 'actual')
```
The "kids" pmf was given the label "actual", because it represents the actual probabilities of having a given number of kids in a household for this data. 

Next, I manipulated the data to reflect the responses to 'how many kids are in the household' as if answered by the kids themselves, which constitutes a biased dataset. To do this, I copied the kids pmf, then multiplied each response by itself:

```
def CreateBias(pmf, label):
    biased_pmf = pmf.Copy(label=label)
    
    for x, p in pmf.Items():
        biased_pmf.Mult(x, x)
        
    biased_pmf.Normalize()
    return biased_pmf
```
```
kids_biased = CreateBias(kids, label="observed")
```
```
kids_biased
>>> Pmf({0: 0.0, 1: 0.20899335717935616, 2: 0.38323965252938175, 3: 0.25523760858456823, 4: 0.10015329586101177, 5: 0.052376085845682166}, 'observed')
```
Performing this operation skews the data, effectively weighting each response by how likely a kid with that number of kids in their family would be in your sample -- a family with 5 kids would have 5 respondents all answering '5', while an only child would only constitute 1 respondent answering '1'.

Plotting both the actual and biased pmfs visually and effectively demonstrates the change in probablilty distribution, as seen below: 
```
thinkplot.PrePlot(2)
thinkplot.Pmfs([kids, kids_biased])
thinkplot.Config(xlabel="Number of Kids in Household", ylabel="PMF")
```
![graph]

As seen in both the plot and the output values for kids vs. kids_biased, 
```
kids.Mean(), kids_biased.Mean()
```
