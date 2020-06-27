---
layout: post
title: 'Finding Economic Articles with Data (2nd Update)'
cover: 
date:   2020-06-25 16:00:00
categories: r
tags: [R, shiny]
---



Almost a year is now gone since I posted my last update about my shiny-powered search app. It allows to search among currently more than 5000 economic articles that have an accessible data and code supplement:

[https://ejd.econ.mathematik.uni-ulm.de](https://ejd.econ.mathematik.uni-ulm.de)

(Note that the server was offline on June the 26th and 27th due a power outage. Its now online again)

<br>
<a href="https://ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/search3.PNG" style="width: 100%; height: 100%"></a>
<br>

The main data for my app can be downloaded as a [zipped SQLite database](http://econ.mathematik.uni-ulm.de/ejd/articles.zip) from my server. Let us do some analysis. 

```r
library(RSQLite)
library(dbmisc)
library(dplyr)
db = dbConnect(RSQLite::SQLite(),"articles.sqlite") %>%
  set.db.schemas(
    schema.file=system.file("schema/articles.yaml",
    package="EconJournalData")
  )

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
  summarize(
    num_art = first(num_art),
    num_with_r = n(),
    share_with_r=round((num_with_r / first(num_art))*100,2)
  ) %>%
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
<tr><th class="data-frame-th">journ</th><th class="data-frame-th">num_art</th><th class="data-frame-th">num_with_r</th><th class="data-frame-th">share_with_r</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">ecta</td><td class="data-frame-td" nowrap bgcolor="#dddddd">144</td><td class="data-frame-td" nowrap bgcolor="#dddddd">19</td><td class="data-frame-td" nowrap bgcolor="#dddddd">13.19</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aeri</td><td class="data-frame-td" nowrap bgcolor="#ffffff">28</td><td class="data-frame-td" nowrap bgcolor="#ffffff">3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10.71</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">jep</td><td class="data-frame-td" nowrap bgcolor="#dddddd">127</td><td class="data-frame-td" nowrap bgcolor="#dddddd">12</td><td class="data-frame-td" nowrap bgcolor="#dddddd">9.45</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">restud</td><td class="data-frame-td" nowrap bgcolor="#ffffff">312</td><td class="data-frame-td" nowrap bgcolor="#ffffff">22</td><td class="data-frame-td" nowrap bgcolor="#ffffff">7.05</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">jpe</td><td class="data-frame-td" nowrap bgcolor="#dddddd">155</td><td class="data-frame-td" nowrap bgcolor="#dddddd">9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5.81</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejmic</td><td class="data-frame-td" nowrap bgcolor="#ffffff">129</td><td class="data-frame-td" nowrap bgcolor="#ffffff">5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">3.88</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejpol</td><td class="data-frame-td" nowrap bgcolor="#dddddd">426</td><td class="data-frame-td" nowrap bgcolor="#dddddd">15</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.52</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aer</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1540</td><td class="data-frame-td" nowrap bgcolor="#ffffff">53</td><td class="data-frame-td" nowrap bgcolor="#ffffff">3.44</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">jeea</td><td class="data-frame-td" nowrap bgcolor="#dddddd">154</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.25</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejapp</td><td class="data-frame-td" nowrap bgcolor="#ffffff">430</td><td class="data-frame-td" nowrap bgcolor="#ffffff">13</td><td class="data-frame-td" nowrap bgcolor="#ffffff">3.02</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejmac</td><td class="data-frame-td" nowrap bgcolor="#dddddd">314</td><td class="data-frame-td" nowrap bgcolor="#dddddd">8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.55</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">restat</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">813</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">6</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">0.74</td></tr>
</table>
We see that there is quite some variation in the share of articles with R code going from 13.2% in Econometrica (ecta) to only 0.74% in the Review of Economics and Statistics (restat). (The statistics exclude all articles that don't have a code supplement or a supplement whose file types I did not analyse, e.g. because it is too large or the ZIP files are nested too deeply.)

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
<tr><th class="data-frame-th">file_type</th><th class="data-frame-th">count</th><th class="data-frame-th">share</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3338</td><td class="data-frame-td" nowrap bgcolor="#dddddd">70.44</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">m</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1195</td><td class="data-frame-td" nowrap bgcolor="#ffffff">25.22</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">r</td><td class="data-frame-td" nowrap bgcolor="#dddddd">170</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.59</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">py</td><td class="data-frame-td" nowrap bgcolor="#ffffff">68</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.43</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">jl</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">8</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.17</td></tr>
</table>

Roughly 70% of the articles have Stata `do` files and a quarter Matlab `m` files and only 3.6% R files.

While R, Python and Julia increased their share over recent years, it seems not like a very strong trend yet.

```r
sum_dat = fs %>% 
  left_join(select(articles, year, id), by="id") %>%
  group_by(year) %>%
  mutate(n_art_year = n()) %>%
  group_by(year, file_type) %>%
  summarize(
    count = n(),
    share=round((count / first(n_art_year))*100,2)
  ) %>%
  filter(file_type %in% c("do","r","py","jl","m")) %>%
  arrange(year,desc(share))  

library(ggplot2)
ggplot(sum_dat, aes(x=year, y=share, color=file_type)) +
  geom_line(size=1.5) + scale_y_log10() + theme_bw()
```

<img src="http://skranz.github.io/images/prog_shares3.svg" style="max-width: 100%;">

I also have a log file that anonymously stores data about which articles that have been clicked on. The code below shows the 20 most clicked on articles so far:

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
## # A tibble: 2,707 x 2
##    article                                                                 count
##    <fct>                                                                   <int>
##  1 Consumer Spending during Unemployment: Positive and Normative Implicat~    50
##  2 Do Expert Reviews Affect the Demand for Wine?                              44
##  3 Tax Evasion and Inequality                                                 38
##  4 A Macroeconomic Model of Price Swings in the Housing Market                35
##  5 Is Your Lawyer a Lemon? Incentives and Selection in the Public Provisi~    33
##  6 The Welfare Effects of Social Media                                        31
##  7 The Rise of Market Power and the Macroeconomic Implications                29
##  8 Carbon Taxes and CO2 Emissions: Sweden as a Case Study                     27
##  9 Public Debt and Low Interest Rates                                         27
## 10 The Sad Truth about Happiness Scales                                       25
## 11 Job Polarization and Jobless Recoveries                                    24
## 12 The New Tools of Monetary Policy                                           24
## 13 Alcohol and Self-Control: A Field Experiment in India                      23
## 14 Disease and Gender Gaps in Human Capital Investment: Evidence from Nig~    23
## 15 Some Causal Effects of an Industrial Policy                                23
## 16 Food Deserts and the Causes of Nutritional Inequality                      22
## 17 Minimum Wage and Real Wage Inequality: Evidence from Pass-Through to R~    22
## 18 The Cost of Reducing Greenhouse Gas Emissions                              22
## 19 Adaptation to Climate Change: Evidence from US Agriculture                 21
## 20 Do Parents Value School Effectiveness?                                     21
## # ... with 2,687 more rows
```

So far there were over 11000 thousand clicks in total. Well, that is almost twice as much as the average number of Google searches in 100 milliseconds ;)
