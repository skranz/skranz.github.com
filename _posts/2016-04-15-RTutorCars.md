---
layout: post
title: 'RTutor: Technological Progress and Fuel Economy of Cars'
cover: 
date: 2016-04-15 12:00:00
categories: r
tags: [R,RTutor, shiny]
---
 
Christopher Knittel estimates in his article

[Automobiles on Steroids: Product Attribute Trade-Offs and Technological Progress in the Automobile Sector (American Economic Review, 2011)](https://www.aeaweb.org/articles?id=10.1257/aer.101.7.3368)

the technological progress that has occurred since 1980 in the automobile industry and the trade-offs faced when choosing between fuel economy, weight, and engine power characteristics. The estimation is performed without using any cost data or engineering models but it uses only observed characteristics from a large sample of car models.

As part of his Bachelor thesis at Ulm University, Marius Breitmayer has created an interactive RTutor problem set that allows you to replicate large parts of the analysis in R. The results show how considerable progress has taken place but has only to a small part been directed into a better fuel economy.
Like previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to procceed. (Note that while there are many very nice parts in the problem set, in some parts the econometric exposition may not always be as clear as it could be.)

Here is a screenshoot:
<hr>
![Screenshot missing](http://skranz.github.io/images/cars.PNG)
<hr>

To install the problem set locally, follow the instructions here:

[https://github.com/MariusBreitmayer/RTutorAttributeTradeOffs](https://github.com/MariusBreitmayer/RTutorAttributeTradeOffs)

There is also an online version hosted by shinyapps.io that allows you explore the problem set without any local installation. (The online version is capped at 30 hours total usage time per month. So it may be greyed out when you click at it.)

[https://mariusbreitmayer.shinyapps.io/RTutorAttributeTradeOffs](https://mariusbreitmayer.shinyapps.io/RTutorAttributeTradeOffs)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

Note:
The name `RTutor` has become quite attractive, which may lead to some confusion. So here is a short list of projects that share the basic name but vary on its spelling:

- I guess first was the website with information about R [http://www.r-tutor.com/](http://www.r-tutor.com/).
- Then I put my package [RTutor](https://github.com/skranz/RTutor) on Github and regularly announce new problem sets on R-bloggers.
- Recently, the package [RtutoR](https://cran.r-project.org/web/packages/RtutoR/index.html) has been put on CRAN and also been announced on R-bloggers.

All are very nice projects, but rather unrelated.
