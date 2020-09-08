---
layout: post
title: "Solving Interactive R Exercises: RStudio Editor vs Shiny Web Interface"
cover: 
date: 2020-09-08 18:00:00
categories: r
tags: [R, RTutor, shiny]
---

My course [Market Analysis with Econometrics and Machine Learning](https://github.com/skranz/MarketAnalysis) uses a lot of [RTutor](https://skranz.github.io/RTutor/) problem sets where students can solve interactive exercises with automatic hints and checks of their solutions.

One can design RTutor problem sets to be solved in a shiny web interface (for an example see [https://skranz.shinyapps.io/MarketAnalysis_2b](https://skranz.shinyapps.io/MarketAnalysis_2b/)), which is similar to [learnr](https://github.com/rstudio/learnr). Another way is to design the problem sets such that students solve them directly in RStudio by editing an RMarkdown document. There they can use an RStudio Addin to check the problem set and get a hint after a failed check by typing `hint()` in the R console.

The course started with problem sets where students directly edited an Rmd file in RStudio. That was because I wanted students to get used to a professional IDE and the Rmd format, which are great tools beyond the course. Nevertheless, later problem sets in my course were designed for the shiny web interface  because they contained longer explanations, longer mathematical formulas and multiple choice quizzes.

In the course evaluation I asked the students which form of problem sets they preferred. I thought that most students would prefer the shiny web interface because it just looks nicer and it seems more convenient for beginners. Yet, interestingly a (slim) majority of students preferred the RStudio version where they edited an RMarkdown file and several students would have liked all problem sets in that format. It looks as if once students get used to a full fledged IDE, they may really prefer it over a shiny web interface.

Next year I would like to give students the option to solve all problem sets by editing an RMarkdown file in RStudio. At the same time I want to stick to the default web interface for the later problem sets because also a substantial fraction of students preferred the shiny interface. In principle, a problem could already be solved with both interfaces. But that worked properly only for problem sets that did not contain multiple choice quizzes which were only implemented for the web interface.

So I updated RTutor and multiple choice quizzes can now also be conveniently solved when working with an Rmd file in RStudio. I am looking forward to see which options the students will pick next year when having free choice...
