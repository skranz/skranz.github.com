---
layout: post
title: "RTutor: How do competition policy and industrial policy affect economic development?"
cover: 
date: 2020-10-08 10:00:00
categories: r
tags: [R, RTutor, shiny]
---

Competition authorities regularly decide (and often defend in legal courts) whether to allow or forbid a merger, what fines to impose for illegal collusion, whether to restrict certain business practices of firms that have achieved a dominant position, and particularly in the EU, whether certain forms of state aid distort competition in the common market. Sometimes contrasting the efforts of such [competition policy](https://ec.europa.eu/competition/consumers/what_en.html) is active [industrial policy](https://en.wikipedia.org/wiki/Industrial_policy) where countries try to promote particular sectors or [national champions](https://en.wikipedia.org/wiki/National_champions), e.g. via infrastructure investments, direct subsidies or even by hampering market entry of foreign competitiors.

An important but tricky empirical question is how stricter competition policy or certain forms of industrial policy causally affect a country's economic development.

As part of his Master Thesis at Ulm University, Julia Latzko has created a very nice [RTutor](https://github.com/skranz/RTutor) problem set that allows you to replicate two great empirical studies on this topic in an interactive fashion. Like in previous RTutor problem sets, you can enter free R code in a web-based shiny app. The code will be automatically checked and you can get hints how to proceed. 

You can test the problem set online on shinyapps.io

[https://julianlatzko.shinyapps.io/RTutorCompetitionPolicy/](https://julianlatzko.shinyapps.io/RTutorCompetitionPolicy/)

or locally install the problem set, by following the installation guide at the problem set's Github repository:

[https://github.com/julianlatzko/RTutorCompetitionPolicy](https://github.com/julianlatzko/RTutorCompetitionPolicy)

The problem set first explores the article [Competition policy and productivity growth: An empirical assessment](https://www.mitpressjournals.org/doi/10.1162/REST_a_00304) by Buccirossi et. al. (2013). The authors have generated a panel data set that allows to compare several indicators for the toughness of competition policy (e.g. the legal basis or the enforcement resources) across 12 OECD countries from 1995 to 2005. The article uses it to study the impact of a tougher competition policy on economic development measured by the growth in total factor productivity. The problem set very nicely explains the data set with examples and the causal estimation strategy and issues that arise.  

Here is a screenshot of one of the plots you generate in the problem set:

<center>
<img src="http://skranz.github.io/images/competition_cpi.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

Afterwards the problem set explores the article [Industrial policy and competition](https://www.aeaweb.org/articles?id=10.1257/mac.20120103) by Aghion et. al. (2015). It uses a large Chinese firm level panel data set to study the interaction effects of industrial policy and degree of competition in a sector. The article finds that sector wide industrial policy, e.g. reduced interest rate loans for all firms in a particular sector, can spur productivity growth if it is allocated to sectors that already have a high degree of competition. Industrial policy also increases total factor productivity if it strengthens competition in a sector, e.g by making market entry more attractive. The problem set allows you to interactively work through the data, the empirical strategy and to replicate the main results.


If you want to learn more about RTutor, try out other problem sets, or create a problem set yourself, take a look at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

or at the documentation

[https://skranz.github.io/RTutor](https://skranz.github.io/RTutor)

### References:

Buccirossi, Paolo, Lorenzo Ciari, Tomaso Duso, Giancarlo Spagnolo, and Cristiana Vitale. "Competition policy and productivity growth: An empirical assessment." Review of Economics and Statistics 95, no. 4 (2013): 1324-1336.

Aghion, Philippe, Jing Cai, Mathias Dewatripont, Luosha Du, Ann Harrison, and Patrick Legros. "Industrial policy and competition." American Economic Journal: Macroeconomics 7, no. 4 (2015): 1-32.
