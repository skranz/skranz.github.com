---
layout: post
title: "Shiny without Shiny: RTutor in RStudio's new Visual Markdown Mode"
cover: 
date:   2021-02-02 09:30:00
categories: r
tags: [R, economics]
---


RStudio version 1.4 brought [many great enhancements](https://blog.rstudio.com/2021/01/19/announcing-rstudio-1-4/). My favorite feature is the new [visual markdown editing mode](https://rstudio.github.io/visual-markdown-editing/). This allows to solve interactive [RTutor](https://github.com/skranz/RTutor) problem sets with a shiny look directly in the RStudio editor. Here is a screenshot:


<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/visual_mode.PNG" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

The example RTutor problem set `VisualMode.Rmd` is shown in the left pane. It is opened in the visual markdown editing mode. Students can edit its code chunks as in any other RMarkdown document in RStudio. The RTutor package adds [RStudio addins](http://rstudio.github.io/rstudioaddins/) that allow to check the problem set and get hints. For this to work a "compiled" version  of the sample solution (here `VisualMode.rps`) must be in the same folder as the Rmd file. In the screenshot above, the student makes a common beginner's mistake to add the error term `u` in the call to `lm`. Asking for a hint yields the [automatic adaptive hint](https://skranz.github.io/RTutor/articles/03_HintsAndTests.html) shown in the console, which should hopefully guide the student to the correct solution.

While the problem set can also be solved in the traditional source editor mode, the visual mode looks much nicer because

- latex formulas
- images
- web links

are all directly shown in a nicely rendered version. So these problem sets look almost as nice as the web-based shiny versions of RTutor (e.g. [here](https://skranz.shinyapps.io/MarketAnalysis_2b/)) or [learnr](http://skranz.github.io/r/2019/04/29/RTutor_vs_Learnr.html) exercises, which are also completely shiny based.

The advantage of directly using RStudio is that students can work in the IDE that they use later for work with all its great features like the Environment pane and the R console. Several of my students said that they preferred the RStudio environment even if the problem sets don't look as nice. With the visual markdown mode, directly using RStudio should be even more attractive since a lot of the visual gap to shiny based problem sets can be mended. 

While typically all RTutor problem sets look nicer in the visual markdown mode, I have not yet especially optimized the RTutor problem sets of my courses [Empirical Economics with R](https://github.com/skranz/empecon) and [Market Analysis with Econometrics and Machine Learning](https://github.com/skranz/MarketAnalysis) to the visual markdown mode. But I will probably do so sooner or later. An example solution file that exploits the visual markdown features (formulas, images, links) is [here](https://raw.githubusercontent.com/skranz/RTutor/master/inst/examples/VisualMode_sol.Rmd). 

Note: You should edit the solution file only in source editing mode, since the visual markdown mode automatically rewrites files in a way that is incompatible with some RTutor syntax that extends RMarkdown to specify [blocks](https://skranz.github.io/RTutor/articles/02_ElementsSolutionFile.html) like awards or custom hints. Luckily there is no problem for the resulting Rmd file given to students. It should generally work without problems in visual markdown mode.

### LeverageData's RTutor fork for more shiny Shiny.

There are some features that can still be better implemented with the shiny-based version of RTutor problem sets, mainly multiple choice quizzes or customized HTML. In this context, I am happy to report that RTutor now has another active [fork](https://github.com/LeverageData/RTutor) by [LeverageData](https://www.leveragedata.de/). LeverageData is a young data science consulting firm that also offers professional data science courses that specialize on German speaking participants. (Disclaimer: The Co-founder and CEO Martin Kies was my PhD student, so I root a bit for them, but except for some joint advancement of RTutor, I am not involved with the firm.)

LeverageData uses shiny-based RTutor problem sets for their courses and they have included some nice new features into RTutor. In particular, multiple choice quizzes can be more adaptive in their fork and e.g. contain images. [This shiny app](https://leveragedata.shinyapps.io/QuizExamples/) showcases the new quiz features. While I already merged some elements from LeverageData's fork into the main RTutor fork, the forks will probably remain a bit varied since some of their pure shiny features are less compatible with RMarkdown based problem sets.

