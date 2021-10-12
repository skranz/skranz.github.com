---
layout: post
title: "RTutor: Does Bank Lending Increase Before Elections?"
cover: 
date: 2021-10-12 07:10:00
categories: r
tags: [R, RTutor, shiny]
---

Assume you are an incumbent local politician that can exert certain control on your local public savings bank. Would you try to increase lending before local elections to polish economic performance?

In their great article [Electoral Cycles in Savings Bank Lending](https://academic.oup.com/jeea/article/15/2/296/2691493?login=true) (JEEA, 2017) Florian Englmaier and Till Stowasser explore the degree of such electoral lending cycles using detailed German data and non-public banks as a control group. They also study related questions: For example, it does matter whether the election was fairly contested or not. 

As part of his Master Thesis at Ulm University, Markus Reichart has created a very nice [RTutor](https://github.com/skranz/RTutor) problem set that allows you to replicate key findings in an interactive fashion. He puts great emphasis in explaining in detail the underlying difference-in-difference structure that is fairly complex in the final analysis and also adds a nice own analysis about the role of the financial crisis. Like in previous RTutor problem sets, you can enter free R code in a web-based shiny app. The code will be automatically checked and you can get hints how to proceed. 

You can test the problem set online on shinyapps.io

[https://markusreichart.shinyapps.io/RTutorElectionsAndBankLending](https://markusreichart.shinyapps.io/RTutorElectionsAndBankLending)

or locally install the problem set, by following the installation guide at the problem set's Github repository:

[https://github.com/mareichart/RTutorElectionsAndBankLending](https://github.com/mareichart/RTutorElectionsAndBankLending)

Here is a screenshot of one of the plots you generate in the problem set:

<center>
<img src="http://skranz.github.io/images/bank_lending_bavaria.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

(Yes, part of the problem set is a discussion of the parallel trends assumption. While one surely would not mind a bit more parallel development in the shown plot, the final regressions do add additional control variables. Also trends look more parallel if averaged across states.)

If you want to learn more about RTutor, try out other problem sets, or create a problem set yourself, take a look at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

or at the documentation

[https://skranz.github.io/RTutor](https://skranz.github.io/RTutor)
