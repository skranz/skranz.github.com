---
layout: post
title: 'Finding Economic Articles With Data'
cover: 
date:   2019-02-22 9:00:00
categories: r
tags: [R, shiny]
---

In my view, one of the greatest developments during the last decade in economics is that the <a href="https://www.aeaweb.org/journals/" target="_blank">Journals of the American Economic Association</a> and some other leading journals require authors to upload the replication code and data sets of accepted articles.

I wrote a Shiny app that allows to search currently among more than 3000 articles that have an accessible data and code supplement. It can be accessed here:

[http://econ.mathematik.uni-ulm.de:3200/ejd/](http://econ.mathematik.uni-ulm.de:3200/ejd/)

One can perform a keyword search among the abstract and title. The screenshot shows an example:

<img src="http://skranz.github.io/images/search.PNG" style="width: 100%; height: 100%">

<br>

One gets some information about the size of the data files and the used code files. I also tried to find and extract a README file from each supplement. Most README files explain whether all results can be replicated with the provided data sets or whether some results require confidential or proprietary data sets. The link allows you to look at the README without the need to download the whole data set.

The main idea is that such a search function could be helpful for teaching  economics and data science. For example, my students can use the app to find an interesting topic for a Bachelor or Master Thesis in form of an interactive analysis with <a href="https://github.com/skranz/RTutor" target="_blank">RTutor</a>. You could also generate a topic list for a seminar, in which students shall replicate some key findings of a resarch article.

If you want to analyse yourself the collected data underlying the search app, you can download the zipped SQLite databases using the following links:
<ul>
<li>
Main database (should suffices for most analyses):<br><a href="http://econ.mathematik.uni-ulm.de/ejd/articles.zip">http://econ.mathematik.uni-ulm.de/ejd/articles.zip</a>
</li><li>
 Large database containing names and sizes of all files in the data and code supplements:<br><a href="http://econ.mathematik.uni-ulm.de/ejd/articles.zip">http://econ.mathematik.uni-ulm.de/ejd/files.zip</a> 
</li>
</ul>
I try to update the databases regularly.


Below is an example, for a simple analysis based on that databases. First make sure that you download and extract `articles.zip` into your working directory.

We first open a database connection

```r
library(RSQLite)
db = dbConnect(RSQLite::SQLite(),"articles.sqlite")
```

File type conversion between databases and R can sometimes be a bit tedious. For example, SQLite knows no native `Date` or `logical` type. For this reason, I typically use my package <a href="https://github.com/skranz/dbmisc">dbmisc</a> when working with SQLite databases. It allows to specify a database schema as simple yaml file and has a lot of convenience function to retrieve or modify data that automatically use the provided schema. The following code sets the database schema that is provided in the package `EconJournalData`:

```r
library(dbmisc)
db = set.db.schemas(db, schema.file=system.file("schema/articles.yaml", package="EconJournalData"))
```

Of course, for a simple analysis as ours below just using the standard function in the `DBI` package without schemata suffices. But I am just used to working with the `dbmisc` package.

The main information about articles is stored in the table `article`

```r
# Get the first 4 entries of articles as data frame
dbGet(db, "article",n = 4)
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">id</th><th class="data-frame-th">year</th><th class="data-frame-th">date</th><th class="data-frame-th">journ</th><th class="data-frame-th">title</th><th class="data-frame-th">vol</th><th class="data-frame-th">issue</th><th class="data-frame-th">artnum</th><th class="data-frame-th">article_url</th><th class="data-frame-th">has_data</th><th class="data-frame-th">data_url</th><th class="data-frame-th">size</th><th class="data-frame-th">unit</th><th class="data-frame-th">files_txt</th><th class="data-frame-th">downloaded_file</th><th class="data-frame-th">num_authors</th><th class="data-frame-th">file_info_stored</th><th class="data-frame-th">file_info_summarized</th><th class="data-frame-th">abstract</th><th class="data-frame-th">readme_file</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aer_108_11_1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2018</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2018-11-01</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aer</td><td class="data-frame-td" nowrap bgcolor="#dddddd">Firm Sorting and Agglomeration</td><td class="data-frame-td" nowrap bgcolor="#dddddd">108</td><td class="data-frame-td" nowrap bgcolor="#dddddd">11</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">https://www.aeaweb.org/articles?id=10.1257/aer.20150361</td><td class="data-frame-td" nowrap bgcolor="#dddddd">TRUE</td><td class="data-frame-td" nowrap bgcolor="#dddddd">https://www.aeaweb.org/doi/10.1257/aer.20150361.data</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.05339</td><td class="data-frame-td" nowrap bgcolor="#dddddd">MB</td><td class="data-frame-td" nowrap bgcolor="#dddddd">NA</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aer_vol_108_issue_11_article_1.zip</td><td class="data-frame-td" nowrap bgcolor="#dddddd">NA</td><td class="data-frame-td" nowrap bgcolor="#dddddd">TRUE</td><td class="data-frame-td" nowrap bgcolor="#dddddd">NA</td><td class="data-frame-td" nowrap bgcolor="#dddddd">Abstract
					To account for the uneven distribution of economic activity in space, I propose a theory of the location choices of heterogeneous firms in a variety of sectors across cities. In equilibrium, the distribution of city sizes and the sorting patterns of firms are uniquely determined and affect aggregate TFP and welfare. I estimate the model using French firm-level data and find that nearly half of the productivity advantage of large cities is due to firm sorting, the rest coming from agglomeration economies. I quantify the general equilibrium effects of place-based policies: policies that subsidize smaller cities have negative aggregate effects.</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aer/2018/aer_108_11_1/READ_ME.pdf</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aer_108_11_2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2018</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2018-11-01</td><td class="data-frame-td" nowrap bgcolor="#ffffff">aer</td><td class="data-frame-td" nowrap bgcolor="#ffffff">Near-Feasible Stable Matchings with Couples</td><td class="data-frame-td" nowrap bgcolor="#ffffff">108</td><td class="data-frame-td" nowrap bgcolor="#ffffff">11</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">https://www.aeaweb.org/articles?id=10.1257/aer.20141188</td><td class="data-frame-td" nowrap bgcolor="#ffffff">TRUE</td><td class="data-frame-td" nowrap bgcolor="#ffffff">https://www.aeaweb.org/doi/10.1257/aer.20141188.data</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.07286</td><td class="data-frame-td" nowrap bgcolor="#ffffff">MB</td><td class="data-frame-td" nowrap bgcolor="#ffffff">NA</td><td class="data-frame-td" nowrap bgcolor="#ffffff">aer_vol_108_issue_11_article_2.zip</td><td class="data-frame-td" nowrap bgcolor="#ffffff">NA</td><td class="data-frame-td" nowrap bgcolor="#ffffff">TRUE</td><td class="data-frame-td" nowrap bgcolor="#ffffff">NA</td><td class="data-frame-td" nowrap bgcolor="#ffffff">Abstract
					The National Resident Matching program seeks a stable matching of medical students to teaching hospitals. With couples, stable matchings need not exist. Nevertheless, for any student preferences, we show that each instance of a matching problem has a "nearby" instance with a stable matching. The nearby instance is obtained by perturbing the capacities of the hospitals. In this perturbation, aggregate capacity is never reduced and can increase by at most four. The capacity of each hospital never changes by more than two.</td><td class="data-frame-td" nowrap bgcolor="#ffffff">aer/2018/aer_108_11_2/Readme.pdf</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aer_108_11_3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2018</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2018-11-01</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aer</td><td class="data-frame-td" nowrap bgcolor="#dddddd">The Costs of Patronage: Evidence from the British Empire</td><td class="data-frame-td" nowrap bgcolor="#dddddd">108</td><td class="data-frame-td" nowrap bgcolor="#dddddd">11</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">https://www.aeaweb.org/articles?id=10.1257/aer.20171339</td><td class="data-frame-td" nowrap bgcolor="#dddddd">TRUE</td><td class="data-frame-td" nowrap bgcolor="#dddddd">https://www.aeaweb.org/doi/10.1257/aer.20171339.data</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.44938</td><td class="data-frame-td" nowrap bgcolor="#dddddd">MB</td><td class="data-frame-td" nowrap bgcolor="#dddddd">NA</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aer_vol_108_issue_11_article_3.zip</td><td class="data-frame-td" nowrap bgcolor="#dddddd">NA</td><td class="data-frame-td" nowrap bgcolor="#dddddd">TRUE</td><td class="data-frame-td" nowrap bgcolor="#dddddd">NA</td><td class="data-frame-td" nowrap bgcolor="#dddddd">Abstract
					I combine newly digitized personnel and public finance data from the British colonial administration for the period 1854-1966 to study how patronage affects the promotion and incentives of governors. Governors are more likely to be promoted to higher salaried colonies when connected to their superior during the period of patronage. Once allocated, they provide more tax exemptions, raise less revenue, and invest less. The promotion and performance gaps disappear after the abolition of patronage appointments. Patronage therefore distorts the allocation of public sector positions and reduces the incentives of favored bureaucrats to perform.</td><td class="data-frame-td" nowrap bgcolor="#dddddd">aer/2018/aer_108_11_3/Readme.pdf</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">aer_108_11_4</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">2018</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">2018-11-01</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">aer</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">The Logic of Insurgent Electoral Violence</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">108</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">11</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">4</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">https://www.aeaweb.org/articles?id=10.1257/aer.20170416</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">TRUE</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">https://www.aeaweb.org/doi/10.1257/aer.20170416.data</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">56</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">MB</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">NA</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">aer_vol_108_issue_11_article_4.zip</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">NA</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">TRUE</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">NA</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">Abstract
					Competitive elections are essential to establishing the political legitimacy of democratizing regimes. We argue that insurgents undermine the state's mandate through electoral violence. We study insurgent violence during elections using newly declassified microdata on the conflict in Afghanistan. Our data track insurgent activity by hour to within meters of attack locations. Our results 

suggest that insurgents carefully calibrate their production of violence during elections to avoid harming civilians. Leveraging a novel instrumental variables approach, we find that violence depresses voting. Collectively, the results suggest insurgents try to depress turnout while avoiding backlash from harming civilians. Counterfactual exercises provide potentially actionable insights 

for safeguarding at-risk elections and enhancing electoral legitimacy in emerging democracies.</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">aer/2018/aer_108_11_4/READ_ME.pdf</td></tr>
</table>

The table `files_summary` contains information about code, data and archive files for each article

```r
dbGet(db, "files_summary",n = 6)
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">id</th><th class="data-frame-th">file_type</th><th class="data-frame-th">num_files</th><th class="data-frame-th">mb</th><th class="data-frame-th">is_code</th><th class="data-frame-th">is_data</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp_1_1_10</td><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.009427</td><td class="data-frame-td" nowrap bgcolor="#dddddd">TRUE</td><td class="data-frame-td" nowrap bgcolor="#dddddd">FALSE</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejapp_1_1_10</td><td class="data-frame-td" nowrap bgcolor="#ffffff">dta</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.100694</td><td class="data-frame-td" nowrap bgcolor="#ffffff">FALSE</td><td class="data-frame-td" nowrap bgcolor="#ffffff">TRUE</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp_1_1_3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">19</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.103628</td><td class="data-frame-td" nowrap bgcolor="#dddddd">TRUE</td><td class="data-frame-td" nowrap bgcolor="#dddddd">FALSE</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">aejapp_1_1_4</td><td class="data-frame-td" nowrap bgcolor="#ffffff">csv</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.024872</td><td class="data-frame-td" nowrap bgcolor="#ffffff">FALSE</td><td class="data-frame-td" nowrap bgcolor="#ffffff">TRUE</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">aejapp_1_1_4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">dat</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">7.15491</td><td class="data-frame-td" nowrap bgcolor="#dddddd">FALSE</td><td class="data-frame-td" nowrap bgcolor="#dddddd">TRUE</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">aejapp_1_1_4</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">do</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">9</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">0.121618</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">TRUE</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">FALSE</td></tr>
</table>

Let us now analyse which share of articles uses Stata, R, Python, Matlab or Julia and how the usage has developed over time.

Since our datasets are small, we can just download the two tables and work with `dplyr` in memory. Alternatively, you could use some SQL commands or work with `dplyr` on the database connection.


```r
articles = dbGet(db,"article")
fs = dbGet(db,"files_summary")
```

Let us now compute the shares of articles that have one of the file types, we are interested in

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
<tr><th class="data-frame-th">file_type</th><th class="data-frame-th">count</th><th class="data-frame-th">share</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2606</td><td class="data-frame-td" nowrap bgcolor="#dddddd">70.55</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">m</td><td class="data-frame-td" nowrap bgcolor="#ffffff">852</td><td class="data-frame-td" nowrap bgcolor="#ffffff">23.06</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">r</td><td class="data-frame-td" nowrap bgcolor="#dddddd">105</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.84</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">py</td><td class="data-frame-td" nowrap bgcolor="#ffffff">32</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.87</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">jl</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">2</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.05</td></tr>
</table>

Roughly 70% of the articles have Stata `do` files and almost a quarter Matlab `m` files. Using Open Source statistical Software seems not yet very popular among economists, less than 3% of articles have R code files, Python is below 1% and only 2 articles have Julia code.

This dominance of Stata in economics never ceases to surprise me, in particular when for some reason I just happened to open the Stata do file editor and compare it with RStudio... But then, I am not an expert in writing empirical economic research papers - I just like R programming and rather passively consume empirical research. For writing empirical papers it probably *is* convenient that in Stata you can add an `robust` or `robust cluster` option to almost every type of regression in order to quickly get the economists' standard standard errors...

For a teaching empirical economics with R the dominance of Stata is not neccessarily bad news. It means that there are a lot of studies which students can replicate in R. Such replication would be considerably less interesting if the original code of the articles would already be given in R.

Let us finish by having a look at the development over time...

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
head(sum_dat)
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">year</th><th class="data-frame-th">file_type</th><th class="data-frame-th">count</th><th class="data-frame-th">share</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">2005</td><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">25</td><td class="data-frame-td" nowrap bgcolor="#dddddd">22.12</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">2005</td><td class="data-frame-td" nowrap bgcolor="#ffffff">m</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10</td><td class="data-frame-td" nowrap bgcolor="#ffffff">8.85</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">2006</td><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">24</td><td class="data-frame-td" nowrap bgcolor="#dddddd">20.87</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">2006</td><td class="data-frame-td" nowrap bgcolor="#ffffff">m</td><td class="data-frame-td" nowrap bgcolor="#ffffff">13</td><td class="data-frame-td" nowrap bgcolor="#ffffff">11.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">2007</td><td class="data-frame-td" nowrap bgcolor="#dddddd">do</td><td class="data-frame-td" nowrap bgcolor="#dddddd">24</td><td class="data-frame-td" nowrap bgcolor="#dddddd">19.35</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">2007</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">m</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">16</td><td class="data-frame-td-bottom" nowrap bgcolor="#ffffff">12.9</td></tr>
</table>

```r
library(ggplot2)
ggplot(sum_dat, aes(x=year, y=share, color=file_type)) + geom_line(size=1.5) + scale_y_log10() + theme_bw()
```

![](FindingEconomicArticles_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

Well, maybe there is a little upward trend for the open source languages, but not too much seems to have happened over time so far...
