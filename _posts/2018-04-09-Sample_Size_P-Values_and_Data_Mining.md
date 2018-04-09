---
layout: post
title: 'P-Values, Sample Size and Data Mining'
cover: 
date: 2018-04-09 12:00:00
categories: r
tags: [R]
---  
  

Recently, a paper was presented at our university that showed a significant effect for a variable of interest but had a relatively small number of observations. One colleague suggested that we should consider the significance of the results with care since the number of observations was fairly small.

This ignited some discussion. Given that the significance test computed exact p-values, why should the significance of the results be less convincing than if we had a larger sample? Independent of the sample size the probability to find a result significant at a 5% level if there is actually no effect should be 5%.

The discussion turned to the question whether small sample sizes could sometimes be problematic because they may magnify possible biases if some data mining takes place. Indeed the dangers of data mining and 'p-hacking' are a regular theme in statistics literature and on statistics blogs. To get a better gut feeling of the relationship between sample size and possible biases from data mining, I just have run different Monte-Carlo simulations in R that are shown below.   


## Freedman's simulation study
I am currently reading the very nice book [Counterfactuals and Causal Inference](https://www.amazon.com/Counterfactuals-Causal-Inference-Principles-Analytical/dp/1107694167/ref=dp_ob_title_bk) by Stephen Morgan and Christopher Winship. They cite a [classical simulation study by Freedman (1983)](https://amstat.tandfonline.com/doi/abs/10.1080/00031305.1983.10482729) that had a result that I found quite surprising and can be related to the question above.

Let us replicate the study with slightly modified numbers in R.

```r
set.seed(123456789)
vars = 100 # Number of explanatory variables
obs = 150 # Number of observations

# Create 100 random iid explanatory variables
x = matrix(rnorm(obs*vars), obs, vars)
colnames(x) = paste0("x",1:vars)
# A dependent variable that is independent of all x
y = rnorm(obs)

# Combine to data frame
dat = cbind(data.frame(y=y),x)

# Regress y on all 100 explanatory variables
reg1 = lm(y~., data=dat)

# Collect regression results in a data frame
# and extract p-values
library(broom)
res1 = broom::tidy(reg1)
p = res1$p.value[-1]

# Count the number of significant variables
# at different significance levels
sum(p <= 0.01)
```

```
## [1] 1
```

```r
sum(p <= 0.05)
```

```
## [1] 5
```

```r
sum(p <= 0.10)
```

```
## [1] 13
```
We have 100 explanatory variables that are completely independent from each other and from the dependent variable y. We run a regression of y on all x and find that 1 of the 100 explanatory variables is significant at a 1% level, 5 at a 5% level and 13 at a 10% level. Nothing surprising so far.

Now we select the subset of variables that has a p-value below 0.25. This can roughly reflect some data driven selection of variables, which unfortunately may well happen in some research papers. We run the regression again with the subset of the 37 selected variables.


```r
# Select variables with p-value below 25%
used.vars = which(res1$p.value[-1] <= 0.25)
length(used.vars) 
```

```
## [1] 37
```

```r
dat2 = dat[,c(1,used.vars+1)]

# Regress y on selected explanatory variables
# that had p-value below 25% in first regression
reg2 = lm(y~., data=dat2)

# Collect regression results in a data frame
# and extract p-values
res2 = broom::tidy(reg2)
p = res2$p.value[-1]

# Count the number of significant variables
# at different significance levels
sum(p <= 0.01)
```

```
## [1] 7
```

```r
sum(p <= 0.05)
```

```
## [1] 19
```

```r
sum(p <= 0.10)
```

```
## [1] 25
```
The absolute number of significant variables has strongly risen! Now 7 variables are significant at the 1% level compared to the one variable in the original regression, and 19 variables are significant at the 5% level, compared to 5 in the original regression.

So the problem of pre-selection is worse than just the obvious effect that the relative share of significant variables in the 37 selected variables increases compared to the share in the 100 original variables. Also the absolute number of significant variables increases by large factors.


The following graph shows the results of a systematic Monte-Carlo study. I have repeated the simulation above for different combinations of number of variables and number of observations. For each combination I have run the simulation 200 times and computed the average share of coefficients that is significant at the 1% in the second regression. To be clear, the share is computed by dividing the number of significant variables after pre-selection by the *original number* of variables before pre-selection.

<img src="http://skranz.github.io/images/freedman_1.svg">

The share of significant variables only seems to depend on the ratio of observations to the original number of variables. The share of 1% significant coefficients is maximized at a level above 5% for a ratio of around 1.5, and goes down, converging to 1%, as the number of observations grows large.

Interestingly, if the ratio of observations to variables falls below 1.5, the share of significant variables also goes down. I must admit that my intuition for the geometry of OLS estimators is not good enough to grasp why we have this non-monotonicity.

Yet, even though there is no monotone relationship, having a low number of observations may make it more likely to be in a range where variable pre-selection can substantially increases the absolute number of significant variables.


The following plot illustrates another issue of small sample sizes. It plots the mean absolute size of those coefficients that are significant at a 5% (before variable selections) against the number of observations:

<img src="http://skranz.github.io/images/freedman_2.svg">

Not surprisingly, we see that for a smaller number of observations, the coefficients of the spuriously significant variables have larger absolute values. Hence, also effect sizes may be taken with some grain of salt when the number of observations is small. Of course this should typically already be reflected, at least to some extend, in larger standard errors.


## The greedy p-hacker

Let us now consider a variation of the study above. Assume a p-hacker systematically eliminates explanatory variables of a regression with the goal to find a significant effect of the explanatory variable `x1` on the dependent variable `y`. As in our initial example, assume we have `obs=150` observations of one dependent variable `y` and of `vars=100` explanatory variables that are all independently drawn from each other and not related to `y`.

The p-hacker proceeds according to the following greedy algorithm:

- The p-hacker first runs the regression of y on all 100 explanatory variables and notes the p-value for x1.

- He then runs a regression in which he omits explanatory variable x2. If the resulting p-value for x1 gets smaller, he permanently removes x2 from all future regression, otherwise he keeps x2.

- He then moves on in the same fashion through all other explanatory variables x3, x4, x5, ... and restarts the loop after reaching the last explanatory variable.

- He repeats the process until he can no longer improve the p-value of x1 by removing any more explanatory variable.

The R function `greedy.phack` below simulates values and implements this greedy p-hacking algorithm. To speed up computations, we use the function `fastLmPure` from the package RcppEigen. (Actually, the help for RcppEigen recommends to use `lm.fit` for quick regressions instead, but `fastLmPure` also directly returns standard errors which is convenient for computing p-values).

```r
compute.p.value = function(reg,vars, obs, row=2) {
  #restore.point("fastLm.p")
  df = obs-vars-1
  t = reg$coefficients[row] / reg$se[row]
  p = pt(t,df)
  p = 2*(pmin(p,1-p))
  p 
}

greedy.phack = function(vars=100, obs=round(vars*factor), factor=obs/vars, verbose=FALSE) {
  y = rnorm(obs)
  x = matrix(rnorm(obs*vars), obs, vars)
  
  X = cbind(1,x)
  reg = RcppEigen::fastLmPure(y=y,X=X)
  p.org = p = compute.p.value(reg, vars, obs, row=2)
  coef.org = reg$coefficients[2]
  
  ind = 2
  steps = 0
  deletions = 0
  while (TRUE) {
    ind = ind+1
    if (ind > NCOL(X))
      ind = 3
    steps = steps+1
    reg = RcppEigen::fastLmPure(y=y,X=X[,-ind])
    p.cur = compute.p.value(reg, vars, obs, row=2)
    
    # Did the ommission reduce the p-value?
    if (p.cur < p) {
      deletions = deletions+1
      X = X[,-ind]
      if (verbose) {
        cat(paste0("\n", deletions,". p from ", round(p*100,2) , "% to ", round(p.cur*100,2),"%"))
      }
      p = p.cur
      steps = 0
      
      # Stop in case that only x1 remains
      if (NCOL(X)<=2) break
    }
    
    # Stop if p valued could not be reduced
    # during a loop through all variables
    if (steps > NCOL(X)-2) break
  }
  list(
    vars = vars,
    obs = obs,
    factor = factor,
    p.org = p.org,
    p = p,
    coef.org = coef.org,
    coef = reg$coefficients[2],
    deletions=deletions
  )
}
```

Let us run the simulation one time...

```r
set.seed(123456789)
greedy.phack(vars=100, obs=150)
```

```
## $vars
## [1] 100
## 
## $obs
## [1] 150
## 
## $factor
## [1] 1.5
## 
## $p.org
## [1] 0.6996701
## 
## $p
## [1] 0.002898235
## 
## $coef.org
## [1] 0.04955448
## 
## $coef
## [1] 0.2287564
## 
## $deletions
## [1] 49
```
We can interpret the output as follows. In the initial regression, x1 was clearly insignificant with a p-value of 70%. The algorithm then iteratively deleted a total of 49 variables. In the final regression with 51 remaining explanatory variables, x1 became highly significant with a p-value of 0.29%. Also the regression coefficient of x1 increased more than 4-fold from an original 0.05 to 0.23 in the final regression.

Let us now repeat the simulation with 10000 instead of only 150 observations:

```r
set.seed(123456789)
greedy.phack(vars=100, obs=10000)
```

```
## $vars
## [1] 100
## 
## $obs
## [1] 10000
## 
## $factor
## [1] 100
## 
## $p.org
## [1] 0.6100959
## 
## $p
## [1] 0.3679074
## 
## $coef.org
## [1] 0.005237386
## 
## $coef
## [1] 0.009142232
## 
## $deletions
## [1] 52
```
In the original regression, the coefficient of x1 was insignificant with a p-value of 61%. In total 52 variables variables were deleted. Yet, the resulting coefficient of x1 remains insignificant with a p-value of 37%. The coefficient size after selection has a level of 0.009, which is much lower than in our small sample example. 


I have also run a Monte-Carlo study where I repeated the simulation above a 1000 times each for different combinations of numbers of variables and observations. The following plot shows the percentage of simulations in which the p-hacker was able to achieve a p-value for x1 below 1%. 

<img src="http://skranz.github.io/images/p-hacker.svg">

For the case of 100 variables and 150 observations, the p-hacker managed to a get p-value for x1 below 1% in 94% of the simulations. Personally, I am indeed surprised by this large percentage given that the "only" thing the p-hacker does is to remove variables from the original regression.

We see how this percentage decreases with more observations. This suggests that a very large number of observations may provide some safeguard against p-hacking. Also, not surprisingly, this form of p-hacking gets harder if the set of candidate variables that can be removed is smaller.

