---
layout: post
title: 'Interactive RTutor Problemsets via RStudio Cloud'
cover: 
date: 2018-06-04 09:00:00
categories: r
tags: [R]
---  

I just learned about RStudio Cloud (see [https://rstudio.cloud/](https://rstudio.cloud/)) that allows to simply run RStudio instances from your browser. Moreover, you can simply set-up RStudio projects that other users can simply copy and use themselves. RStudio Cloud is still in alpha and currently one can freely register to use it.

I have set up a project that allows you to simply test and create an interactive RTutor problem set. Just click on the following link:

[https://rstudio.cloud/project/39040](https://rstudio.cloud/project/39040)

You are first asked to sign-up to RStudio Cloud or to sign-in via your Google or Github account. Then you see a running instance of RStudio in your browser. You can look at the `README.md` file or directly open the file `Intro.Rmd` from the file pane to solve an RMarkdown based interactive problem set directly in RStudio.

<img src="http://skranz.github.io/images/rcloud1.PNG">

This problem set is a simple introduction to R that is typically the first problem set in my courses at [Ulm University](https://www.uni-ulm.de/en/) before additional problem sets focus on the topics of the course.

The RMarkdown file `Intro.Rmd` contains several chunks, in which the student shall enter some code. One can check the solution via `Addins -> Check Problemset` and get hints for the current chunk by typing `hint()` in the R console. One also sees how many exercises have already been solved by typing `stats()` and can earn awards that are shown by `awards()`.

Nowadays there exists a lot of great ways to learn R in such an interactive fashion, like the web-based [Data Camp courses](https://www.datacamp.com) or [Swirl](http://swirlstats.com) that is mainly based on direct input into the R console. Yet, to the best of my knowledge  [RTutor](https://github.com/skranz/RTutor) is still unique by allowing to interactively work through RMarkdown files directly in RStudio. Hence, students can learn in the same environment that they probably use later in their own data science projects. 

The file `Intro_sol.Rmd` in the subdirectory `ps_source` contains the source code used to generate this problem set.

<img src="http://skranz.github.io/images/rcloud3.PNG">

This is basically a sample solution of the problem set. RTutor is able to automatically generate hints and tests of the student's input from such a sample solution. It is also possible to customize hints and specify options how the student's solution shall be checked. You can modify `Intro_sol.Rmd` to generate your own RTutor problem set, which you can host on your modified copy of my RStudio Cloud instance. [This vignette](https://github.com/skranz/RTutor/blob/master/vignettes/Guide_for_Developing_Interactive_R_Problemsets.md) offers more details on how to create RTutor problem sets.


RTutor also offers the opportunity to show and solve problem sets in shiny based format in the browser, as in the following screenshot:

<img src="http://skranz.github.io/images/rcloud2.PNG">

You can use the shiny based version also from the RStudio Cloud instance by simply typing in the R console:
```r
library(RTutor)
show.ps("Intro", user.name="TestUser")
```

Several of my students at Ulm University have created very nice nice, shiny-based RTutor problem sets based on empirical research articles in economics. The problem sets are hosted on shinyapps.io and Github. Short descriptions of a some of these problem sets are given on my blog 
[http://skranz.github.io](http://skranz.github.io) and a list of these problem sets is on the RTutor Github page: [https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)


In my courses, I let students solve RTutor problem sets in the RMarkdown format, however. After having solved a problem set, a student can create a submission file with the command `make.submission()` and upload it to the Moodle page of my course. At the end of the course, I download a big ZIP file of all submission and automatically grade the problem sets based on the R code here: [https://github.com/skranz/RTutor/blob/master/grading/grade_sub.r](https://github.com/skranz/RTutor/blob/master/grading/grade_sub.r).

Currently, in my courses all students install RStudio and RTutor locally on their computer. This works, but sometimes it can be a bit time consuming to get the software work on every computer given heterogeneous operating systems and the fact that not every student is very IT savvy. RStudio Cloud looks like a great alternative if one wants to avoid local installation for each student. Of course, in how far that is a long-run viable option will depend on the pricing system RStudio will choose after the test phase and whether you have an available budget for it.

By the way, if you have generated your own RTutor problem set and would like to share it, you can send me an email (email address is on [my website](https://www.uni-ulm.de/mawi/mawi-wiwi/institut/mitarbeiter/skranz/)) with a link to your Github repository or RStudio Cloud project. It would be great to have a list of external RTutor problem sets.

