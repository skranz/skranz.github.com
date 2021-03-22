---
layout: post
title: "Detailed Current Data on the Economic Impact of Covid-19"
cover: 
date:   2021-03-22 14:20:00
categories: r
tags: [R, economics]
---

I was very impressed when watching Raj Chetty's [video presentation](https://youtu.be/UmgBeN5Ohn0?t=109) in the AEA panel "The Economic Impact and Policy Response to the Pandemic: What Happened in 2020 and What is Ahead?". He is part of a group (Chetty, Friedman, Hendren, Stepner, and the OI Team) that convinced several private sector companies to make publicly available important data that allows to track in detail the economic impact of Covid-19 in the US with a much smaller time-lag than official statistics. 

All data is publicly available on [this Github repository](https://github.com/OpportunityInsights/EconomicTracker) and it can be explored interactively on [tracktherecovery.org](https://tracktherecovery.org/). Here is a graph of the change in employment for different income groups:

<center>
<img src="http://skranz.github.io/images/covid/employment_change.png" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

You see how many more low wages jobs were lost than high wage jobs. Such numbers are of course very important when designing Covid relief packages.

In contrast, small businesses located high income ZIP codes lost more revenues:

<center>
<img src="http://skranz.github.io/images/covid/small_business_revenues.png" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

One reason could be a higher propensity of richer households to use online shopping. One can also explore the revenue impacts on [a map](https://opportunityinsights.org/small-biz-revenue-zip-map/):

<center>
<img src="http://skranz.github.io/images/covid/map_small_business_ny.PNG" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

The map may suggest another reason: high income areas in cities are often areas with many offices whose employees largely work from home during the crisis. Again, I encourage you to take a look at the [video presentation](https://youtu.be/UmgBeN5Ohn0?t=109) for Raj Chetty's interpretation, insights and recommendations based on the collected data.

Normally, I would now add a small analysis in R with the data set. However, since analyzing that data is a topic in my current empirical economics seminar and another student is going to write an [interactive RTutor](https://github.com/skranz/RTutor) Master thesis about the [working paper](https://opportunityinsights.org/wp-content/uploads/2020/05/tracker_paper.pdf), I will abstain from it.
