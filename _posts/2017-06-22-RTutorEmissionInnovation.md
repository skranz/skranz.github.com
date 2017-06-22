---
layout: post
title: 'RTutor: Emission Certificates and Green Innovation'
cover: 
date: 2017-06-22 21:00:00
categories: r
tags: [R,RTutor, shiny]
---

Which policy instruments should we use to cost-effectively reduce greenhouse gas emissions? For a given technological level there are many economic arguments in favour of tradeable emission certificates or a carbon tax: they generate static efficiency by inducing emission reductions in those sectors and for those technologies where it is most cost effective.

Specialized subsidies, like the originally extremely high subsidies on solar energy in Germany and other countries are often much more costly. Yet, we have seen a tremendous cost reduction for photovoltaics, which may have not been achieved on such a scale without those subsidies. And maybe in a world, where the current president of a major polluting country seems not to care much about the risks of climate change, the development of cheap green technology that even absent goverment support can cost-effectively substitute fossil fuels, is the most decisive factor to fight climate change.

Yet, the impact of different policy measures on innovation of green technology is very hard to assess. Are focused subsidies or mandates the best way, or can also emission trading or carbon taxes considerably boost innovation of green technologies? That is a tough quantitative question, but we can try to get at least some evidence.

In their article  [Environmental Policy and Directed Technological Change: Evidence from the European carbon market](http://www.mitpressjournals.org/doi/abs/10.1162/REST_a_00470), Review of Economic and Statistics (2016), Raphael Calel and Antoine Dechezlepretre study the impact of the EU carbon emission trading system on patent activities of the regulated firms. By matching them with unregulated firms, they estimate that the emission trading has increased the innovation activities for low carbon technologies of the regulated firms by 10%.

As part of his Master Thesis at Ulm University, Arthur Schäfer has generated an RTutor problem set  that allows you to replicate the main insights of the paper in an interactive fashion.

Here is screenshoot:

<img src="http://skranz.github.io/images/greenpatents.PNG" style="width: 70%; height: 70%">

<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to procceed. In addition you are challenged by many multiple choice quizzes.

To install the problem set the problem set locally, first install RTutor as explained here:

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

and then install the problem set package:

[https://github.com/ArthurS90/RTutorEmissionTrading](https://github.com/ArthurS90/RTutorEmissionTrading)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://arthurs90.shinyapps.io/RTutorEmissionTrading/](https://arthurs90.shinyapps.io/RTutorEmissionTrading/)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

