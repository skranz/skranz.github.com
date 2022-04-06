---
layout: post
title: "Finding Economic Articles with Data: Now with URL Parameters"
cover: 
date: 2022-04-06 17:40:00
categories: r
tags: [R]
---

If you want to find reproducible empirical economic studies that are about some specific topic or [that use some specific method](http://skranz.github.io/r/2021/01/05/FindingEconomicArticles4.html), you can take a look at my shiny-powered app "Find Economic Articles with Data":

[https://ejd.econ.mathematik.uni-ulm.de](https://ejd.econ.mathematik.uni-ulm.de)

I just included a small new feature: one can specify search terms as an URL parameter. For example, assume you are interested in new reproducible articles on the topics `labor` and `climate change`. You could then bookmark this URL:

[https://ejd.econ.mathematik.uni-ulm.de?search=labor,"climate change"&sortby=date](https://ejd.econ.mathematik.uni-ulm.de?search=labor,"climate change"&sortby=date)

Every time you open the app with this URL, you find the articles that match these topics ordered from newest to oldest. Of course, you can adapt the search terms in the URL as you like.

If you leave out the argument `sortby=date`, articles are ordered by how often the search terms appear in title and abstract:

[https://ejd.econ.mathematik.uni-ulm.de?search=labor,"climate change"](https://ejd.econ.mathematik.uni-ulm.de?search=labor,"climate change")

That's all. I hope the app and the new feature is useful.