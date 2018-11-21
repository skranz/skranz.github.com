---
layout: post
title: 'RTutor: Driving Electric or Gasoline Cars? Comparing the Pollution Damages.'
cover: 
date:   2018-11-21 8:00:00
categories: r
tags: [R, RTutor, shiny]
---

How does driving an electric car compare to driving a similar gasoline car in terms of total pollution damages? While some comments about the future of mobility may suggest that electric cars are already clearly much more environmental friendly, the answer is not at all clear cut and depends on a lot of factors. For example, how does one weight local pollutions that mainly create health damages against carbon dioxide emmisions that contribute to global warming? While the local pollution damages of a gasoline car mainly depend on where the car is driven, a most relevant factor for electric cars is which power plants generate the required electricity. Also outside temperatures play an important role by affecting the efficiency of electric cars.

In their very interesting article <a href="https://www.aeaweb.org/articles?id=10.1257/aer.20150897" target="_blank">Are There Environmental Benefits from Driving Electric Vehicles? The Importance of Local Factors</a> (American Economic Review, 2016) Stephen P. Holland, Erin T. Mansur, Nicholas Z. Muller and Andrew J. Yates conduct a careful analysis of this question for the United States.

As part of his Master Thesis at Ulm University, Felix Stickel has generated a very nice RTutor problem set that allows you to explore the insights in an entertaining, interactive fashion. You learn a lot about the question at hand, delve into detailed electricity and pollution data and at the same time can hone your data science and R skills.

Here is a small screenshot of one many graphs generated in the interactive problem set:

<img src="http://skranz.github.io/images/ford_focus.png" style="width: 100%; height: 100%">

<hr>

Like in previous RTutor problem sets, you can enter free R code in a web based shiny app. The code will be automatically checked and you can get hints how to proceed. In addition you are challenged by many multiple choice quizzes.


You can test the problem set online on the rstudio.cloud: [https://rstudio.cloud/project/139129](https://rstudio.cloud/project/139129) Source the `run.R` file to start the problem set.


If you don't want to register at rstudio cloud, you can also check out the problem on shinyapps.io: [https://felsti.shinyapps.io/RTutorECars](https://felsti.shinyapps.io/RTutorECars)

The free shinyapps.io account is capped at 25 hours total usage time per month. So it may be greyed out when you click at it. Also, unlike on rstudio.cloud, you cannot save your progress on shinyapps.io.

To locally install the problem set, follow the installation guide at the problem set's Github repository: [https://github.com/felsti/RTutorECars](https://github.com/felsti/RTutorECars)

If you want to learn more about RTutor, to try out other problem sets, or to create a problem set yourself, take a look at the RTutor Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)
