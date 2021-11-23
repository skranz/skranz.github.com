---
layout: post
title: "RTutor: What explains the employment drop in the great recession 2007-2009?"
cover: 
date: 2021-11-23 10:20:00
categories: r
tags: [R, RTutor, shiny]
---

During the financial crisis and great recession from 2007 to 2009 the US lost 8.6 million jobs. We know that a lot of things happened in the crisis, like a crisis of trust in the financial system including the collapse of Lehman Brothers. Another related crucial aspect was a massive drop in house prices. Highly indebted home owners lost a huge amount of wealth and Keynesian economic theory suggests that this reduces consumption and employment.

In their great article [What Explains the 2007-2009 Drop in Employment?](https://amirsufi.net/miansufi_emtra_2014.pdf) (Econometrica 2014) Atif Mian and Amir Sufi explore in detail the impact of the decline in house prices on employment. They exploit the fact that average reductions in house prices varied significantly between counties partly due to fairly exogenous factors like the availability of land for real estate projects.

As part of her Master Thesis at Ulm University, Birgit Schroff has created a  very nice [RTutor](https://github.com/skranz/RTutor) problem set that allows you to replicate key findings of this analysis in an interactive fashion. It starts with a descriptive analysis but also covers the more advanced topics. For example, an alternative channel by which a house price collapse reduces employment could be the following: banks are harmed from the drop in house prices and subsequently restrict their new loans to businesses. In Section 5 of the problem set you will learn how one can empirically explore why this channel seems to play a less important role compared to direct consumption effects of home owner's wealth reduction. As the argumentation can be a bit subtle, I really like how the problem set puts a lot of emphasis in carefully explaining the different analyses, e.g. by first comparing the hypotheses with simple graphs:

<center>
<img src="http://skranz.github.io/images/house_prices_employment.png" style="max-width: 100%; width: 10cm; margin-bottom: 0.5em;">
</center>

(I believe drawing such graphs is useful beyond a full blown [formal causal analysis with a DAG](https://mixtape.scunning.com/dag.html). The graph on the right hand side is not even a DAG but nevertheless quite informative about the underlying hypothesis.)

You can test the problem set online on shinyapps.io

[https://bschroff.shinyapps.io/RTutorDropInEmployment](https://bschroff.shinyapps.io/RTutorDropInEmployment)

or locally install the problem set, by following the installation guide at the problem set's Github repository:

[https://github.com/bschroff/RTutorDropInEmployment](https://github.com/bschroff/RTutorDropInEmployment)

If you want to learn more about RTutor, try out other problem sets, or create a problem set yourself, take a look at the Github page

[https://github.com/skranz/RTutor](https://github.com/skranz/RTutor)

or at the documentation

[https://skranz.github.io/RTutor](https://skranz.github.io/RTutor)
