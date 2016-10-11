---
layout: post
title: 'RTutor: The effect of air pollution on house prices'
cover: 
date:   2016-10-11 6:00:00
categories: r
tags: [R,RTutor, shiny]
---

A key question of environmental economics is to economically quantify damages from different types of pollution and benefits from environmental policy that attempts to reduce pollution. Often only indirect measures are available. An important measure is house prices, which should to some extend reflect negative externalities from local pollution.

In their article [Who Benefits from Environmental Regulation? Evidence from the Clean Air Act Amendments](http://www.mitpressjournals.org/doi/abs/10.1162/REST_a_00493#.V_xdZPl9603) (Review of Econonomics and Statistics, 2015) Antonio Bento, Matthew Freedman and Corey Long study a comprehensive regionally dissaggregated data set that contains data about air pollution (PM 10 concentration) in 1990 and 2000 for many monitoring stations around the United States. In addition they collected data on real estate prices, distance to the monitoring stations and demographic compositions of almost 2000 localities in the neighbourhood of these stations. They exploit the fact that the clear air act ammendents require active policy measures to reduce air pollution if measured pollution at the monitoring stations exceed some threshold, in order to estimate the effects of clean up measures on house prices. They find that especially houses in lowly priced localities have benefited from these clean up measures.

As part of his Master Thesis at Ulm University, Moritz Sporer has created an interactive RTutor problem set. It allows you to explore the data set and the studied questions in an interactive fashion and learn more about R at the same time. Even though there may be some rough edges, it allows you to dive nicely into the topic. For example, there is a very nice exposition of the non trivial issue of measuring causal effects and the differences between OLS and IV estimates in this study. 

Screenshoot:
<hr>
![Screenshot missing](http://skranz.github.io/images/AirPollutionOLS.PNG)
<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to procceed. 

To install the problem set the problem set locally, first install RTutor as explained here:
[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

and then install the problem set package:

[https://github.com/msporer/RTutorEnvironmentalRegulation](https://github.com/msporer/RTutorEnvironmentalRegulation)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://msporer.shinyapps.io/RTutorEnvironmentalRegulations/](https://msporer.shinyapps.io/RTutorEnvironmentalRegulations/)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

