---
layout: post
title: 'Usage shares of programming languages in economics research'
cover: 
date:   2023-12-29 09:00
categories: r
tags: [R]
---

My shiny app *Finding Economics Articles with Data* contains meanwhile over 8000 economic articles with replication packages. You can use it here:
[https://ejd.econ.mathematik.uni-ulm.de](https://ejd.econ.mathematik.uni-ulm.de)

Some of the data on articles and file types in the reproduction packages can be downloaded as a zipped SQLite database from my server (see the "About" page in the app for the link). Let us use the database to take a look at the usage shares of different programming languages.

The following code extracts our data set by merging two tables from the data base.

```r
library(RSQLite)
library(dbmisc)
library(dplyr)

# Open data base using schemas as defined in my dbmisc
# package
db = dbConnect(RSQLite::SQLite(),"articles.sqlite")

articles = dbGet(db,"article")
fs = dbGet(db,"files_summary") 
fs = fs %>% 
  left_join(select(articles, year, journ, id), by="id")
head(fs)
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 table.data-frame-table tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">id</th><th class="data-frame-th">file_type</th><th class="data-frame-th">num_files</th><th class="data-frame-th">mb</th><th class="data-frame-th">is_code</th><th class="data-frame-th">is_data</th><th class="data-frame-th">year</th><th class="data-frame-th">journ</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp_10_4_5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">csv</td><td class="data-frame-td" nowrap bgcolor="#dddddd">9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">6.49858</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2018</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejapp_10_4_5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">do</td><td class="data-frame-td" nowrap bgcolor="#ffffff">19</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.169755</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2018</td><td class="data-frame-td" nowrap bgcolor="#ffffff">aejapp</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp_10_4_5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">dta</td><td class="data-frame-td" nowrap bgcolor="#dddddd">207</td><td class="data-frame-td" nowrap bgcolor="#dddddd">19918.231</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2018</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejpol_10_4_8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">csv</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.110033</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2018</td><td class="data-frame-td" nowrap bgcolor="#ffffff">aejpol</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejpol_10_4_8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">18</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.118644</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2018</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aejpol</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">aejpol_10_4_8</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">gz</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">1</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">4294.9673</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">0</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">0</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">2018</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">aejpol</td></tr>
</table>

The data frame `fs` contains for each article and corresponding reproduction packages counts for common data or code files. 

Let us take a look at the total number of reproduction packages and then compute the shares of reproduction packages that contain at least one file of specific programming languages (I am aware that not everybody would call e.g. Stata a programming language. Just feel free to replace the term by your favorite expression like *scripting language* or *statistical software*.):

```r
n_art = n_distinct(fs$id)
n_art
```

```
## [1] 8262
```

```r
fs %>% 
  group_by(file_type) %>%
  summarize(
    count = n(),
    share=round((count / n_art)*100,1)
  ) %>%
  # note that all file extensions are stored in lower case
  filter(file_type %in% c("do","r","py","jl","m","java","c","cpp","nb","f90","f95", "sas","mod","js","g","gms","ztt")) %>%
  arrange(desc(share))
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 table.data-frame-table tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">file_type</th><th class="data-frame-th">count</th><th class="data-frame-th">share</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5915</td><td class="data-frame-td" nowrap bgcolor="#dddddd">71.6</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">m</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2023</td><td class="data-frame-td" nowrap bgcolor="#ffffff">24.5</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">r</td><td class="data-frame-td" nowrap bgcolor="#dddddd">808</td><td class="data-frame-td" nowrap bgcolor="#dddddd">9.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">sas</td><td class="data-frame-td" nowrap bgcolor="#ffffff">349</td><td class="data-frame-td" nowrap bgcolor="#ffffff">4.2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">py</td><td class="data-frame-td" nowrap bgcolor="#dddddd">341</td><td class="data-frame-td" nowrap bgcolor="#dddddd">4.1</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">mod</td><td class="data-frame-td" nowrap bgcolor="#ffffff">198</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.4</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">f90</td><td class="data-frame-td" nowrap bgcolor="#dddddd">188</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">nb</td><td class="data-frame-td" nowrap bgcolor="#ffffff">116</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.4</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">c</td><td class="data-frame-td" nowrap bgcolor="#dddddd">105</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">ztt</td><td class="data-frame-td" nowrap bgcolor="#ffffff">104</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">cpp</td><td class="data-frame-td" nowrap bgcolor="#dddddd">66</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">jl</td><td class="data-frame-td" nowrap bgcolor="#ffffff">39</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.5</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">java</td><td class="data-frame-td" nowrap bgcolor="#dddddd">33</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.4</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">g</td><td class="data-frame-td" nowrap bgcolor="#ffffff">28</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">gms</td><td class="data-frame-td" nowrap bgcolor="#dddddd">19</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">js</td><td class="data-frame-td" nowrap bgcolor="#ffffff">18</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.2</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">f95</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">7</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.1</td></tr>
</table>

The most used software is by a far margin Stata, whose `.do` scripts can be found in 71.6% of reproduction packages. It follows Matlab with 24.5%. The most popular open source language is R with 9.8%. After one more proprietary software SAS, Python then follows as second most most used open source language with 4.1%. If you wonder why the shares add up to more than 100%: some reproduction packages simply use more than one language.

Let us take a look at the development over time for Stata, Matlab, R and Python.

```r
year_dat = fs %>%
  filter(year >= 2010) %>%
  group_by(year) %>%
  mutate(n_art_year = n_distinct(id)) %>%
  group_by(year, file_type) %>%
  summarize(
    count = n(),
    share=count / first(n_art_year),
    # Compute approximate 95% CI of proportion
    se = sqrt(share*(1-share)/first(n_art_year)),
    ci_up = share + 1.96*se,
    ci_low = share - 1.96*se
  ) %>%
  filter(file_type %in% c("do","r","py","m")) %>%
  arrange(year,desc(share))  

library(ggplot2)
ggplot(year_dat, aes(x=year, y=share,ymin=ci_low, ymax=ci_up, color=file_type)) +
  facet_wrap(~file_type) +
  geom_ribbon(fill="#000000", colour = NA, alpha=0.1) +
  geom_line() +
  theme_bw()
```

<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/ejd/lang_shares-1.svg" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

The usage share of Stata and Matlab stays relatively constant over time. Yet, we still see a substantial increase in R usage from 1.4% in 2010 to over 20% in 2023. Also Python usage increases: from 0.4% in 2010 to almost 10% in 2023.

So open source software is getting more popular in academic economic research with large growth rates but absolute usage levels that are still substantially below Stata usage.

Note that the representation of journals is not balanced across years in our data base. E.g. the first reproduction package from Management Science in our data base is from 2019. 
To check whether the growth of R usage can also be found within journals, let us look at the development of its usage share within journals:

```r
year_journ_dat = fs %>%
  filter(year >= 2010) %>%
  group_by(year, journ) %>%
  mutate(n_art = n_distinct(id)) %>%
  group_by(year, journ, file_type) %>%
  summarize(
    count = n(),
    share=count / first(n_art),
    # Compute approximate 95% CI of proportion
    se = sqrt(share*(1-share)/first(n_art)),
    ci_up = share + 1.96*se,
    ci_low = share - 1.96*se

  )
ggplot(year_journ_dat %>% filter(file_type=="r"),
  aes(x=year, y=share,ymin=ci_low, ymax=ci_up)) +
  facet_wrap(~ journ, scales = "free_y") +
  geom_ribbon(fill="#000000", colour = NA, alpha=0.1) +
  geom_line() +
  coord_cartesian(ylim = c(0, 0.4)) +
  ylab("") +
  ggtitle("Share of replication packages using R")+
  theme_bw()
```

<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/ejd/r_shares-1.svg" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

We see a substantial increase in R usage in most journals. Finally, let us take a similar look at the time trends of Stata usage within journals.

```r
ggplot(year_journ_dat %>% filter(file_type=="do"),
  aes(x=year, y=share,ymin=ci_low, ymax=ci_up)) +
  facet_wrap(~ journ, scales = "free_y") +
  geom_ribbon(fill="#000000", colour = NA, alpha=0.1) +
  geom_line() +
  coord_cartesian(ylim = c(0, 1)) +
  ylab("") +
  ggtitle("Share of replication packages using Stata")+
  theme_bw()
```
<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/ejd/stata_shares-1.svg" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

