---
layout: post
title: "Mozart's Emotions and Creativity: Statistical Analysis of Composers' Letters and Output."
cover: 
date: 2022-04-26 06:00:00
categories: r
tags: [R, RTutor, shiny]
---

How did changes in life and emotional states affect the creativity and output of the great composers Mozart, Beethoven and Liszt? (Yeah, 3 composers are studied but Mozart seems to sell best, so he is in the title.) In his great article [How Are You, My Dearest Mozart? Well-Being and Creativity of Three Famous Composers Based on Their Letters](https://doi.org/10.1162/REST_a_00616) (ReStat 2017) Karol Jan Borowiecki studies this question using a unique database of works, bibliographic data and most importantly 1400 letters of the three composers.

As part of her Bachelor Thesis at Ulm University, Daniel Klinke has created a very nice [RTutor](https://github.com/skranz/RTutor) problem set that allows you to replicate key findings of this analysis in an interactive fashion. In chapter 4 you will dig into the statistical text analysis of the composers' letters using the modern R package [quanteda](https://quanteda.io/) to extract their emotional states during different periods of their lives.

Before, you get a descriptive overview of the emotional states and output over the composers' live, as in the following screenshot:

<center>
<img src="https://skranz.github.io/images/mozart.svg" style="max-width: 100%;margin-bottom: 0.5em;">
</center>

Then it is also explained in detail how to establish the causal impact of emotions on creativity, and how to address potential pitfalls, e.g. by using an instrumental variables approach.

You can test the problem set online on shinyapps.io

[https://dklinke.shinyapps.io/RTutorCreativity/](https://dklinke.shinyapps.io/RTutorCreativity/)

or locally install the problem set, by following the installation guide at the problem set's Github repository:

[https://github.com/DKlinke/RTutorCreativity](https://github.com/DKlinke/RTutorCreativity)

If you want to learn more about RTutor, try out other problem sets, or create a problem set yourself, take a look at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

or at the documentation

[https://skranz.github.io/RTutor](https://skranz.github.io/RTutor)
