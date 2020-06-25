[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

## Exercise 3.1

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

Next, I manipulated the data to reflect the responses to 'how many kids are in the household' as if answered by the kids themselves, which constitutes a biased dataset. To do this, I copied the kids pmf, then multiplied each response by itself -- performing this operation 

```
def CreateBias(pmf, label):
    biased_pmf = pmf.Copy(label=label)
    
    for x, p in pmf.Items():
        biased_pmf.Mult(x, x)
        
    biased_pmf.Normalize()
    return biased_pmf
kids_biased = CreateBias(kids, label="observed")
thinkplot.PrePlot(2)
thinkplot.Pmfs([kids, kids_biased])
thinkplot.Config(xlabel="Number of Kids in Household", ylabel="PMF")
kids.Mean(), kids_biased.Mean()
```
