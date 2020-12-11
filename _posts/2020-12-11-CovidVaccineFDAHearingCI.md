---
layout: post
title: "A quiz about a 95% CI interpretation in the FDA Covid vaccine meeting"
cover: 
date: 2020-12-11 18:00:00
categories: r
tags: [R, Covid]
---

Personally, I think it was a great decision of the FDA to make the material of their important meeting for the Emergency Use Authorization of the Biontech/Pfizer vaccine [publicly available](https://www.fda.gov/advisory-committees/advisory-committee-calendar/vaccines-and-related-biological-products-advisory-committee-december-10-2020-meeting-announcement). In particular, they published the whole meeting as a [YouTube video](https://www.youtube.com/watch?v=owveMJBTc2I), which contains very interesting presentations.

(I hope the EMA will, perhaps later in less stressful times, be similar transparent so that the interested public can understand which additional investigations were performed that likely will have caused the vaccine to be available only at a later date in the EU. This would allow to better assess whether also a faster Emergency Use Authorization may be sensible for the EU should the next pandemic arrive.)

But let us ignore for a moment big important questions and just consider a small statistical quiz for recreation. Take a look at Pfizer's explanation of the 95% CI `(90.3, 97.6)` for the vaccine efficacy at around [4 hours 50 minutes of the meeting video](https://www.youtube.com/watch?v=owveMJBTc2I&t=4h50m25s) where the presenter states: 

> There is 95% probability that efficacy falls in the interval shown 

Here is the short quiz (click [here](https://skranz.github.io/r/2020/12/11/CovidVaccineFDAHearingCI.html#quiz) if Google forms are not properly shown):<a name="quiz"></a>

<iframe src="https://docs.google.com/forms/d/e/1FAIpQLScRO0t6RGgY-7QmSa_3n0HGJ-Q3Qv9iaPdqVsZ6EzPkJnjUmw/viewform?embedded=true" width="640" height="417" frameborder="0" marginheight="0" marginwidth="0">Loading…</iframe>

<br>
Well, I would say the interpretation is mostly correct.

What??? You may ask. Isn't that statement an example for the most common misinterpretation of confidence intervals? (An interesting discussion of this and other potential misinterpretations of confidence interval is [here](https://link.springer.com/article/10.3758/s13423-015-0947-8).) Sure, that is a wrong interpretation of a confidence interval, but looking at [Table 9 in Pfizer's briefing document](https://www.fda.gov/media/144246/download#page=55) we see that 95% CI here is the abbreviation for the Bayesian 95% [credible interval](https://en.wikipedia.org/wiki/Credible_interval)! You can look at my [earlier post](http://skranz.github.io/r/2020/11/11/CovidVaccineBayesian.html) for an explanation of how Pfizer has computed it. I guess when you work as a statistician in a company and have to make a presentation for somebody else, there are really advantages of using a Bayesian framework. Finally, you don't have to worry that the presenter starts interpreting a 95% CI... one should just hope that the audience is polite enough not to ask about stuff like priors.

What is tricky, though, with Pfizer's presentation is that when the video proceeds to the [subgroup analysis](https://www.youtube.com/watch?v=owveMJBTc2I&t=4h51m30s), the reported 95% CI is indeed a confidence interval. As the footnote of [Table 12](https://www.fda.gov/media/144246/download#page=59) explains, here a [Clopper and Pearson confidence interval](https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#:~:text=The%20Clopper%E2%80%93Pearson%20interval%20is,distribution%20rather%20than%20an%20approximation) is used to adjust for surveillance time. In parts of the video that I have watched, the presenter avoided a misinterpretation despite the varying meaning of `CI` in the different slides. Not bad. 

How would Bayesian credible intervals look for the subgroup analysis? While I don't know how to implement surveillance time adjustment in a Bayesian framework, e.g. for the subgroup with age above 65 the surveillance times for the treatment and placebo group look very similar. There was 1 Covid-19 case in the treatment group and 19 Covid-19 cases in the control group. The stated 95% confidence interval for the vaccine efficacy is `(66.7, 99.9)`. Using the Bayesian approach implemented in the main analysis, we can compute the 95% credible interval as follows (see [here](http://skranz.github.io/r/2020/11/11/CovidVaccineBayesian.html) for details):


```r
# Assumed parameters of the prior Beta distribution for theta
# which measures the probability that a Covid-19 case was
# in the treatment group
a0 = 0.700102; b0 = 1
# 95% posterior credible interval for theta
theta.ci = qbeta(c(0.025,0.975),a0+1,b0+19)
# 95% credible interval for vaccine efficacy
VE.ci = rev((1-2*theta.ci)/(1-theta.ci))
round(VE.ci*100,1)
```

```
## [1] 71.9 99.2
```

We see that the Bayesian 95% credible interval `(71.9,99.2)` for the vaccine efficacy for subjects above 65 years is narrower and more optimistic about the lower efficacy bound than the confidence interval.
