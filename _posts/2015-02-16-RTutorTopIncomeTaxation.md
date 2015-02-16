---
layout: post
title: 'RTutor Kicks Off: An Interactive R Problem Set about Top Income Taxation'
cover: 
date:   2015-02-16 12:00:00
categories: r
tags: [R,RTutor, shiny]
---

RTutor is a new R package that allows to develop interactive R exercises. Problem sets can be solved off-line or can be hosted online via shiny server.

## Try out an Interactive Problem Set about Optimal Taxation of Top Labor Incomes

Jonas Send has written a very nice interactive R problem set that allows you to explore the key insights of the article "Optimal Taxation of Top Labor Incomes: A Tale of Three Elasticities" by Thomas Piketty, Emmanuel Saez and Stefanie Stantcheva [(AEJ Policy, 2014)](https://www.aeaweb.org/articles.php?doi=10.1257/pol.6.1.230). By solving it, you can learn a bit about R, about econometrics and about historical relationships between top income taxation and income inequality.

To install and try it out locally, follow the instructions here:

[https://github.com/skranz/RTutorTopIncomeTaxation](https://github.com/skranz/RTutorTopIncomeTaxation)

Currently there is also an online version hosted by shinyapps.io:

[https://skranz.shinyapps.io/RTutorTopIncomeTaxation/](https://skranz.shinyapps.io/RTutorTopIncomeTaxation/)

(The online version sometimes greys out. On my computer it runs more stable with Chrome than with Firefox. The link for the online version may change in the future. Check out the github page if the link does not work anymore.)

### A screenshot:

![alt text](http://skranz.github.io/images/RTutorTopIncomeTaxation.PNG)

## Building your own RTutor problem sets

If you want to build your own problem set, take a look at the RTutor github page


[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

and read the vignette

[Guide for Developing Interactive R Problemsets.md](https://github.com/skranz/RTutor/blob/master/vignettes/Guide_for_Developing_Interactive_R_Problemsets.md)

The core of a problem set is an RMarkdown solution file that is augmented by specific syntax to include customized tests, hints or info boxes. I try to make RTutor generate sensible tests and useful hints automatically. Yet, customization often improves a problem set. For a comprehensive example, take a look at Jonas' solution file for the problem set on Top Income Taxation:

[Optimal Taxation of Top Labor Incomes sol.Rmd](https://raw.githubusercontent.com/skranz/RTutorTopIncomeTaxation/master/inst/ps/Optimal%20Taxation%20of%20Top%20Labor%20Incomes_sol.Rmd)

## Using RTutor for courses

I plan to regularly use RTutor for some of my courses at Ulm University. In addition to the browser-based interface, problem sets can also have the form of RMarkdown files. RMarkdown based problem sets can be solved interactively using RStudio, which may be preferable for more advanced classes. I plan to write up a small vignette with advice on how to use RTutor for courses after I have gained sufficient experience and have implemented some features that facilitate submission and assessment of students' solutions.

## Feedback and Issues

RTutor is still a very young project and I appreciate your feedback. If you have ideas for nice features or find some bug, just open an issue on the project's github page:

[https://github.com/skranz/RTutor/issues](https://github.com/skranz/RTutor/issues)


