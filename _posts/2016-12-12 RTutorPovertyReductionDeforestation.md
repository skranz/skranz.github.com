---
layout: post
title: 'RTutor: Poverty Reduction and Deforestation'
cover: 
date:   2016-12-12 8:00:00
categories: r
tags: [R,RTutor, shiny]
---


The environmental Kuznets curve hypothesizes an inverted u-shape relationship between income and environmental degradation: while richer countries tend to invest more ressources into environmental protection as their income rises, income growth in poor countries initially goes hand in hand with stronger degradation of natural ressources. But the environmental Kuznets curve is nothing like an universal natural law, and it is not clear whether, by how much or by which channels certain forms of poverty relief may affect environmental quality. 

In their article ['The ecological footprint of poverty alleviation: evidence from Mexico's Oportunidades program'](http://www.mitpressjournals.org/doi/abs/10.1162/REST_a_00349?journalCode=rest#.WErSnLIrL3g) (*Review of Economics and Statistics*, 2013), Jennifer Alix-Garcia, Jennifer, Craig McIntosh, Katharine RE Sims, and Jarrod R. Welch study the effect of a Mexican poverty relief program on the rate of deforestation in treated communities.

As part of her Master's Thesis at Ulm University, Katharina Kaufmann has generated a very well done RTutor problem set  that allows you to replicate the insights of the paper in an interactive fashion. You will get to know the economic findings along with the according analytic steps, as well as explanations of the economic theory behind it and useful R commands in this context - all in an interactive way.

The outline is as follows:

**1. Introduction**
The first exercise gives background information about the “Oportunidades”“ program and deforestation and makes you familiar with the first data set.

**2. Treatment effect**
The central question in the second exercise is: "Is there a correlation or causal effect between the participation in the program and deforestation?”. A *Tobit model* and the concept of the used *Regression Discontinuity* design are introduced here.

**3. Household level explanation & Role of infrastructure quantity**
The third exercise deals with a possible explanation of the findings in Exercise 2 studying the changes in consumption and production behavior of households and makes use of a second data set. Here, a *difference-in-difference estimator* is used.
As soon as treated localities are linked with other localities through roads, increases in consumption can be sourced from neighboring localities and lead to environmental impacts there. The exercise thus also studies the extent to which the effect is mediated and draws conclusions about the measurability of the treatment effect.

**4. Summary and conclusion:**
The last exercise will sum up the results, draw conclusions from them and give an outlook on what could be studied in the future.

(I must admit, that in this blog I don't adhere to standards of scientific writing and have simply copied large parts of the description above from the outline of the problemset.)

Here is screenshoot:
![Screenshot missing]((http://skranz.github.io/images/Mexico.PNG)
<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to procceed. In addition you are challenged by very well designed quizzes.

To install the problem set the problem set locally, first install RTutor as explained here:
[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

and then install the problem set package:

[https://github.com/KathKaufmann/RTutorEcologicalFootprintOfPovertyAlleviation](https://github.com/KathKaufmann/RTutorEcologicalFootprintOfPovertyAlleviation)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://kathkaufmann.shinyapps.io/RTutorEcologicalFootprintOfPovertyAlleviation/](https://kathkaufmann.shinyapps.io/RTutorEcologicalFootprintOfPovertyAlleviation/)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

