---
title: 'A shiny quiz app about the Russian influence campaign before the 2016 US elections'
cover: null
date:   2020-03-05 8:00:00
categories: r
tags: [R, shiny]
---

Based on Robert S. Mueller's investigations about foreign involvement in the 2016 US presidential elections US Congress created [this website](https://intelligence.house.gov/social-media-content/) with the title "Exposing Russia’s Effort to Sow Discord Online: The Internet Research Agency and Advertisements".
It refers to the Internet Research Agency (IRA) as 'the notorious Russian “troll” farm' and cites from the Mueller report that the IRA:

> “[H]ad a strategic goal to sow discord in the U.S. political system, including the 2016 U.S. presidential election. Defendants posted derogatory information about a number of candidates, and by early to mid-2016, Defendants’ operations included supporting the presidential campaign of then-candidate Donald J. Trump (“Trump Campaign”) and disparaging Hillary Clinton. Defendants made various expenditures to carry out those activities, including buying political advertisements on social media in the names of U.S. persons and entities.”

Congress provides data on Tweets written by the IRA and [Facebook advertisement](https://intelligence.house.gov/social-media-content/social-media-advertisements.htm) bought by the IRA. While Facebook provided the Data as PDF files, <a href='https://mith.umd.edu/irads/about' target='_blank'>researchers at the University of Maryland</a> transformed it into convenient <a href='https://mith.umd.edu/irads/data/'>csv data</a>. One example to analyse the data statistically can be found [here](https://www.r-bloggers.com/what-were-ira-facebook-objectives-in-2016-election/) on R-bloggers.

I found it interesting to just look at the advertisements to good better intuition of how foreign influence campaigns are designed in a social-media society. But then I thought why not add a little bit gamification? So I wrote a small shiny app that you can find here:

[http://fbquiz.econ.mathematik.uni-ulm.de/](http://fbquiz.econ.mathematik.uni-ulm.de/)

It is just a little quiz that shows in each round 2 Facebook ads bought by the IRA. You have to guess which ad had more clicks per impression... 

The goal to sow discontent or to manipulate the election seems quite apparent for some ads. Other ads look rather harmless, but keep in mind that most ads were linked to external websites that can have more aggressive content. Ads that directly call to vote for Trump are extremely rare. In contrast, there are many ads whose likely goal was to steer black voters away from Clinton. 

The source code of the quiz app can be found [here](https://github.com/skranz/facebook_ira_ad_quiz) on Github.