---
layout: post
title: "Finding Economic Articles with Data und Stata Reproductions"
cover: 
date: 2023-01-23 12:20:00
categories: r
tags: [R, economics]
---

If you're looking for reproducible empirical economic studies, check out my Shiny-powered search app at

[https://ejd.econ.mathematik.uni-ulm.de](https://ejd.econ.mathematik.uni-ulm.de)

(Recently the server was sometimes off-line, but most of the time you can use it.)

Most empirical analyses in economics are performed with Stata, but it is a great exercise for students to replicate parts in R, e.g. in form of an [interactive RTutor problem set](https://github.com/skranz/RTutor). 

For this and other purposes, it is helpful if one can quickly look at the results of the Stata code in an article's data and code supplement. For example, if many Stata commands throw errors due to missing data sets, one might want to pick another study for replication in R.

Thus, I've spent quite some time building a pipeline to automatically execute Stata code in data and code supplements and to make the results easily accessible.

For example, [here you can see the Stata reproduction](https://econ.mathematik.uni-ulm.de/ejd/repbox/aer/aer_112_9_9/) of [Kranz and Puetz (AER, 2022)](https://www.aeaweb.org/articles?id=10.1257/aer.20210121). (Yeah, that is some blatant advertisement of the only article co-authored by me in the data base. But it just fits nicely to the theme: the link shows an automatic reproduction of a published comment replicating a meta study on p-hacking. The original meta study has an author who founded the Institute for Replication and the replication has an author who created a database that contains automatic reproductions of the replication, of the replicated meta study and of some studies studied in the meta study.) OK, let's move on to a screenshot: <br>

<center>
<a href="https://econ.mathematik.uni-ulm.de/ejd/repbox/aer/aer_112_9_9/do.html?do=3"><img src="http://skranz.github.io/images/ejd/ejd_repbox_1.jpeg" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

The reproduction includes HTML versions of the Stata do files, allowing you to click on each command and see the output. Graphical output is also displayed, as shown in the screenshot above.

Search results in [my app](https://ejd.econ.mathematik.uni-ulm.de) indicate if an automatic Stata reproduction is available, and what percentage of commands ran without error: <br>

<center>
<a href="https://ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/ejd/ejd_repbox_2.jpeg" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

(If it weren't for the amazing work of the [AEA Data Editor](https://aeadataeditor.github.io/) Lars Vilhuber and his team, in particular their efforts to push authors to improve their supplements, there would surely not be a 100% at the Stata reproduction of our comment. Lars' [Docker container set-up](https://aeadataeditor.github.io/posts/2021-11-16-docker.) is also a key element of my reproduction pipeline.)

If you click on the [link of a Stata reproduction](https://econ.mathematik.uni-ulm.de/ejd/repbox/aer/aer_112_9_9/), you'll first see an overview page with information such as missing and existing data sets and the total run-time. The app also also allows to filter search results based on the Stata reproductions. Unfortunately, not all articles have automatic reproductions yet, as it takes a lot of time to run them. Also the pipeline is still work in progress, so some reproduction problems are due to limits and errors in my pipeline while the code in the supplement is perfectly fine. 

For example, before running a do file, my pipeline injects and rewrites some code so that the output can be nicely extracted and operations like file path correction can be performed. For this purpose, I have written a rudimentary Stata parser that first transforms the code in a form that facilitates those code modifications. However, Stata can change inside a do file to another language called Mata, which would mess up my parser and code transformation. Sometimes I can handle those Mata blocks using a simple heuristic, but sometimes the use of Mata can cause problems for my pipeline and lead to reproduction errors. As not too many supplements use Mata, fixing this particular issue has not the highest priority, however.

There are further plans (and some drafts for funding applications), like making the reproduction systematically extract regression results, map them to regression tables in the PDFs of the published articles, and store everything in a data base to facilitate meta studies. Of course, automatically running the R code in supplements is also on the list.

## ChatGPT Replication

At the end let us add an automatic replication of this blog post. I asked ChatGPT to rewrite the text above as a pop ballad...

```
Verse 1:
If you're searching for the truth
In economic studies, youth
Check out my Shiny-powered app
You'll find what you're looking for, snap

Chorus:
My search engine's got your back
With reproducible facts
Empirical studies in sight
Just follow the link, make it right

Verse 2:
Stata's the tool of the trade
But replication's a game to be played
With R, it's a great exercise
To see the errors and the results rise

Chorus:
My search engine's got your back
With reproducible facts
Empirical studies in sight
Just follow the link, make it right

Bridge:
I've spent time building a pipeline
To execute Stata code in no time
The results are easy to find
Just search and you'll see, it's all fine

Outro:
Not all articles are there yet
But with time, we'll not forget
To run them all, it takes a while
But it's worth it, with a smile.
```

<script type="text/javascript">
var sc_project=12455234; 
var sc_invisible=1; 
var sc_security="36f1b76e"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="real time web
analytics" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12455234/0/36f1b76e/1/"
alt="real time web analytics"></a></div></noscript>
