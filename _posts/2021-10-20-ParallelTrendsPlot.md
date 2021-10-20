---
layout: post
title: "Visually assessing the parallel trends assumption for DID estimation with control variables"
cover: 
date: 2021-10-20 12:50:00
categories: r
tags: [R, RTutor, shiny]
---


Difference-in-Difference (DID) estimation is a very intuitive and popular approach to estimate causal effects (see [here](skranz.github.io/r/2021/01/27/EmpEconC.html) for my take on teaching it). If you know about DID and want to directly know how to create a plot to assess the parallel trends assumption if we have additional control variables [click here](http://skranz.github.io/r/2021/10/20/ParallelTrendsPlot.html#plot). Otherwise, we proceed step-by-step.

## DID estimation and parallel trends assumption in a nutshell

Let us simulate a data set for the DID estimation. We have `N=40` subjects equally divided into a treatment and control group, yet *not* in a perfectly randomized fashion. We observe an outcome `y` for 50 periods before the experiment starts and 50 periods during the experiment.

```r
library(dplyr)
set.seed(12345)
T = 100 # no of periods
N = 40 # no of subjects

dat = expand.grid( t = 1:T,i = 1:N) 

# Simulate a common AR(1) time trend
time.trend = as.numeric(arima.sim(n=T,list(ar = c(0.4,0.5), ma=c(0.6,0.5))))*3+0.7*(1:T)

dat = mutate(dat,
  group = ifelse(i > N/2,"treat","control"),
  treat = 1L*(group == "treat"), 
  exp = 1L*(t > T/2),
  treat_exp = exp*treat,
  mu.t = time.trend[t],
  eps = rnorm(n()),
  y = mu.t + treat*40 + treat_exp*50 + eps
)
sample_n(dat, 5)
```

```
##    t  i group treat exp treat_exp     mu.t        eps         y
## 1 50 31 treat     1   0         0 23.71765  0.2370906  63.95474
## 2  8 21 treat     1   0         0 18.62367 -0.2297932  58.39387
## 3 22 26 treat     1   0         0 19.90087  1.5255634  61.42644
## 4 62 40 treat     1   1         1 33.21133 -0.3764318 122.83490
## 5 37 22 treat     1   0         0 28.60870 -0.7653263  67.84337
```

Let us draw a plot:

```r
show.plot = function(dat,label="", show.means=TRUE) {
  library(ggplot2)
  gdat = dat %>%
    group_by(group, t,exp,treat) %>%
    summarize(y = mean(y))
    
  gg = ggplot(gdat, aes(y=y,x=t, color= group)) +
    geom_line() + 
    geom_vline(xintercept=T/2) +
    theme_bw() +
    annotate("text",x=T/4, y = 0.9*max(gdat$y), label=label)
    
  if (show.means) {
    y.pre.tr <<- mean(filter(gdat,treat==1, exp==0)$y) %>% round(1)
    y.exp.tr <<- mean(filter(gdat,treat==1, exp==1)$y) %>% round(1)
    y.pre.co <<- mean(filter(gdat,treat==0, exp==0)$y) %>% round(1)
    y.exp.co <<- mean(filter(gdat,treat==0, exp==1)$y) %>% round(1)
    gg = gg + 
      annotate("label", x=T/4, y=y.pre.tr+15,label=y.pre.tr) +
      annotate("label", x=T/4, y=y.pre.co-15,label=y.pre.co) +
      annotate("label", x=T*0.75, y=y.exp.tr+15,label=y.exp.tr) +
      annotate("label", x=T*0.75, y=y.exp.co-15,label=y.exp.co)
  }
  gg
}  
show.plot(dat)
```
<center>
<img src="http://skranz.github.io/images/paratrends/unnamed-chunk-2-1.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

The code shows that the true causal effect of the treatment on average daily outcomes is given by 50. A naive estimate for the causal effect of the treatment would be 

141.1 - 67.4 = 73.7

where we simply take the daily mean of the treatment group `y` during the experimental phase (141.1) and subtract the daily mean before the experiment (67.4). Yet, this simple difference estimator is biased if `y` also changes over time due to reasons unrelated to the experiment. This is the case in our data set.

The idea of DID estimation is to have a control group (red line) that is not affected by the experiment to correct for such time trends.  Here the DID estimator is given by:

(141.1 - 67.4)-(51 - 27.4) = 73.7 - 23.6 = 50.1

This is pretty close to the actual causal effect of 50. For the DID approach the assignment to the treatment and control group does not have to be perfectly randomized. Indeed, in our example the control group has systematically lower outcomes than the treatment group already before the experiment.

Crucial is the

>*Parallel Trends Assumption:* Absent treatment the outcomes for the control and treatment group would follow parallel trends. 

We can never completely check the parallel trends assumption since we don't observe the complete outcome path for the treatment group absent treatment. One typically checks (often visually) whether the trends of both groups run in parallel before the treatment (parallel pretrends).

In our simulated example, the pre-trends are almost perfectly parallel which gives substantial confidence that the DID estimator is appropriate. In practice, the DID estimator is typically implemented by running a linear regression:


```r
library(broom)
tidy(lm(y ~ exp*treat, data=dat))
```

```
## # A tibble: 4 x 5
##   term        estimate std.error statistic   p.value
##   <chr>          <dbl>     <dbl>     <dbl>     <dbl>
## 1 (Intercept)     27.4     0.443      62.0 0        
## 2 exp             23.6     0.626      37.7 5.32e-266
## 3 treat           40.0     0.626      63.8 0        
## 4 exp:treat       50.1     0.885      56.6 0
```

The coefficient of the interaction term `exp:treat` corresponds to the DID estimator. One advantage of the regression approach is that we directly get standard errors for the DID estimator. Another advantage is that one can add control variables, as we explain below.


## Adding a confounder that leads to non-parallel trends

We now assume that the outcome `y` also depends on a confounding variable `x` that develops differently for the control and treatment group over time:


```r
dat = dat %>% mutate(
  x = ifelse(treat,-t, t)+runif(n())*2,
  y = mu.t + treat*40 + treat_exp*50 + 0.8*x + eps
)
show.plot(dat, show.means=FALSE, label="Pre-trends not parallel")
```

<center>
<img src="http://skranz.github.io/images/paratrends/unnamed-chunk-4-1.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

The pre-trends now look far from parallel, casting doubt on the applicability of our previous DID estimator. Let's repeat the estimation.


```r
tidy(lm(y ~ exp*treat, data=dat))
```

```
## # A tibble: 4 x 5
##   term        estimate std.error statistic   p.value
##   <chr>          <dbl>     <dbl>     <dbl>     <dbl>
## 1 (Intercept)   48.6       0.574     84.6  0        
## 2 exp           63.6       0.812     78.3  0        
## 3 treat         -0.830     0.812     -1.02 3.07e-  1
## 4 exp:treat    -29.9       1.15     -26.1  2.07e-138
```

We now wrongly estimate a negative treatment effect. One advantage of the regression approach is that we can add additional control variables. So let's add `x` as control variable.


```r
tidy(lm(y ~ exp*treat+x, data=dat))
```

```
## # A tibble: 5 x 5
##   term        estimate std.error statistic   p.value
##   <chr>          <dbl>     <dbl>     <dbl>     <dbl>
## 1 (Intercept)   27.4      0.601       45.6 0        
## 2 exp           23.5      0.990       23.8 7.96e-117
## 3 treat         40.0      1.00        40.0 2.38e-294
## 4 x              0.801    0.0153      52.3 0        
## 5 exp:treat     50.2      1.77        28.4 1.91e-161
```

Hooray, we are back to a consistent estimator of the treatment effect.

The question I asked myself is: 

> How can we visually assess whether pre-trends are parallel when accounting for the control variables?


### Wrong attempt using residuals of y that cannot be explained by the control variable x

Intuitively, we want to remove the impact of $x$ in our trends plot. A  first idea that may come to mind is to regress $y$ on $x$ and show a plot of the residuals for the control and treatment group. The idea is that those residuals capture the variation in $y$ that is not explained by the confounder $x$


```r
dat$y.org = dat$y
dat$y = resid(lm(y~x,data=dat))
show.plot(dat,show.means = FALSE)
```
<center>
<img src="http://skranz.github.io/images/paratrends/unnamed-chunk-7-1.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

Hmm, the resulting pre-trends don't look very parallel. Indeed, this residual approach does not work. For some intuition about what is going wrong, you can take a look at [my earlier post about regression anatomy](http://skranz.github.io/r/2020/07/01/PuzzlingRegressionAnatomy.html). 
<a id="plot"></a>
## Correct approach (I hope) for trends plot adjusted for control variables

Here is an approach that seems to work. We first estimate the complete DID regression including the additional control variables. Then we predict the outcomes for treatment and control groups assuming that the control variables don't change over time. We also add the residuals of the original regressions:

```r
reg = lm(y~exp*treat+x, data=dat)

# Fictitious data set where control variables
# have a constant value for treatment and control
# group: here just set everywhere 0
dat0 = mutate(dat, x=0)

# Now predict y and add residuals of original regression
dat$y = predict(reg, dat0) + resid(reg)
show.plot(dat,show.means = FALSE, label="parallel pretrends")
```
<center>
<img src="http://skranz.github.io/images/paratrends/unnamed-chunk-8-1.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

We now get again nicely parallel pre-trends consistent with the fact that the DID estimator with `x` as control variable indeed works in this example.

Is this really a correct approach that generally works? I hope, but must admit that I am not completely sure as I have not found a treatment of this kind of plots somewhere else in the literature. (Admittingly, I know only a very small fraction of the literature). If you know more, please consider to leave a comment below.

## Using the ParallelTrendsPlot package

I also wrote a small R package to facilitate the generation of the trend plots given control variables. Below is the example:

```r
library(ParallelTrendsPlot)
dat$y=dat$y.org
pt.dat = parallel.trends.data(dat,cvars="x")
parallel.trends.plot(pt.dat) + theme_bw()
```
<center>
<img src="http://skranz.github.io/images/paratrends/unnamed-chunk-9-1.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

This function is constructed such that control and treatment groups have the same average values as in the original data set.

## Formal test for parallel pretrends using the did package

If you want to formally test the assumption of parallel pretrends take a look at the package [did](https://cran.r-project.org/web/packages/did/index.html) that is written by absolute experts in the field. The [vignette about pre-testing](https://cran.r-project.org/web/packages/did/vignettes/pre-testing.html) also discusses possible pitfalls that one should be aware of also in a pure graphical analysis.

{% include ghcomments.html issueid=5 %}

