---
layout: post
title: "Post election update: Will the German parliament have a gigantic size?"
cover: 
date: 2021-09-27 10:20:00
categories: r
tags: [R, RTutor, shiny]
---


This is just a short update to [my previous post](http://skranz.github.io/r/2021/09/24/bundestag2021.html). Bundestag elections are over and according to the [preliminary official results](https://www.bundeswahlleiter.de/bundestagswahlen/2021/ergebnisse.html) the new Bundestag will have 735 members. That's 32 more than the previous 703 members, but at least far from 841 members predicted in [my previous post](http://skranz.github.io/r/2021/09/24/bundestag2021.html) that was based on last week's forecast data.

The main reason is that the Bavarian CSU performed substantially better in terms of 2nd votes than predicted by the Forsa forecast from last week. While the forecast predicted a CSU 2nd vote share of 29.3% in Bavaria among the parties  entering the parliament, the CSU actually achieved 36.8%.

### Election results 

I copied the preliminary results from [mandatsrechner.de](https://www.mandatsrechner.de) and added them to my [Github repository](https://github.com/skranz/seat_calculator_bundestag).

Let's see whether we get the same seat distribution as in the [official results](https://www.bundeswahlleiter.de/bundestagswahlen/2021/ergebnisse/bund-99.html):

```r
source("seat_calculator.R")
dat = read.csv("results_2021.csv",encoding="UTF-8") %>%
  select(-seats.mr)

res = compute.seats(dat)
summarize.results(res)
```

```
## Total size:  734  seats
```

```
## # A tibble: 7 x 5
##   party  vote_share seat_shares seats ueberhang
##   <chr>       <dbl>       <dbl> <dbl>     <dbl>
## 1 SPD        0.282       0.281    206         0
## 2 CDU        0.207       0.206    151         0
## 3 Gruene     0.162       0.161    118         0
## 4 FDP        0.126       0.125     92         0
## 5 AfD        0.113       0.113     83         0
## 6 CSU        0.0567      0.0613    45         3
## 7 Linke      0.0536      0.0531    39         0
```

That are the same results as the preliminary official one's, with one exception. As the party of the danish minority party, the SSW did not have to pass the 5% hurdle and also got one seat this election. Adding this seat leads to the total of 735 seats. Also note that all vote shares are only relative to the votes of the parties that enter the Bundestag.


### How 146 voters in Munich generated 17 extra seats in the Bundestag

Except for the district [Munich-South](https://www.bundeswahlleiter.de/bundestagswahlen/2021/ergebnisse/bund-99/land-9/wahlkreis-219.html), which went to the Green party, the CSU won all direct districts in Bavaria. But e.g. the district [Munich-West](https://www.bundeswahlleiter.de/bundestagswahlen/2021/ergebnisse/bund-99/land-9/wahlkreis-220.html) was won by the CSU won with only 146 votes ahead of the 2nd ranked candidate from the Green party. What would be the size of the parliament if the Green would have also won Munich-West?


```r
library(dplyrExtras)
dat.mod = dat %>%
  mutate_rows(party=="CSU" & land=="Bayern", direct = direct-1) %>%
  mutate_rows(party=="Gruene" & land=="Bayern", direct = direct+1)
  
res = compute.seats(dat.mod)
sum(res$seats)+1
```

```
## [1] 718
```

We would have 17 seats less: only 718 seats. Note that in each of the 4 Munich districts the candidates of the SPD and the Green party have together substantially more direct votes than the CSU. If the voters of these two parties would have better coordinated so that the CSU gets no direct seat in Munich, the size of the Bundestag could have been reduced to even 683 members. Normally, one would have thought the Bavarian voters would take such a chance to reduce the amount of money spent in Berlin...

### What if Linke would have gotten only 2 direct seats?

There were even direct seats more expensive in terms of Bundestag size than a direct seat for the CSU: one of the three direct seats captured by the Linke. If the Linke would not have won three direct seats, they would have lost all their 2nd vote seats, because they did not breach the 5% hurdle. The number of seats is not reduced automatically by the fact that a party does not enter the parliament. Yet, if the Linke would not get its second votes, the CSU share among the relevant 2nd votes would increase and a smaller increase of the Bundestag's size would be necessary to balance CSU's direct seats with its 2nd vote share.

So what would be the size of parliament if the Linke only would have gotten only 2 direct seats and one of its Berlin seats went to the Green party?


```r
dat.mod = dat %>%
  filter(party != "Linke") %>%
  mutate_rows(party=="Gruene" & land=="Berlin", direct = direct+1)
res = compute.seats(dat.mod)
sum(res$seats)+3
```

```
## [1] 698
```

We then would have a parliament of only 698 members. So the third direct seat won by the Linke generated 37 additional seats in the Bundestag.


