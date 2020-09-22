---
layout: post
title: "RTutor: The Causal Effects of Sweden's CO2 Tax"
cover: 
date: 2020-09-22 10:00:00
categories: r
tags: [R, RTutor, shiny]
---

Most economists agree that putting a price on greenhouse gas emmissions either in form of emission certificates or a carbon tax should be the main instrument to combat climate change in a cost efficient manner. Sweden has introduced a steadily increasing Carbon tax in 1991 which reached in 2020 a level of 110 Euro per ton CO2, which makes it the highest carbon tax in the world. The tax applies to those sectors that are not covered by the EU emission trading system (mainly transport and residential heating).

Two important questions related to such a tax are:

1. How effective was the carbon tax in reducing Sweden's carbon emissions?

2. How did the tax affect Sweden's economic development measured by GDP growth?

One can get a first idea by looking at the development over time for these two variables. The Swedish government made the following plot:

<center>
<a href="https://www.government.se/carbontax"><img src="https://www.government.se/globalassets/government/bilder/finansdepartementet/carbon-taxes/ny-gdp-development-and-ghg-emissions.png" style="max-width: 70%; padding-bottom: 1em"></a>
</center>

However, obviously we don't know how those curves would have looked if Sweden would not have introduced the carbon tax. This means just looking at time trends does typically not show us the causal effect of a policy intervention.

The article [Carbon Taxes an CO2 Emissions: Sweden as a case study](https://www.aeaweb.org/articles?id=10.1257/pol.20170144) (2019, AEJ: Economic Policy) by Julius J. Andersson estimates the causal effect of Sweden's CO2 tax on emissions in the transport sector using the [synthetic control model](https://en.wikipedia.org/wiki/Synthetic_control_method).

The key idea is to take as control group a *synthetic Sweden* constructed as a weighted sample of other countries. The country weights are determined in a nested optimization procedure that gives higher weights to countries that are in the pre-intervention period closer to Sweden with respect to certain explanatory variables, like GDP per capita or the share of urban population. Those explanatory variables are weighted such that the constructed synthetic Sweden well matches Sweden's time path of the pre-intervention emission levels.

As part of her Master Thesis at Ulm University, Theresa Graefe has created a very nice [RTutor](https://github.com/skranz/RTutor) problem set that allows you to replicate the analysis and dig deeper into the synthetic control method in an interactive fashion with R. Like in previous RTutor problem sets, you can enter free R code in a web-based shiny app. The code will be automatically checked and you can get hints how to proceed. In addition you are challenged by multiple choice quizzes. This means you will be guided how to generate plots like the following that illustrates the time path of the estimated causal effects as the post-treatment difference between Sweden's and synthetic Sweden's CO2 emmissions:

<center>
<img src="http://skranz.github.io/images/sweden_co2_synth.svg" style="max-width: 100%;">
</center>

and you will see in similar plots that there was essential no measurable causal effect of the CO2 tax on Sweden's GDP growth. You will also learn about Placebo tests that help to assess statistic significance (often in an informal manner) of the estimated causal effects.

You can test the problem set online on shinyapps.io:

[https://theresagraefe.shinyapps.io/RTutorCarbonTaxesAndCO2Emissions/](https://theresagraefe.shinyapps.io/RTutorCarbonTaxesAndCO2Emissions/)

The free shinyapps.io account is capped at 25 hours total usage time per month. So it may be greyed out when you click at it.

To locally install the problem set, follow the installation guide at the problem set's Github repository: [https://github.com/TheresaGraefe/RTutorCarbonTax](https://github.com/TheresaGraefe/RTutorCarbonTax)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

or the documentation at

[https://skranz.github.io/RTutor/](https://skranz.github.io/RTutor/)
