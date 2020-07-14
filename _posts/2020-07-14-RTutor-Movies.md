---
layout: post
title: 'RTutor: Quantifying Social Spillovers in Movie Ticket Sales'
cover: 
date:   2020-07-14 9:00:00
categories: r
tags: [R, RTutor, shiny]
---  

Probably many of us would be more inclined to watch a particular movie in cinema if some friends or colleagues have already seen it and talk about it (at least if it is a decent movie). 

Duncan Sheppard Gilchrist and Emily Glassberg Sands quantify such social spillover effects in their great article [Something to Talk About: Social Spillovers
in Movie Consumption](https://www.journals.uchicago.edu/doi/abs/10.1086/688177) (2016, Journal of Political Economy). The key idea is to use random weather fluctuations during a movie's premiere to causally identify the social spillover effects.

Consider the following simplified causal model of factors that determine the number of views during a movie's premiere:

<img src="http://skranz.github.io/images/movie_spillovers.svg" style="max-width: 100%;">

We want to instrument the number of views at a movie's premiere with weather characteristics that are uncorrelated with movie characteristics. There are two catches however: First, the premiere date is likely correlated with both the weather and movie characteristics (e.g. commercially very promising movies may be timed to premiere around Christmas). Second, since there are a huge amount of possible relevant weather conditions, the authors use a LASSO approach to select relevant weather variables, i.e. we cannot just run a standard IV regression with date variables as exogenous controls.

I think it is a very nice econometric quiz to determine how one can causally estimate the social spillover effects using only a sequence of basic OLS regressions and one LASSO regression. (This paper was also one of the motivators for writing my [previous blog post on regression anatomy](https://skranz.github.io/r/2020/07/01/PuzzlingRegressionAnatomy.html).) 

As part of her Bachelor Thesis at Ulm University, Lara Santak has created a very nice RTutor problem set that allows you to replicate the analysis in an interactive fashion in R. Like in previous RTutor problem sets, you can enter free R code in a web-based shiny app. The code will be automatically checked and you can get hints how to proceed. In addition you are challenged by multiple choice quizzes. While the causal identification strategy is explained more stringent in the original paper, the RTutor problem set nicely allows you to dive yourself into the very interesting data set. 

You can test the problem set online on shinyapps.io:

[https://lara-santak.shinyapps.io/RTutorSomethingToTalkAbout](https://lara-santak.shinyapps.io/RTutorSomethingToTalkAbout)

The free shinyapps.io account is capped at 25 hours total usage time per month. So it may be greyed out when you click at it.

To locally install the problem set, follow the installation guide at the problem set's Github repository: [https://github.com/larasantak/RTutorSomethingToTalkAbout](https://github.com/larasantak/RTutorSomethingToTalkAbout)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at 

[https://skranz.github.io/RTutor/](https://skranz.github.io/RTutor/)

or at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

