---
layout: post
title: 'RTutor: CO2 Trading and Risk of Firm Relocation'
cover: 
date:   2016-11-03 15:00:00
categories: r
tags: [R,RTutor, shiny]
---


Many economists would agree that the most efficient way to fight global warming would be a world-wide tax or an emmission trading system for greenhouse gases. Yet, if only a part of the world implements such a scheme, a reasonable concern is that firms may decide to relocate to other parts of the world, causing job losses and less effective emmission reduction.

The European Union adressed this concern in its carbon emmission trading system by not auctioning off all emmission permits, but granting free emmission permits to facilities in economic sectors characterized by  high trade intensity or high carbon intensity. It is true that also freely allocated permits provide incentives to reduce carbon emmissions (opportunity costs are still equal to the price at which permits are traded). Yet, there are reasons, e.g. fiscal income, to limit the amount of freely given permits.

In their article ['Industry Compensation under Relocation Risk: A Firm-Level Analysis of the EU Emissions Trading Scheme'](https://www.aeaweb.org/articles?id=10.1257/aer.104.8.2482) (*American Economic Review*, 2014), Ralf Martin, Mirabelle Muûls, Laure B. de Preux and Ulrich J. Wagner study the most efficient way to allocate a fixed amount of free permits among facilities in order to minimize the risk of job losses or carbon leakage. Given their available data, they establish simple alternative allocation rules that can be expected to substantially outperform the current allocation rules used by the EU.

As part of his Master's Thesis at Ulm University, Benjamin Lux has generated a very nice RTutor problem set that allows you to replicate the insights of the paper in an interactive fashion. You learn about the data and institutional background, run explorative regressions and dig into the very well explained optimization procedures to find efficient allocation rules. At the same time you learn some R tricks, like effective usage of some dplyr functions.

Screenshoot:
<hr>
![Screenshot missing]((http://skranz.github.io/images/VS_Score.PNG)
<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to procceed. In addition you are challenged by very well designed quizzes.

To install the problem set the problem set locally, first install RTutor as explained here:

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

and then install the problem set package:

[https://github.com/b-lux/RTutorCarbonLeakage](https://github.com/b-lux/RTutorCarbonLeakage)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://b-lux.shinyapps.io/RTutorCarbonLeakage/](https://b-lux.shinyapps.io/RTutorCarbonLeakage/)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

