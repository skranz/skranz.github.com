---
layout: post
title: 'RTutor: Public Procurement Auctions: Design, Outcomes and Adaption Costs'
cover: 
date:   2015-12-18 14:00:00
categories: r
tags: [R,RTutor, shiny]
---
 

As an economist, I consider public procurement of construction projects a very interesting topic. While several firms may initially bid for a project and propose prices, the news in Germany are regulary filled with construction projects whose costs went far beyond those that were initially agreed upon. For example, Wikipedia states that when the city of Hamburg initially signed the contracts for its new concert house, the "Elbphilharmonie", payments of 114 Million Euro where agreed upon. In 2013, Hamburg's mayor stated that expected payments are already 789 Millionen Euro. Given that requirements for the construction are quite often adjusted after contracts are signed, the design of an intelligent mechanism for procurement and ex-post compensation is by no means trivial. 

Frederik Collin has created a very nice interactive RTutor problem set that allows you to explore the design and outcomes of procurement auctions for constructions and repairs of Californian highways. It was created as part of his Master thesis at Ulm University and is based on the article 

["Bidding for Incomplete Contracts: An Empirical Analysis of Adaption Costs", by Patrick Bajari, Stephanie Houghton and Steven Tadelis, American Economic Review, 2014 ](https://www.aeaweb.org/articles.php?doi=10.1257/aer.104.4.1288)

The article and problem set is based on an awesome data set that contains engineer estimates, detailed itemized bids, background data on bidders, and information of ex-post adjustments and compensations of all Californian highway procurement auctions for several years. 

In the interactive problem set, you first learn, with a selected example auction, details about the auction design that uses engineer estimates and itemized bids. You then examine bidding behavior, e.g. how markups across auctions depend on firm characteristics and competitive pressure. You also study in how far firms systematically `skew` their bids to exploit systematic mistakes in official engineer estimates. Finally, the data on ex-post changes and negotiated compensations is studied. It is estimated that, possibly due to high transaction and haggling costs, firms anticipate ex-post adaptions to be  considerably more costly as what they are compensated for. 


The problem set incorporates some novel RTutor features, such as quizes and output of data frames as html tables by default...

Screenshoot:
<hr>
![Screenshot missing](http://skranz.github.io/images/ProcurementQuiz.PNG)
<hr>


Like previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to procceed. 

Screenshoot:
<hr>
![Screenshot missing](http://skranz.github.io/images/ProcurementHints.PNG)
<hr>

To install the problem set the problem set locally, follow the instructions here:

[https://github.com/Fcolli/RTutorProcurementAuction](https://github.com/Fcolli/RTutorProcurementAuction)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://fcolli.shinyapps.io/RTutorProcurementAuction](https://fcolli.shinyapps.io/RTutorProcurementAuction)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)
