---
layout: post
title: 'RTutor: How much less effective are publicly assigned lawyers?'
cover: 
date: 2020-07-06 12:00:00
categories: r
tags: [R, RTutor, shiny]
---  

So far, my main source of information about the US justice system have been tv shows like *Suits*, or *The Good Wife*. They suggest that hiring yourself a good lawyer can be quite expensive but may have a big impact on the verdict. But that are tv shows. What can we infer from real world data?

Amanda Agan, Matthew Freedman and Emily Owens study this question in their very interesting article [Is Your Lawyer a Lemon? Incentives and Selection in the Public Provision of Criminal Defense](https://www.mitpressjournals.org/doi/abs/10.1162/rest_a_00891?mobileUi=0) (RESTAT, 2019). They collected a comprehensive data set of criminal cases from Bexar County, Texas with many background variables on case characteristics and attorneys.

Defendants either privately hired their own attorneys or the state assigned and paid a private sector attorney for them. In the descriptive statistics we find that defendants with assigned attorneys are roughly 50 percent (18 percentage points) more likely to be convicted as guilty.

Of course there can be different reasons for this difference:

1. Case characteristics can differ between cases with privately hired and publicly assigned attorneys.

2. Adverse selection: Assigned attorneys may be less competent on average than privately hired ones.

3. Mismatch: The match between assigned attorneys and their clients may be worse and therefore yield less successful outcomes.

4. Moral hazard: E.g. due to a different fee structure assigned attorneys may put less effort or time into a case than if they would be hired privately.

Using the large number of background variables and the fact that many lawyers work both as assigned and privately hired attorneys, the authors are able to distinguish the different reasons to a large extend.

As part of his Master Thesis at Ulm University, Artemij Cadov has generated a very nice RTutor problem set that allows you to replicate this fascinating analysis in an interactive fashion in R. 

Here is a screenshot:

<img src="http://skranz.github.io/images/lawyers.PNG" style="max-width: 100%;">

<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to proceed. In addition you are challenged by many multiple choice quizzes.

You can test the problem set online on shinyapps.io: [https://kendamaqq.shinyapps.io/RTutorLawyers/](https://kendamaqq.shinyapps.io/RTutorLawyers/)

The free shinyapps.io account is capped at 25 hours total usage time per month. So it may be greyed out when you click at it.

To locally install the problem set, follow the installation guide at the problem set's Github repository: [https://github.com/KendamaQQ/LawyersLemon](https://github.com/KendamaQQ/LawyersLemon)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at 

[https://skranz.github.io/RTutor/](https://skranz.github.io/RTutor/)

or at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

