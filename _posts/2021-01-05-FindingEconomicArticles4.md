---
layout: post
title: 'Finding Economic Articles with Data and Specific Empirical Methods'
cover: 
date:   2021-01-05 08:00:00
categories: r
tags: [R, economics]
---

Do you want to find reproducible empirical economic studies that use a particular method or concept, like random forests or instrumental variable estimation? This becomes now even easier with my freshly updated shiny-powered app "Find Economic Articles with Data":

[https://ejd.econ.mathematik.uni-ulm.de](https://ejd.econ.mathematik.uni-ulm.de)

The app allows to search among more than 5800 articles with data and code supplement from several top economic journals. The previous version already allowed to search within the title and abstract for arbitrary phrases. While an research area like `climate change` or `financial crisis` can typically be well detected from the abstract, in applied papers the abstract only rarely provides information about the used empirical methods.

To improve the app, I counted the number of occurrences of special methodological phrases like `random forest` in the full texts of more than 5200 articles. Often several phrases are mapped to a single keyword. For example, the keyword `lab experiment` aggregates full text occurrences of the phrases `laboratory experiment`, `laboratory study`, `lab experiment` and `experimental laboratory`.

Here is a screenshot of a search result:

<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/ejd/ejd4.PNG" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

In that example, I search for `electricity`, which is no special keyword, and the method keyword `DID` that indicates a difference-in-differences approach. The search results show for each article the detected method keywords and number of occurrences in the full text. This gives a quick overview of an article's methodology. A simple way to add such a keyword to your search query, is to click on it in the search results. Alternatively, go to the `Help` panel for a list of all keywords.

Note that e.g. due to confidentiality agreements a substantial share of data supplements unfortunately doesn't contain all data sets required to replicate the study. This can typically be checked by looking at the README file of the data supplement which I tried to link for most search results.

Here is the top 10 of method keywords ordered by the number of articles they are used in:

<div>

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 table.data-frame-table tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">Rank</th><th class="data-frame-th">Keyword</th><th class="data-frame-th">No. of Articles</th><th class="data-frame-th">Share</th><th class="data-frame-th">Matches per Article</th><th class="data-frame-th">Matched Phrases</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">equilibrium</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3000</td><td class="data-frame-td" nowrap bgcolor="#dddddd">59.2%</td><td class="data-frame-td" nowrap bgcolor="#dddddd">16.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">equilibrium</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">fixed effect</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2940</td><td class="data-frame-td" nowrap bgcolor="#ffffff">58%</td><td class="data-frame-td" nowrap bgcolor="#ffffff">12</td><td class="data-frame-td" nowrap bgcolor="#ffffff">fixed-effect, fixed effect</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">IV</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1870</td><td class="data-frame-td" nowrap bgcolor="#dddddd">36.9%</td><td class="data-frame-td" nowrap bgcolor="#dddddd">6.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">instrumental variable, _ instrument _</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">4</td><td class="data-frame-td" nowrap bgcolor="#ffffff">panel data</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1570</td><td class="data-frame-td" nowrap bgcolor="#ffffff">31%</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">panel data</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">time series</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1280</td><td class="data-frame-td" nowrap bgcolor="#dddddd">25.2%</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">time series</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">nonparametric</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1140</td><td class="data-frame-td" nowrap bgcolor="#ffffff">22.4%</td><td class="data-frame-td" nowrap bgcolor="#ffffff">3.7</td><td class="data-frame-td" nowrap bgcolor="#ffffff">nonparametric, non-parametric</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">field experiment</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1080</td><td class="data-frame-td" nowrap bgcolor="#dddddd">21.2%</td><td class="data-frame-td" nowrap bgcolor="#dddddd">4.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">field experiment</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">natural experiment</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1010</td><td class="data-frame-td" nowrap bgcolor="#ffffff">19.9%</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">natural experiment</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">DID</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1010</td><td class="data-frame-td" nowrap bgcolor="#dddddd">19.8%</td><td class="data-frame-td" nowrap bgcolor="#dddddd">4.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">difference-in-difference, DID, DiD, DD, difference in difference, differences-in-difference</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">10</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">bootstrap</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">1000</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">19.7%</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">4.9</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">bootstrap</td></tr>
</table>
</div>

While it is unclear how many economic processes are actually in some form of equilibrium, economists just love this expression. It appears in roughly 60% of the (mostly empirical) articles at least once. On average `equilibrium` is mentioned more than 16 times in the articles that mention it at least once.

Close behind is the keyword `fixed effects`. Well, I guess many regressions just add some fixed effects as control variables.

Ranked third is `IV`, which matches `instrumental variable` or just ` instrument ` (with leading and trailing spaces). Even so the phrase ` instrument ` may sometimes be used in different contexts, the third rank reflects that economists really like the instrumental variable technique to identify causal effects.

We then see that `panel data` seems a bit more popular than `time series` and that more than 20% of articles at least mention something `nonparametric`. Afterward, we have a tight race between `field experiment` and `natural experiment` which both are mentioned in around 20% of articles. In the same ballpark and likely with a considerable overlap are articles that mention `DID`, i.e. difference-in-difference as a method of causal identification. And finally still more than 1000 articles refer to `bootstraping`.

There are many more keywords than these top 10, e.g. covering areas like machine learning, the potential outcomes framework for causal identification, or macro-econometrics.  Best [search yourself](https://ejd.econ.mathematik.uni-ulm.de)... 


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
