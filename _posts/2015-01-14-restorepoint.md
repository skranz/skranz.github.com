---
layout: post
title: Debugging with restore points instead of break points
cover: cover.jpg
date:   2015-01-14 12:00:00
categories: r
tags: [R,restorepoint]
---

## break points and restore points

The standard method to debug an R function is to set *break points* via the `browser` function. When during execution of the function, `browser()` is called, the R console immediately changes into an interactive debugging mode that allows to step through the code and enter any R expressions. Thanks to RStudio's visual debugging support, this can be done quite conveniently.

While break points are nice, I personally prefer now most times to debug via *restore points* using my package `restorepoint`.

Here is a simple example for a restore point in a buggy function:


{% highlight r %}
library(restorepoint)
swap.in.vector = function(vec,swap.ind) {
  restore.point("swap.in.vector")
  left = vec[1:(swap.ind - 1)]
  right = vec[swap.ind:nrow(vec)]
  c(right, left)
}}
{% endhighlight %}

When `restore.point("swap.in.vector")` is called inside the function, it stores all currently local variables under the name `"swap.in.vector"`. Since restore point is called at the beginning of the function, the two arguments `vec` and `swap.ind` are stored. If you want to debug the function at some later point (to find its error), you can just run directly in the R-console the body of the function line by line:


{% highlight r %}
  restore.point("swap.in.vector")
  left  = vec[1:(swap.ind-1)]
  #...
{% endhighlight %}

The call to `restore.point("swap.in.vector")` in the global environment copies the originally stored local variables into your global environment and you can (in most situations) replicate the behavior your function by running it line by line directly in your R console.

## Sounds messy? Yet, I like it...

This approach to debugging sounds quite messy on first sight, since you mercilessly overwrite global variables. Surprisingly, however, I find restore points extremely convinient when coding larger R projects. 

I guess in mainly object oriented languages, like Python, restore points are much less useful than in a language like R, which promotes a functional programming paradigm. In particular in R 

  - by default variables are passed by value
  
  - functions should have no side effects
  
  - functions should not use global variables
  
If a function satisfies these conditions, we can basically replicate its behavior from the last time it was called, by just running it line by line in the R console, after restore.point has generated the function's arguments as global variables.

This also means that restore points are not suited for all situations. In particular, they are less useful for debugging code that deviates from the functional paradigm, e.g. if functions have side effects by changing global variables or by changing environments which have been passed by reference.

## Reasons why I like restore points

I prefer restore points in many situations over break points, mainly for the following reasons:

1. When debugging nested function calls, handling several break points can become very tedious, since the program flow is interrupted with every break point. Despite using traceback(), it is often not clear where exactly the error has occured. As a consequence, I tend to set too many break points and the program flow is interrupted too often.

2. When I want to turn off invocation of the browser, I have to comment out #browser() and source again the function body. That can become quite tedious. When using restore points, I typically just keep the calls to restore.point in the code even if I may not need them at the moment. Calls to restore.point are simply not very obtrusive. They just make silently a copy of the data. True: there is some memory overhead and execution slows down a bit. Yet, I usually find that negligible. I tend to have a call to restore.point at beginning of every function. This allows me to always find out, step by step what has happened the last time some function was called.

3. I often would like to restart from the break point after I changed something in the function, to test whether the new code works. But with nested function calls, e.g. inside an optimization procedure, for which an error only occurred under certain parameter constellations, it can sometimes be quite time consuming until the state in which the error has occurred is reached again. This problem does not arise for restore points: I can always restart at the restore point and test my modified function code for the function call that caused the error.

4. The interactive browser used by browser() has a own set of command, e.q. pressing "Q" quits the browser or pressing "n" debugs the next function. For that reason, one cannot always simply copy & paste R code into the browser. This problem does not arise with restore points.

5. One is automatically thrown out the debugging mode of browser() once a line with an error is pasted. This does not happen in the restore point browser. I find it much more convenient to stay in the debug mode. It allows me to paste all the code until an error has occurred and to check only afterward the values of local expressions.


## Installing restorepoint



While there is an old restorepoint version on CRAN, there is an considerably newer and improved version on Github [https://github.com/skranz/restorepoint](https://github.com/skranz/restorepoint). Just install it in the usual fashion with devtools by running the following code:


{% highlight r %}
if (!require(devtools))
  install.packages("devtools")
  
devtools::install_github(repo="skranz/restorepoint")
{% endhighlight %}

## Tutorial for restore points

Take a look at the [restorepoint vignette](https://github.com/skranz/restorepoint/blob/master/vignettes/Guide_restorepoint.md) for a tutorial and an overview of some additional features of the restorepoint package.
