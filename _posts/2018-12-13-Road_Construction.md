---
layout: post
title: 'RTutor: Better Incentive Contracts For Road Construction'
cover: 
date:   2018-12-13 8:00:00
categories: r
tags: [R, RTutor, shiny]
---

Since about two weeks, I face a large additional traffic jam every morning due to a construction site on the road. When passing the construction site, often only few people or sometimes nobody seems to be working there. Being an economist, I really wonder how much of such traffic jams could be avoided with better contracts that give the construction company proper incentives to account for the social cost of the road blocking and therefore more often fully staff the constructing site and finish earlier.

Patrick Bajari and Gregory Lewis have collected a detailed sample of 466 road construction projects in Minnesota to study this question in their very interesting article <a href="https://academic.oup.com/restud/article/81/3/1201/1602080" target="_blank"> Moral Hazard, Incentive Contracts and Risk: Evidence from Procurement</a> in the Review of Economic Studies, 2014.

They estimate a structural econometric model and find that changes in contract design could substantially reduce the duration of road blockages and largely increase total welfare at only minor increases in the risk that road construction firms face. 

As part of his Master Thesis at Ulm University, Claudius Schmid has generated a nice and detailed RTutor problem set that allows you to replicate the findings in an interactive fashion. You learn a lot about the structure and outcomes of the currently used contracts, the theory behind better contract design and how the structural model to assess the quantitative effects can be estimated and simulated. At the same time, you can hone your general data science and R skills.

Here is a small screenshot:

<img src="http://skranz.github.io/images/road_construction.png" style="width: 100%; height: 100%">

<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to proceed. In addition you are challenged by many multiple choice quizzes.


You can test the problem set online on the rstudio.cloud: [https://rstudio.cloud/project/137023](https://rstudio.cloud/project/137023) Source the `run.R` file to start the problem set.


If you don't want to register at rstudio cloud, you can also check out the problem on shinyapps.io: [https://clamasch.shinyapps.io/RTutorIncentiveContracts](https://clamasch.shinyapps.io/RTutorIncentiveContracts)

The free shinyapps.io account is capped at 25 hours total usage time per month. So it may be greyed out when you click at it. Also, unlike on rstudio.cloud, you cannot save your progress on shinyapps.io.

To locally install the problem set, follow the installation guide at the problem set's Github repository: [https://github.com/ClaMaSch/RTutorIncentiveContracts](https://github.com/ClaMaSch/RTutorIncentiveContracts)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)
