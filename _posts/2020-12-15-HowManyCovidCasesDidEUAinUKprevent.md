---
layout: post
title: "How many Covid cases and deaths did UK's fast vaccine authorization save?"
cover: 
date: 2020-12-15 18:00:00
categories: r
tags: [R, Covid]
---


The UK approved on December 2nd, as first country in the world, the emergency use authorization (EUA) of Biontech/Pfizer's Covid-19 vaccination. A few days later on December 8th, the first NHS patient became vaccinated.

Neither the European Union nor any of its member states chose an EUA but rather opted to wait for the decision on a more extensive [conditional marketing authorization](https://ec.europa.eu/commission/presscorner/detail/en/QANDA_20_2390), which requires more thorough, more time consuming investigations. Most news currently report that vaccinations are estimated to start in the EU in the beginning of January, so roughly one month later than in the UK. 

Note that also the UK must still adhere in 2020 to EU regulations but EU law does not forbid member states to grant an EUA (even though it seems politically discouraged). So it is not true that the EUA for the UK was only possible because of the Brexit.

The choice between an earlier EUA vs a later approval is a trade-off between risks and benefits. Two measures of benefit are the expected numbers of avoided Covid-19 cases and deaths. Nobody knows these numbers, but in this post I try to make some very rough estimations.

There are many data sources for confirmed Covid-19 cases. One popular source are [the statistics compiled by the John Hopkins University](https://github.com/CSSEGISandData/COVID-19). The same data is accessible in a more tidy format on the [Github page](https://github.com/RamiKrispin/coronavirus/tree/master/csv) of the R package `coronavirus`.

The following code downloads the data file:


```r
download.file(url =
  "https://raw.githubusercontent.com/RamiKrispin/coronavirus/master/csv/coronavirus.csv",
  "jhu_covid.csv"
)
```

We now load the downloaded data set, extract the UK data, put it in a wide format with separate columns for the counts of new cases and deaths, and plot the times series of new cases:


```r
library(dplyr)
dat = rio::import("jhu_covid.csv") %>%
  filter(country == "United Kingdom", province=="") %>%
  select(date, type, cases) %>%
  tidyr::pivot_wider(names_from="type", values_from="cases") %>%
  rename(newcases = confirmed)

library(ggplot2)
ggplot(dat, aes(x=date,y=newcases)) + geom_line(col="blue") +
  ggtitle("Daily confirmed new Covid-19 cases (UK) ")
```

<center>
<img src="http://skranz.github.io/images/covid/cases_uk.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

The data shows the officially confirmed number of cases. The true number of cases is larger because not every Covid-19 case is detected. For example, the much larger case number in autumn compared to spring reflects to a large extend the larger number of tests.

We first want to make a rough guess about the daily probability to get infected with Covid-19. The lower the assumed infection probability, the lower will be the number of cases prevented by an earlier start of vaccinations. We will just take the number of *confirmed* cases, which should yield a conservative, low estimate of the infection probability. As an approximation for the relevant daily cases, let us take the mean daily cases for the first days in December contained in our data set:


```r
dec = filter(dat, date >="2020-12-01")
NROW(dec)
```

```
## [1] 13
```

```r
daily.cases = mean(dec$newcases)
daily.cases
```

```
## [1] 16903.54
```

As a very rough approximation for infection probability, we divide this daily confirmed case number by the UK population that did not yet have a confirmed Corona infection before first of December:


```r
UK.pop = 67e6 # roughly 67 million
sumcases = sum(dat$newcases[dat$date < "2020-12-01"])
sumcases
```

```
## [1] 1629657
```

```r
daily.infect.prob = daily.cases / (UK.pop-sumcases)
round(daily.infect.prob*100,2)
```

```
## [1] 0.03
```

This means our estimate for the daily infection probability is 0.03%. Of course, that is a very rough approximation. For example, for me it is not clear whether the vaccination recipients would have on average a higher or lower infection probability than the average UK population.

To estimate how many Covid-19 cases are directly prevented due to the earlier vaccination, we need to know how many persons were how much earlier vaccinated.

The UK got 800000 vaccination dosages for the year 2020. Given that there are two vaccination shots per person, this allows 400000 early vaccinations. Without emergency use authorization the UK probably would have waited until the EMA approved the vaccine. Let us assume this will happen on December 29th, which is the most common fluctuated date (I still hope it is earlier though.). This would be 27 days later than the UK authorization. 

Of course, it takes some time until all 400000 people are vaccinated, in particular the 2nd shot is 21 days after the first. However, given that it is plausible that all 400000 persons can at least get their first shot within 27 days, it seems not unreasonable to assume that the time path of the first 400000 vaccinations would be simply shifted by 27 days if the approval would take place 27 days later. OK, maybe the vaccines could have been quicker transported to the UK at a later approval date. So let's say we could save the 6 days from approval to the first vaccine shot on December 8th and assume that the UK EUA allowed each of the 400000 people to be vaccinated 21 days earlier.

Let us assume a vaccine efficacy of 95% as reported in the phase III trial results. (If you want to know how Biontech/Pfizer computed Bayesian credible interval for the efficacy [see my earlier post](http://skranz.github.io/r/2020/11/11/CovidVaccineBayesian.html).). Then we can approximately compute the number of vaccinated person that did not get Covid-19 because of their vaccination in these 21 days as follows:


```r
directly.prevented.cases = 
  21*400000*0.95*daily.infect.prob
directly.prevented.cases
```

```
## [1] 2063.478
```

So this rather conservative estimate states that 2063 Covid-19 diseases among the early vaccinated people are prevented by the EUA. 

### Indirectly prevented cases

The number above would be the relevant number if we assume that the vaccine only protects against the disease but the vaccinated people are as infectious as unvaccinated people.

On the other extreme, we could assume that vaccinated people who don't fall sick don't infect other people at all. Then the famous [R value](https://www.nature.com/articles/d41586-020-02009-w) tells us how many other persons one Covid-19 case is assumed to directly infect. [Goverment estimates](https://www.gov.uk/guidance/the-r-number-in-the-uk) on December 11th put the UK R number between 0.9 and 1. Let's again be conservative and take the lower bound of 0.9. This would mean the directly prevented cases from the vaccine would prevent additional


```r
R = 0.9
R*directly.prevented.cases
```

```
## [1] 1857.13
```

next round cases. If R would stay constant, those cases would have infected another 


```r
R*R*directly.prevented.cases
```

```
## [1] 1671.417
```

cases and so on. Of course, as the vaccination proceeds and winter passes by, we should expect R to decrease. The exact future time path of R is hard to predict, though.

We also need to make a guess of how many days it takes on average for a Covid-19 infected to infect another person. The incubation period for Covid-19, i.e. time between infection and first symptoms is [estimated to be on average between 5 and 6 days.](https://www.who.int/news-room/commentaries/detail/transmission-of-sars-cov-2-implications-for-infection-prevention-precautions#:~:text=The%20incubation%20period%20of%20COVID,to%20a%20confirmed%20case.) and [this study](https://www.thelancet.com/pdfs/journals/lanmic/PIIS2666-5247(20)30172-5.pdf) was [interpreted](https://www.euronews.com/2020/11/20/coronavirus-new-research-shows-infected-people-most-contagious-in-first-5-days) such that people are most contagious the first 5 days after showing symptoms. So let us just say that one average it takes 10 days time for an infected person to infect another person.   

Let us also just assume there are 15 infection rounds (150 days, i.e. 5 months) during which the R value thanks to new vaccinations and better weather conditions linearly decreases from 0.9 to 0.

The following code computes under those assumptions the total number of prevented cases.

```r
# Sequence of R over 15 infection rounds
# assumed to linearly decrease
R.seq = seq(0.9,0, length.out = 15)
R.seq
```

```
##  [1] 0.90000000 0.83571429 0.77142857 0.70714286 0.64285714 0.57857143
##  [7] 0.51428571 0.45000000 0.38571429 0.32142857 0.25714286 0.19285714
## [13] 0.12857143 0.06428571 0.00000000
```

```r
# To compute the prevented Covid-19 cases in 
# infection round k
# we need to multiply all R up to k
cumprod(R.seq)
```

```
##  [1] 9.000000e-01 7.521429e-01 5.802245e-01 4.103016e-01 2.637653e-01
##  [6] 1.526071e-01 7.848364e-02 3.531764e-02 1.362252e-02 4.378666e-03
## [11] 1.125943e-03 2.171461e-04 2.791878e-05 1.794779e-06 0.000000e+00
```

```r
# Summing yields factor for all indirectly prevented cases
sum(cumprod(R.seq))
```

```
## [1] 3.192217
```

```r
# The total factor by which we multiply
# the directly prevented cases
factor = 1+sum(cumprod(R.seq))
factor
```

```
## [1] 4.192217
```

```r
# Total estimated number of prevented Covid-19 cases
directly.prevented.cases*factor
```

```
## [1] 8650.545
```

So our crude and often conservative assumptions would predict that the earlier vaccinations may have prevented between 2063 (if vaccine does not reduce infectiousness) and 8651 (if infectiousness is also reduced by 95%) Covid-19 cases. Before discussing the results, let us look at the number of possibly prevented Covid-19 deaths. 

## Estimating the number of prevented Covid-19 deaths


The following plot shows the reported number of daily UK deaths ascribed to Covid-19 from the JHU data: 


```r
ggplot(dat, aes(x=date,y=death)) + geom_line(col="black") +
  ggtitle("Daily Covid-19 deaths (UK)")
```

<center>
<img src="http://skranz.github.io/images/covid/deaths_uk.svg" style="max-width: 100%; margin-bottom: 0.5em;">
</center>

To compute the number of Covid-19 deaths prevented from the early EUA vaccinations, we proceed in similar steps than for the Covid-19 cases. We first approximate the number of daily deaths by the average reported number for the days in December 2020:


```r
daily.deaths = mean(dec$death)
daily.deaths
```

```
## [1] 440.1538
```

To compute the number of directly prevented deaths, we first estimate the daily death probability for inhabitants that not yet have had a confirmed Covid-19 case. We then multiply by it by the number of early vaccinated persons, the 21 days head-start and the assumed vaccine efficiency of 95%:


```r
daily.death.prob = daily.deaths / (UK.pop-sumcases)
directly.prevented.deaths = 
  21*400000*0.95*daily.death.prob
directly.prevented.deaths
```

```
## [1] 53.73121
```

So we estimate that making the first 400000 vaccinations 21 days earlier, 54 Covid-19 deaths were directly prevented among the vaccinated persons.

There are arguments why this number may be too large or too small, however.

### Why the estimated number of directly prevented deaths may be too large

One problem with the death data is that it is hard to distinguish between people who died *because* of Covid-19 from people who just died *with* Covid-19 but would also have died without the infection. In this aspect a less biased measure is the so called [excess mortality](https://ourworldindata.org/excess-mortality-covid) that compares the total deaths in a particular week of year to the numbers in previous years. I downloaded the [excess mortality data from Our World in Data](https://ourworldindata.org/excess-mortality-covid#excess-mortality-using-raw-death-counts) and compute the average daily excess mortality in November (December data was not yet available) with the following code:


```r
em = rio::import("excess-mortality-raw-death-count.csv") %>%
  filter(Entity %in% c("England & Wales","Scotland","Northern Ireland"))

# Rename columns and compute excess mortality
em = em[,c(1,3:5)]
colnames(em)[3:4]=c("deaths2020","deaths2015_2019")
em$excess_mortality = em$deaths2020-em$deaths2015_2019

# Aggregate over "England & Wales","Scotland","Northern Ireland"
uk.em = em %>%
  group_by(Date) %>%
  summarize(
    excess_mortality = sum(excess_mortality)
  ) %>%
  na.omit()

# Keep data after November 1st
uk.em.nov = uk.em[uk.em$Date >= "2020-11-01",]
uk.em.nov
```

```
## # A tibble: 5 x 2
##   Date       excess_mortality
##   <date>                <dbl>
## 1 2020-11-01            1267.
## 2 2020-11-08            1723.
## 3 2020-11-15            2197.
## 4 2020-11-22            2474.
## 5 2020-11-29            2334.
```

```r
# Compute daily excess mortality
daily.excess.mortality = mean(uk.em.nov$excess_mortality)/7
daily.excess.mortality
```

```
## [1] 285.5429
```

These 286 daily deaths based on excess mortality are only around 65% of the daily Covid-19 death count (440) from the John Hopkins data.

### Why the estimated number of directly prevented deaths may be too small

On the other hand, our estimate of 54 directly prevented deaths may also well be too small. Our computation estimates the average death probability for all age and risk groups. Yet, besides health-care workers, the early vaccinations were mostly given to the elderly and other high risk groups. For example, [this study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7518649/) with Italian data suggests that the case fatality rate for the elderly with age above 80 is more than 10 times larger than for the age group between 50 and 59 and roughly a 100 times larger than for the age group between 30 and 39. 

These factors suggests that the targeted early vaccinations may have directly prevented considerably more deaths than our estimated 54, even if we would use the lower excess mortality estimates. 

I would therefore consider 54 direct deaths a rather conservative estimate.

### Indirectly prevented deaths

For the case that the vaccine prevents infectiousness as efficiently as it prevents disease, we estimate the total prevented deaths including the prevented deaths from subsequent infection rounds in the same fashion as for the total prevented cases:  


```r
prevented.deaths = directly.prevented.deaths*factor
prevented.deaths
```

```
## [1] 225.2529
```

## Discussion and Opinion

Our estimates are between 54 and 225 prevented Covid-19 deaths due to the early vaccination. While it is less than the average UK Covid-19 deaths on an early December day, it still a considerable number.

Does this expected number of avoided deaths and the larger expected number of avoided cases outweigh the risks from an earlier EUA? As far I have followed the available information about possible risks and side effects, my *non-expert opinion* would be a *clear yes*.

We certainly cannot rule out the possibility of rare, very severe side effects. For example, two vaccinated NHS workers had strong allergic reactions. Some news reports called the incidents *life-threatening* but this [BBC report](https://www.bbc.com/news/health-55244122) states

> They are understood to have had an anaphylactoid reaction, which tends to involve a skin rash, breathlessness and sometimes a drop in blood pressure. This is not the same as anaphylaxis which can be fatal.

Some commentators argue that it was situations like these that the EU wanted to avoid by more thorough investigations. But even if the two allergic reactions would have been life threatening, why should we give them so much, much more weight than at least likely 54 prevented deaths from starting the vaccination earlier? Also, it is not clear how more thorough investigations would have found out about these allergic reactions. No very strong reactions seem to have occurred in the roughly 22000 subjects that got the vaccine in the phase III trial. 

Of course, if the politicians of the EU member states coordinated on the more time consuming conditional marketing authorization instead of a faster EUA, the EMA has to follow this procedure.

I would rather argue that it might have been the wrong political decision by the leaders of EU member states and other involved parties to avoid an EUA. At least it seems wrong for me from the point of view of a benefit-risk analysis of lives saved. But I may have overlooked crucial information.

Personally, I think the problem is that we never know concretely whose deaths or severe Covid-19 cases are prevented by an earlier vaccination authorization. It is hard to write news reports about anonymous estimated numbers. In contrast, if there are side effects of the vaccinations, we will know exactly who was harmed. My guess is that this causes a massive bias towards weighing the risks excessively high compared to the benefits of early vaccination.

In my view, one should have the following scenario in mind when weighing the risks and benefits. Assume we would already know which 400000 people would get the earlier vaccination and we would track their health status no matter whether we allow the earlier vaccination or not. In particularly, if we don't make an EUA, we track which people caught Covid-19 in the period were they would have been vaccinated under an early EUA and which of these people died. Then we would have concrete faces of people who likely have died because of a later start of the vaccination. Personally, I think under such a scenario we would be much more inclined to give more equal weight to the likely lives saved from an EUA as to the likely lives risked by it. 

Then, given that 22000 participants in the phase 3 trials got the vaccination without severe side effects and my (arguably rough) estimates about lives saved, I don't see how we would conclude that overall it is better not to make a fast EUA.

One political argument may be that one needs to ensure the public, in particular vaccine critics, that the vaccine is really save and that this assurance has higher priorities than lives saves from an EUA. That may well be reasonably if we would force people to start taking the vaccine. However, taking the vaccine is voluntary and there are many people who are willing to be first in line. Also, if you are a rational person who still is skeptical about the vaccine, you should prefer that more people take the vaccine before you, because that allows us to learn better about possible rare side effects. Would the UK not have given the early EUA, we would not have known about the possibly strong allergic reactions. So also a rational vaccine skeptic should prefer that volunteers can get more early vaccination. This can be seen as additional testing for side effects.

Perhaps supporting this argument, [this poll](https://www.axios.com/axios-ipsos-poll-vaccine-enthusiasm-813a9a92-35bb-4952-bedf-7b9477a1744c.html) suggests that the willingness of US citizens to get an early vaccination has substantially increased after the first UK and US vaccinations happened.

Of course, there are probably irrational vaccine skeptics who digest information differently. But should some unclear psychological concern for irrational information processing lead us to forbid volunteers to take existing, live-saving vaccines already early after the phase III trials concluded?

I really hope that also the EU will speed up vaccine authorization and are a little bit sad again that the UK has left us. While I am not a fan of populist prime ministers (or presidents), I feel that the UK often had a positive influence on the EU towards a more Utilitarian weighing of risks and benefits.
