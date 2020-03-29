[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

# Link to Jupyter Notebook: [CHU_CH2_E2.ipynb](https://github.com/jonjonchu/ThinkStats2/blob/master/code/CHU_CH2_E2.ipynb)

# Import libraries
    import pandas as pd
    import nsfg
    import thinkstats2
    import thinkplot

    df = nsfg.ReadFemPreg()

# Create DataFrame of live births
    live = df[df.outcome == 1]

# Create DataFrames of first and other births

    firsts = live[live.pregordr == 1]
    others = live[live.pregordr != 1]

# Calculate Cohen's *d* for totalwgt_lb
An effect size is a summary statistic intended to describe (wait for it) the size of an effect. For example, to describe the difference between two groups, one obvious choice is the difference in the means.

Cohen’s d is a statistic intended to do that; it is defined

###  *d* = (x_1 − x_2) / s
  
where __x_1__ and __x_2__ are the means of the groups and __s__ is the “pooled standard deviation”.

## Write a function that calculates Cohen's *d*
    from math import sqrt
    def cohens_d(group1, group2):
      mean1 = group1.mean()
      mean2 = group2.mean()
    
      var1 = group1.var()
      var2 = group2.var()
      n1, n2 = len(group1), len(group2)
    
      pooled_var = (n1*var1 + n2* var2) / (n1 + n2)
      d = (mean1 - mean2) / sqrt(pooled_var)
    
      return d

# Cohen's *d* for totalwgt_lb is: 
    totalwgt_lb_d = cohens_d(firsts.totalwgt_lb, others.totalwgt_lb)

    '%s' % float('%.2g' % totalwgt_lb_d)

-0.069

# And for pregnancy length prglngth:
    prglngth_d = cohens_d(firsts.prglngth, others.prglngth)

    '%s' % float('%.2g' % prglngth_d)

0.014

# Cohen's *d* is larger in magnitude for total weight than for pregnancy length, when comparing first babies to later babies (-0.069 vs 0.014); however, whether such an effect is statistically significant is unknown. (It probably isn't, though.)
