---
layout: post
title: 'RTutor: How would Carbon Pricing Affect US Electricity Production?'
cover: 
date: 2020-05-25 9:00:00
categories: r
tags: [R, RTutor, shiny]
---  

One way by which carbon pricing can reduce CO2 emmissions is by shifting electricity production from coal power plants to natural gas plants. A combined cycle gas turbine emits roughly only half as much CO2 per MWh produced electricity than a coal power plant. (Unfortunately, a substantial part of those benefits may be negated unless methane leakage in natural gas production is sufficiently curbed, see e.g. [here](https://www.scientificamerican.com/article/methane-leaks-erase-some-of-the-climate-benefits-of-natural-gas/).)  

But predicting how exactly carbon pricing would shift US electricity production is not straightfoward, since the usage of coal and gas plants depends on a lot of factors like e.g. congestion of electricity networks.

In their article [Inferring Carbon Abatement Costs in Electricity Markets: A Revealed Preference Approach Using the Shale Revolution](https://www.aeaweb.org/articles?id=10.1257/pol.20150388) (AEJ Economic Policy, 2017) Joseph Cullen and Erin Mansur use the large variation in historical natural gas prices as part of the shale gas revolution to predict the effects of carbon pricing.

As part of his Bachelor Thesis at Ulm University, Daniel Dreyer has generated a nice RTutor problem set that allows you to replicate the findings in an interactive fashion. You will learn in detail the ideas and concrete steps necessary to come up with predictions like in this ggplot graph:


<img src="http://skranz.github.io/images/carbon_reduction.png" style="max-width: 100%;">

<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to proceed. In addition you are challenged by many multiple choice quizzes.

You can test the problem set online on shinyapps.io: [https://danieldreyer.shinyapps.io/RTutorCO2ReductionCosts/](https://danieldreyer.shinyapps.io/RTutorCO2ReductionCosts/)

The free shinyapps.io account is capped at 25 hours total usage time per month. So it may be greyed out when you click at it.

To locally install the problem set, follow the installation guide at the problem set's Github repository: [https://github.com/danieldreyer/RTutorCO2ReductionCosts](https://github.com/danieldreyer/RTutorCO2ReductionCosts)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at 

[https://skranz.github.io/RTutor/](https://skranz.github.io/RTutor/)

or at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

