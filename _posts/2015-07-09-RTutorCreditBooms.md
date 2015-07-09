---
layout: post
title: 'RTutor: Credit Booms Gone Bust'
cover: 
date:   2015-07-09 15:00:00
categories: r
tags: [R,RTutor, shiny]
---


# RTutor: Credit Booms Gone Bust
2015-07-09 12:00:00  

Currently, quite a few students here at Ulm University create RTutor problem sets
based on economic articles as part of their Bachelor or Master thesis.
RTutor is an R package that allows to develop interactive R problem sets,
that can be solved in a browser based or markdown based environment.

Thomas Clausing has written a very nice problem set based on the article

[Credit Booms Gone Bust: Monetary Policy, Leverage Cycles, and Financial Crises, 1870 – 2008” by M. Schularick and A. M. Taylor, American Economic Review 2012](https://www.aeaweb.org/articles.php?doi=10.1257/aer.102.2.1029)

The original authors summarize their article as follows:

  - "The financial crisis has refocused attention on money and credit fluctuations, financial crises, and policy responses. We study the behavior of money, credit, and macroeconomic indicators over the long run based on a new historical dataset for 14 countries over the years 1870-2008. [...] Credit growth is a powerful predictor of financial crises, suggesting that policymakers ignore credit at their peril."

Thomas' problem set allows you investigate this new data set and the economic analysis in an interactive fashion. You can learn a bit about R and about methodologies like fixed effects regressions or the assessment of predictive power via ROC curves.

Here is screenshot:
<hr>

![Screenshot missing](http://skranz.github.io/images/RTutorCreditBooms.PNG)

<hr>

To install and try out the problem set locally, follow the instructions here:

[https://github.com/tcl89/creditboomsgonebust](https://github.com/tcl89/creditboomsgonebust)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it)

[https://tcl89.shinyapps.io/creditboomsgonebust](https://tcl89.shinyapps.io/creditboomsgonebust)

If you want to learn more about RTutor, to try out other problems, or to create a problem set yourself, take a look at the RTutor github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)
