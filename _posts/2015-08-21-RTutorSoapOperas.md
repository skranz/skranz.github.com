---
layout: post
title: 'RTutor: How Soap Operas Reduced Fertility in Brazil'
cover: 
date:   2015-08-21 15:00:00
categories: r
tags: [R,RTutor, shiny]
---

What is the real world impact of tv series? Did Brazilian women get fewer children because they watched soap operas that portray happy, rich families that have few children?

Clara Ulmer has written a very nice RTutor problem set that allows you interactively explore this question in R. It is based on the paper 

[Soap Operas and Fertility: Evidence from Brazil by Eliana La Ferrara, Alberto Chong and Suzanne Duryea, American Economic Journal: Applied Economics (2012)](https://www.aeaweb.org/articles.php?doi=10.1257/app.4.4.1)

While correlations between two time series can be quickly found, the challenge in many econometric studies is to convincingly establish causal effects. The main analysis is based on a micro data panel data set that contains births and background characteristics for Brazilian woman over several years. The analysis exploits the fact that the relevant TV broadcast company entered at different points of time in different regions.

The regression analysis starts with Exercise 4. Subsequently you can explore  many robustness checks (e.g. placebo regressions), that make actually a quite convincing case for the presence of a causal effect of soap operas on fertility.

The panel data analysis in the problem set extensively uses the R package [lfe](https://cran.r-project.org/web/packages/lfe/index.html). In my view `lfe` is quite a nice package, that allows to quickly run multiway fixed effect regressions on considerably large data sets. In addition, many functionality that empirical economists commonly use, like instrumental variables or clustered robust standard errors can be very conveniently accessed. For economists that currently use Stata but are thinking of moving to R, the `lfe` package may be removing some considerable obstacles. You can check it out in Clara's problem set. 

Here is a screenshot:
<hr>

![Screenshot missing](http://skranz.github.io/images/RTutorSoapOperas.PNG)

<hr>
To install and try out the problem set locally, follow the instructions here:

[https://github.com/ClaraUlmer/RTutorSoapOperas](https://github.com/ClaraUlmer/RTutorSoapOperas)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://claraulmer.shinyapps.io/RTutorSoapOperas](https://claraulmer.shinyapps.io/RTutorSoapOperas)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)