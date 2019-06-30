---
layout: post
title: 'Update: Finding Economic Articles With Data'
cover: 
date:   2019-06-30 20:00:00
categories: r
tags: [R, shiny]
---


An <a href="https://skranz.github.io/r/2019/02/21/FindingEconomicArticles.html">earlier post</a> from February, describes a Shiny app that allows to search among currently more than 4000 economic articles that have an accessible data and code supplement. Finally, I managed to configure an nginx reverse proxy server and now you can also access the app under a proper https link here:

[https://ejd.econ.mathematik.uni-ulm.de](https://ejd.econ.mathematik.uni-ulm.de)

(I was very positively surprised how easy [certbot](https://certbot.eff.org/) makes it to add https.) Some colleagues told me that they could not access the app under the originally posted link:

[http://econ.mathematik.uni-ulm.de:3200/ejd/](http://econ.mathematik.uni-ulm.de:3200/ejd/)

I am not sure about the exact reason, but perhaps some security settings don't allow to access web sites on a non-standard port like 3200. Hopefully the new link helps.

Since my initial post, the number of articles has grown, and I included new journals like  [Econometrica](https://onlinelibrary.wiley.com/journal/14680262) or [AER Insights](https://www.aeaweb.org/journals/aeri).


The main data for my app can be downloaded as a [zipped SQLite database](http://econ.mathematik.uni-ulm.de/ejd/articles.zip) from my server. Let us do some analysis. 

```r
library(RSQLite)
library(dbmisc)
library(dplyr)
db = dbConnect(RSQLite::SQLite(),"articles.sqlite") %>%
  set.db.schemas(schema.file=system.file("schema/articles.yaml", package="EconJournalData"))

articles = dbGet(db,"article")
fs = dbGet(db,"files_summary")
```

Let us look grouped by journal at the share of articles whose code supplement has R files:

```r
fs %>% 
  left_join(select(articles, id, journ), by="id") %>%
  group_by(journ) %>%
  mutate(num_art = n_distinct(id)) %>%
  filter(file_type=="r") %>%
  summarize(num_art = first(num_art), num_with_r = n(), share_with_r=round((num_with_r / first(num_art))*100,2), ) %>%
  arrange(desc(share_with_r))
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">journ</th><th class="data-frame-th">num_art</th><th class="data-frame-th">num_with_r</th><th class="data-frame-th">share_with_r</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">ecta</td><td class="data-frame-td" nowrap bgcolor="#dddddd">109</td><td class="data-frame-td" nowrap bgcolor="#dddddd">17</td><td class="data-frame-td" nowrap bgcolor="#dddddd">15.6</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">jep</td><td class="data-frame-td" nowrap bgcolor="#ffffff">113</td><td class="data-frame-td" nowrap bgcolor="#ffffff">11</td><td class="data-frame-td" nowrap bgcolor="#ffffff">9.73</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">restud</td><td class="data-frame-td" nowrap bgcolor="#dddddd">216</td><td class="data-frame-td" nowrap bgcolor="#dddddd">12</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5.56</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejmic</td><td class="data-frame-td" nowrap bgcolor="#ffffff">114</td><td class="data-frame-td" nowrap bgcolor="#ffffff">5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">4.39</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aer</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1453</td><td class="data-frame-td" nowrap bgcolor="#dddddd">46</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.17</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejpol</td><td class="data-frame-td" nowrap bgcolor="#ffffff">378</td><td class="data-frame-td" nowrap bgcolor="#ffffff">11</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.91</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp</td><td class="data-frame-td" nowrap bgcolor="#dddddd">385</td><td class="data-frame-td" nowrap bgcolor="#dddddd">11</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.86</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejmac</td><td class="data-frame-td" nowrap bgcolor="#ffffff">282</td><td class="data-frame-td" nowrap bgcolor="#ffffff">8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.84</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">jeea</td><td class="data-frame-td" nowrap bgcolor="#dddddd">115</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.74</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">restat</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">733</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">6</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">0.82</td></tr>
</table>
We see that there is quite some variation in the share of articles with R code going from 15.6% in Econometrica (ecta) to only 0.82% in the Review of Economics and Statistics (restat). (The statistics exclude all articles that don't have a code supplement or a supplement whose file types I did not analyse, e.g. because it is too large or the ZIP files are nested too deeply.)

Overall, we still have a clear dominance of Stata in economics:

```r
# Number of articles with analyes data & code supplementary
n_art = n_distinct(fs$id)

# Count articles by file types and compute shares
fs %>% group_by(file_type) %>%
  summarize(count = n(), share=round((count / n_art)*100,2)) %>%
  # note that all file extensions are stored in lower case
  filter(file_type %in% c("do","r","py","jl","m")) %>%
  arrange(desc(share))
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">file_type</th><th class="data-frame-th">count</th><th class="data-frame-th">share</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2834</td><td class="data-frame-td" nowrap bgcolor="#dddddd">70.18</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">m</td><td class="data-frame-td" nowrap bgcolor="#ffffff">979</td><td class="data-frame-td" nowrap bgcolor="#ffffff">24.24</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">r</td><td class="data-frame-td" nowrap bgcolor="#dddddd">129</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.19</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">py</td><td class="data-frame-td" nowrap bgcolor="#ffffff">42</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.04</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">jl</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">2</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.05</td></tr>
</table>

Roughly 70% of the articles have Stata `do` files and almost a quarter Matlab `m` files and only slightly above 3% R files.

I also meanwhile have added a log file to the app that anonymously stores data about which articles that have been clicked on. The code below shows the 20 most clicked on articles so far:

```r
dat = read.csv("article_click.csv")

dat %>%
  group_by(article) %>%
  summarize(count=n()) %>%
  na.omit %>%
  arrange(desc(count)) %>%
  print(n=20)
```

```
## # A tibble: 699 x 2
##    article                                                           count
##    <fct>                                                             <int>
##  1 A Macroeconomic Model of Price Swings in the Housing Market          27
##  2 Job Polarization and Jobless Recoveries                              20
##  3 Tax Evasion and Inequality                                           19
##  4 Public Debt and Low Interest Rates                                   16
##  5 An Empirical Model of Tax Convexity and Self-Employment              13
##  6 Alcohol and Self-Control: A Field Experiment in India                11
##  7 Drug Innovations and Welfare Measures Computed from Market Deman~    11
##  8 Food Deserts and the Causes of Nutritional Inequality                11
##  9 Some Causal Effects of an Industrial Policy                          11
## 10 Costs  Demand  and Producer Price Changes                            10
## 11 Breaking Bad: Mechanisms of Social Influence and the Path to Cri~     9
## 12 Government Involvement in the Corporate Governance of Banks           8
## 13 Performance in Mixed-sex and Single-sex Tournaments: What We Can~     8
## 14 Disease and Gender Gaps in Human Capital Investment: Evidence fr~     7
## 15 Housing Constraints and Spatial Misallocation                         7
## 16 Inherited Control and Firm Performance                                7
## 17 Labor Supply and the Value of Non-work Time: Experimental Estima~     7
## 18 Pricing in the Market for Anticancer Drugs                            7
## 19 The Arrival of Fast Internet and Employment in Africa                 7
## 20 The Economic Benefits of Pharmaceutical Innovations: The Case of~     7
## # ... with 679 more rows
```

For a nice thumbnail in R-bloggers let us finish with a screenshot of the app:
<br>
<img src="http://skranz.github.io/images/search2.PNG" style="width: 100%; height: 100%">
<br>

