---
layout: post
title: 'RStudio Addin: Replace in Files'
cover: 
date: 2019-10-03 21:00:00
categories: r
tags: [R, shiny]
---

When developing a new R project, I usually want to quickly get some examples running. This means in early stages, I don't spend much time to consistently name my functions or variables. Correspondingly, there is usually a later phase, before making the project public, in which I change a lot of function names in order to get a bit more consistency.

Since in larger projects function calls are distributed over a lot of R files the `Find in Files` command in RStudio is really useful. Yet, unfortunately so far there is no `Find and Replace in Files` command. There is a corresponding [Github issue](https://github.com/rstudio/rstudio/issues/2066) that shows that more users would love to have such a functionality. Probably it will be implemented in some future RStudio version, but it is not clear when.

As an intermediate solution you can install my very small package [ReplaceInFiles](https://github.com/skranz/ReplaceInFiles). It just provides an [RStudio Addin](https://rstudio.github.io/rstudioaddins/) that allows to find and replace in multiple files.

You can install the package directly from Github by running
```r
if (!require(devtools)) install.packages("devtools")
devtools::install_github("skranz/ReplaceInFiles")
```
You can then use the `Replace in Files` addin in RStudio under the `Addins`.

Screenshot:

<img src="http://skranz.github.io/images/replace_in_files.PNG" style="width: 100%; height: 100%">

By default my addin searches in all files in your project directory (or if no project is open, in your working directory). Instead of changing the files in the background, it opens all files that contain the search pattern in RStudio and replaces the found patterns in RStudio without saving the files. This allows you the possibility to easily undo changes. You can quickly save all changes in RStudio via `File -> Save All`

If you look at the [source code](https://github.com/skranz/ReplaceInFiles/tree/master/R) on Github, you see that there is really very little code and I wrote it quite quickly (hopefully avoiding bugs). It is amazing how easily RStudio Addins and the [rstudioapi](https://cran.rstudio.com/web/packages/rstudioapi/index.html) package allow you to customize RStudio and extend its features. 
