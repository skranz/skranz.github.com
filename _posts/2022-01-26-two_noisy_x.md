---
layout: post
title: "Riddle: Estimate effect of x on y if you only have two noisy measures of x."
cover: 
date: 2022-01-26 08:15:00
categories: r
tags: [R]
---

Consider the following data generating process:


```r
set.seed(1)
n = 100000
x = rnorm(n)

eta1 = rnorm(n) # measurement error1
noisy1 = x + eta1

eta2 = rnorm(n) # measurement error2
noisy2 = x + eta2

u = rnorm(n)
beta0=0; beta1 = 1
y = beta0+beta1*x + u
```

Can you solve the following statistics riddle? We want to consistently estimate the causal effect `beta1 = 1` of `x` on `y`. We don't observe `x` but only `noisy1` and `noisy2`, which are noisy versions of `x` whose unobserved measurement errors `eta1` and `eta2` are independently distributed from each other.  (My answer consists of one line of R code using a common econometrics package.)

## Discussion and Solution

For starters let us just regress `y` on `noisy1`:


```r
coef(lm(y~noisy1))
```

```
## (Intercept)      noisy1 
## -0.00285417  0.50197301
```

Our estimator for `beta1` in this OLS regression is biased towards 0. That is the well known *attenuation bias*. 

Here a short explanation for the bias. The true relationship is

`y = beta0 + beta1 * x + u`

But we estimate the regression

`y = beta0 + beta1 * noisy1 + eps`

Since the right hand side of our estimating equation must be the same as in the true relationship, we know that

```
eps = beta1 * x - beta1 *noisy1 + u
    = beta1 * (x - noisy1) + u
    = -beta1 * eta1 + u
```
Our OLS estimator is consistent only if our explanatory variable `noisy1` is uncorrelated with the error term `eps`. Yet, since `noisy1` is positively correlated with `eta1`, it is negatively correlated with `eps`. The bias of our OLS estimator for `beta1` has the same sign as the correlation between `noisy1` and `eps`. Here, this correlation always has the opposite sign of `beta1` and our estimator is thus biased towards 0.

**Solution of the riddle:**

A consistent estimator of `beta1` comes from a pretty popular method in applied econometrics to overcome endogeneity problems: instrumental variable regression. We can consistently estimate `beta1` in the regression equation

`y = beta0 + beta1 * noisy1 + eps`

if we have an instrumental variable that is correlated with `noisy1` and uncorrelated with the error term `eps = -beta1 * eta1 + u`. Well it happens that our second noisy measure `noisy2 = x + eta2` fulfills both conditions. Obviously it is correlated with `noisy1` since both are noisy measures of `x` and since the measurement errors `eta1` and `eta2` are independent in our data generating process, `noisy2` is also uncorrelated with `eps`. Let's run the instrumental variable regression using the `ivreg` function from the AER package.


```r
AER::ivreg(y~noisy1 | noisy2)
```

```
## 
## Call:
## AER::ivreg(formula = y ~ noisy1 | noisy2)
## 
## Coefficients:
## (Intercept)       noisy1  
##   -0.002249     1.000015
```
Yep, looks like a consistent estimator!

Somehow I like this observation: both `noisy1` and `noisy2` are absolutely symmetric but one gets the role of explanatory variable, the other the role of instrument. Of course, we could also just swap instrument and explanatory variable:

```r
AER::ivreg(y ~ noisy2 | noisy1)
```

```
## 
## Call:
## AER::ivreg(formula = y ~ noisy2 | noisy1)
## 
## Coefficients:
## (Intercept)       noisy2  
##   -0.001177     0.997248
```

(You can try out yourself: If `noisy1` and `noisy2` have different precision, does swapping their roles systematically affect the precision of the resulting estimate?)


I don't know whether the result is of much practical importance, though. How often do we have two noisy measures whose measurement errors are independent from each other?

If the measurement errors are correlated with each other, the procedure does not yield a consistent estimator. Here is an illustration for positively correlated measurement errors.

```r
eta2 = 0.5*eta1+ 0.5*rnorm(n)
noisy2 = x + eta2
y = beta0+beta1*x + u
AER::ivreg(y~noisy1 | noisy2)
```

```
## 
## Call:
## AER::ivreg(formula = y ~ noisy1 | noisy2)
## 
## Coefficients:
## (Intercept)       noisy1  
##   -0.002649     0.671033
```

Now, an attenuation bias still remains. It is just reduced a bit compared to the OLS estimator.






