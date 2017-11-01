---
layout: post
title: 'RTutor: Wall Street and the Housing Bubble'
cover: 
date: 2017-11-01 9:00:00
categories: r
tags: [R, RTutor, shiny]
---  

It is widely recognized that aggressive securitization of housing loans played a decisive role in the creating of a house price bubble in the US and the subsequent financial crisis. An open question is in how far financial experts on securitization were aware of that fragile house price bubble they helped creating.


Ing-Haw Cheng, Sahil Raina, and Wei Xiong shed some light on this question in their very interesting article *Wall Street and the Housing Bubble* (American Economic Review, 2014). They have collected a unique data set on private real estate transactions for a sample of 400 securitization agents, and two control groups consisting of laywers and financial analysts not working in the real estate sector. They use this data set to study whether or not in their private investments the securitization agents seem to have been aware of the bubble and where more successful in the timing of their real estate investments. 


As part of his Bachelor Thesis at Ulm University, Marius Wentz has generated a RTutor problem set that allows you to replicate the main insights of the article in an interactive fashion. You learn about R, econometrics and the housing bubble at the same time.

Here is a screenshoot:

<img src="http://skranz.github.io/images/WallStreet.PNG" style="width: 70%; height: 70%">

<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to proceed. In addition you are challenged by many multiple choice quizzes.

To install the problem set the problem set locally, first install RTutor as explained here:

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

and then install the problem set package:

[https://github.com/mwentz93/RTutorWallStreet](https://github.com/mwentz93/RTutorWallStreet)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 25 hours total usage time per month. So it may be greyed out when you click at it.)

[https://mwentz93.shinyapps.io/RTutorWallStreet/](https://mwentz93.shinyapps.io/RTutorWallStreet/)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

You can also install RTutor as a docker container:
[https://hub.docker.com/r/skranz/rtutor/](https://hub.docker.com/r/skranz/rtutor/)

