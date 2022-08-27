---
layout: post
title: "Natural Gas Prices for main Hubs in Europe, Asia, and US"
cover: 
date:   2022-08-27 12:40:00
categories: r
tags: [R, energy]
output: 
  html_document: 
    keep_md: yes
---



Are spot market natural gas prices in Europe mainly determined by the available world-wide LNG supply or does a bottleneck in the number of usable European LNG terminals increase prices to even substantially higher levels? To get some insight into this question, I added to the European data from [my previous post](http://skranz.github.io/r/2022/08/23/ElectricityPrices.html) also natural gas prices from Japan  & Korea, as well as the US. The data sources are given in the appendix and my generated data set can be downloaded [here](https://github.com/skranz/skranz.github.com/blob/master/data/daily_prices.Rds).


```r
library(dplyr)
library(tidyverse)

pr.df = readRDS("daily_prices.Rds")

# Transform to weekly prices
wpr.df = pr.df %>%
  mutate(weekind = ceiling((1:n()) / 7)) %>%
  select(date, weekind, everything()) %>%
  group_by(weekind) %>%
  summarize(
    across(co2_eur_t:usd_eur, ~mean(.,na.rm=TRUE)),
    date = first(date)
  )

# Transform into long format for ggplot
dat = wpr.df %>%
  select(date, Europe=gas_eur_mwh, USA=us_gas_eur_mwh, `Japan and Korea`=japan_korea_gas_eur_mwh) %>%
  pivot_longer(2:4,values_to = "gas_price",names_to = "Region") %>%
  filter(date > "2014-01-01") 

library(ggplot2)
ggplot(dat, aes(x=date,y=gas_price, color=Region)) +
  geom_line(size=1) +
  ggtitle("Natural Gas Prices (Main Regional Trading Hubs)") +
  geom_hline(yintercept = 0)+
  ylab("EUR / MWh") + xlab("Year")+
  theme_bw()
```

<center>
<img src="https://skranz.github.io/images/gas_price_by_region-1.svg" style="max-width: 97%;margin-bottom: 0.5em;">
<br>
</center>

We see that natural gas prices in Japan and Korea move similar to the prices in Europe. But there is also some gap that could be due to limited LNG import capacity in Europe. While also in the US prices have somewhat gone up, the price increases are much lower than in Europe and Japan.

I was actually quite surprised that European and Asian natural gas hub prices were so close together from 2014 to 2020. I would have thought that cheap Russian pipeline gas would have been reflected in substantially lower European prices. Did I perhaps make a calculation error when transforming units and currencies? The IEA created a similar plot for a shorter period [here](https://www.iea.org/data-and-statistics/charts/natural-gas-prices-in-europe-asia-and-the-united-states-jan-2020-february-2022). It roughly looks similar. Perhaps, in those years Russian gas was either not as cheap as I thought, or the long term contracts with Gazprom stipulated prices that were substantially below the price level on the Dutch TTF trading hub.

Of course, for a deeper understanding on the factors that determine prices and price differences, one should look at reports by market experts, like [this report](https://www.naturalgasintel.com/latest-european-natural-gas-price-spike-forces-some-to-turn-away-u-s-imports/). 

## Appendix: Data

European prices are based on the Dutch TTF and were collected here:

[https://www.investing.com/commodities/ice-dutch-ttf-gas-c1-futures-historical-data](https://www.investing.com/commodities/ice-dutch-ttf-gas-c1-futures-historical-data)

Japan & Korean prices correspond to JKM futures and were collected here:

[https://www.investing.com/commodities/lng-japan-korea-marker-platts-futures-historical-data](https://www.investing.com/commodities/lng-japan-korea-marker-platts-futures-historical-data)

US prices refer to the Henry Hub and were collected here:

[https://www.investing.com/commodities/natural-gas-historical-data](https://www.investing.com/commodities/natural-gas-historical-data)

Note that all data sets refer to futures instead of spot market prices. I think these are the prices of short term futures (likely delivery in the next month), but I am not 100% sure.

Conversion from USD into EUR was based on this exchange rate data:

[https://www.investing.com/currencies/eur-usd-historical-data](https://www.investing.com/currencies/eur-usd-historical-data)

Transformation from MMBtu to MWh was done using the factor: 0.29307107017222 

