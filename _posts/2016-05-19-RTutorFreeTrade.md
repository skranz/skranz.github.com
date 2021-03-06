---
layout: post
title: 'RTutor: Free Trade Agreements'
cover: 
date: 2016-05-19 08:00:00
categories: r
tags: [R,RTutor, shiny]
---
 


There is quite some sceptisim towards TTIP, the currently negotiated free trade agreement between Europe and the USA. Even in an exporting nation like Germany, there are substantial worries about reduction in consumer protection standards, or fear of limits to democracy if cooperations can sue states in international settlement courts. On the other hand, most economists tend to be positive towards free trade in general and stress the benefits from more effective exchange of goods and services. But how large are these possible beneficial impacts of a free trade agreement and how can we estimate them?  

Tobias Fischer has created a very nice interactive RTutor problem set concerning these issues. You will first be introduced to a nice international trade data set. Besides tarif and trade data, it contains sector specific input-output tables, which allow to account for intermediate production. The problem set then presents a modern Ricardian trade model and discusses how it is calibrated. The model is used to predict the welfare effects of the tariff reductions from the North American Free Trade Agreement NAFTA.

While TTIP is not quantitativeley assessed in the problem set, you learn about the methods and data that can be used for such an assessment, and about their limitations. Not surprisingly, it is very hard to predict effects of free trade agreements with any reasonable precision, since a lot of subjective choices must be made when setting up and calibrating such a model. These issues are very well discussed in the problem set.

The problem set is mainly based on the following article:

[Caliendo and Parro (2015): "Estimates of the Trade and Welfare Effects of NAFTA", Review of Economic Studies](http://restud.oxfordjournals.org/content/82/1/1.abstract)

I also like the didactical design of the problem set, which very actively uses quizes.

Screenshoot:
<hr>
![Screenshot missing](http://skranz.github.io/images/FreeTrade.PNG)
<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to procceed. To install the problem set the problem set locally, follow the instructions here:

[https://github.com/fischeruu/RTutorNAFTAfreetrade](https://github.com/fischeruu/RTutorNAFTAfreetrade)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://fischeruu.shinyapps.io/RTutorNAFTAfreetrade/](https://fischeruu.shinyapps.io/RTutorNAFTAfreetrade/)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

We are currently testing the web-based version of RTutor for an introductionary stats course at Ulm University hosted on our own server. So far it seems to work quite well. Later this summer, I hope to find some time to write up a vignette that explains how to securely (using RAppArmor) host RTutor problem sets for your own courses on your own server.  
