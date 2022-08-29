---
layout: post
title: "A proposal for capping exploding electricity spot market prices without subsidies or supply reduction"
date:   2022-08-29 10:00:00
categories: r
tags: [R, energy]
---

At the EEX, [German baseload electricity futures](https://www.eex.com/en/market-data/power/futures#%7B%22snippetpicker%22%3A%22EEX%20German%20Power%20Future%22%7D) for the year 2023 trade at a price of 950 Euro / MWh and peak load futures at 1275 Euro / MWh. Future prices for France are even higher. (Prices were looked up on 2022-08-28). 

In contrast, average German spot market prices 2015 to 2021 were below 50 Euro / MWh. That market participants seem to expect twenty-fold higher prices in 2023 than the average of those previous years seems really scary. (I deeply hope that at least a lot of firms have hedged their long term electricity demand when future prices were still low).

The current electricity markets operate in their core like other free markets where prices are determined by the intersection between supply and demand. (Electricity spot and future markets are also augmented by markets for balancing energy, some mechanism for redispatch to deal with network constraints, and in some countries capacity mechanisms). And in normal times, this market design seems in many dimensions superior to alternatives. Yet, given the current unforeseen, massive shock, some modifications could turn out helpful to dampen the impact. I know that it seems incredibly hard to implement sensible reforms within a short time frame, but still wanted to write up a proposal.

## Proposal

Here is the sketched proposal, later I show some numbers:

- In the electricity spot market, enforce a price cap for power plants that don't use natural gas. The price cap should be well above marginal cost estimates for those plants. Let's say 300 Euro / MWh in this example. Non-gas plants cannot bid above the price cap and get at most the price cap.

- Gas power plants can bid higher prices in the spot market (with no or a much higher cap). E.g. some gas plants may bid 600 Euro / MWh and others may bid 1000 Euro / MWh.

- If demand meets supply at an equilibrium price below the price cap then buyers simply pay the equilibrium price in that trading period. If at the price cap of 300 Euro / MWh demand still exceeds supply, prices are formed as following. We determine a uniform price that gas power plants receive based on their bids so that in the end demand equals supply. Buyers pay a price equal to the total payments to suppliers divided by the traded quantity (see numerical example below).

## A simple numerical example

For simplicity, we assume the spot market trading is for periods of one hour and hourly demand is in the MWh range instead of the more realistic GWh range.

- Assume we have a fixed demand of 50 MWh and every buyer is willing to bid at most 1500 Euro / MWh.

- Supply by non-gas power plants is 30 MWh: no bid is allowed to exceed the price cap of 300 Euro / MWh.

- CCGT (combined cycle gas turbine) gas power plants offer a supply of 10 MWh at a price of 600 Euro / MWh.

- In addition OCGT (open cycle gas turbine) gas power plants offer a supply of 15 MWh at a price 1000 Euro / MWh.

In the existing market design, the marginal OCGT gas power plant would determine the equilibrium price of 1000 Euro / MWh, causing total expenditures of 50*1000 = 50000 Euro for the buyers.

With my proposal all gas power plants also receive the marginal price of 1000 Euro / MWh, but the other power plants only receive the price cap of 300 Euro / MWh. Buyers have to pay in total 

30 * 300 + 20 * 1000 = 29000 Euro

which corresponds to a price of 29000 / 50 = 580 Euro / MWh.

This means in this example the price cap reduces buyer expenditures by 42% compared to the current marginal cost pricing system. The money comes from a reduction of the infra-marginal rents of non-gas power plants. 

## UPDATE [2022-09-28 15:00]: RISK OF A MONEY PUMP?

I just got a very good remark on a potentially quite tricky problem (thanks Justus!). 

Under the proposed mechanism, gas power plant owners could buy gas cheaper than the price they get when selling gas. That might possibly provide incentives to buy and sell electricity at the same time in a form of money pump. Below I also suggest that pump storage suppliers should get the same price than gas power plants, i.e. a similar problem would arise for storage owners. Possibly the money pump problem is indeed only an issue for storage owners, since they are physically able to generate demand.

This problem might be quite problematic for this mechanism. But it might not be as bad. I have to think deeper about that issue. 

One always could think of the following modification of the mechanism. Buyers pay the marginal cost as price, i.e. in the example 1000 Euro / MWh, but non-gas plants still get at most the price cap. The extra earnings will be collected by the government that can use the money to support energy users (e.g. lump sum transfers based on historic electricity usage). The mechanism would then not reduce electricity prices but still collect some of the inframarginal rents to have money that is needed to suppport energy consumers.

Possibly, I will update the post later again, with hopefully a better understanding on this issue.

## Discussion

Let us discuss the key elements and critical issues of this proposal:

- There is no explicit government subsidy. Everything is paid by the buyers in the spot market.

- The price cap should be set sufficiently above marginal costs of coal power plants (and perhaps oil power plants), so that they have incentives to always bid their full capacity. On the other hand, we don't need to incentivize new investments of coal power plants as there is the plan to phase out of coal energy.

- Such a price cap should still provide sufficient incentives for wind and solar investments, as prices are still much higher than before the crisis. I don't think that the current investment bottlenecks are low electricity prices, but it is rather administrative hurdles and limited construction capacity. In general, it may anyway be a good idea to more often incentivize renewable investments with contracts for differences (see e.g. [here](https://www.windbranche.de/news/ticker/offshore-windenergie-bdew-plaediert-fuer-differenzvertraege-und-europaeischen-ansatz-artikel2606)), at least for off-shore wind.  Also note that the price cap probably affects wind and solar plants less than coal power plants, since in times with high wind and solar productions market prices are likely below the cap anyway.

- In principle, one could extend the system by specifying different price caps for different non-gas technologies (coal, lignite, wind, solar, etc.). But my gut feeling is that this is not necessary and not too much would be gained from this additional complexity.

- I would treat electricity production from storage (e.g. pump storage or battery storage) like production from gas power plants, i.e. impose no price cap. Storage providers need sufficient price differences and we also want to keep investments incentives for storage at a high level. 

- Note that the mechanism still has substantial price variation between periods, which is important to provide incentives for increased demand flexibility.

- It is true that the lower electricity prices provide smaller incentives to reduce demand. But this is a measure to reduce the burden of possibly 10-20  higher electricity prices than in the past. Also with this measure prices will stay still very high. To induce demand reduction changing old household contracts with low prices to new contracts with high prices (for a compensation) as e.g. suggested [here](https://twitter.com/christianbaye13/status/1560293430697279489) might be much more efficient.

- One main question is whether such a design could be legally implemented. In the end the crux is to take away some of the extraordinary wind-fall profits from non-gas power plants. But is the state allowed to do so? I don't know what or how this can be legally done. 

- A second important question is how to make the design compatible with cross-border trades. If only German markets implement a price cap on coal and renewables, coal and renewable producers may tend to export their power while we import gas power at uncapped prices from our neighbors. This might substantially reduce the price reduction from this proposal. So one should (and probably legally has to) coordinate such a mechanism on EU level. In principle, I see that other EU countries like France would benefit from this mechanism in a similar fashion as Germany, so incentives for a coordinated implementation could be there.

- A third main question is whether it is technically & administratively possible to implement such a system sufficiently quickly. While fast implementation is probably tricky, the proposal seems considerably simpler than something like a tax on excessive profits of energy companies that is also discussed in politics.

- But what happens with people who already bought electricity futures for 1000 Euro / MWh? Won't they have a problem if now suddenly such a policy measure reduces prices? I guess market participants anticipate the possibility of some policy measures. So they probably won't have bought all their future electricity demand at those prices.

- Are other measures necessary? Definitely. Targeted transfers to low income households will be crucial as gas and electricity will still be incredibly high. Yet, I guess devising sensible targeted transfers for firms is much more tricky. Trying to reduce spot prices using this proposal might be a sensible complementary measure to dampen the burden of skyrocketing energy costs for firms and households. In addition, I really hope, that our government decides quickly to keep the remaining three nuclear plants running for a little while longer, that we ensure that all available coal capacity is running, that we manage to substantially reduce gas demand, and that we manage to considerably speed up renewable investments.


- Have I overlooked some important points? That might very well be the case. Please send me an email then or also send me an email if you have other comments or suggestions.

- Has anybody else made this proposal earlier? That might well be the case, I have not performed an extensive literature check and I guess most energy economists are currently thinking hard on this problem. Even better if somebody else has had the same or a better idea.

## Some numbers

For a better quantitative intuition, I performed some very crude computations in R yielding the following plot:

<center>
<img src="https://skranz.github.io/images/elprices/elec_prices_vs_cost_aug22.svg" style="max-width: 97%;margin-bottom: 0.5em;">
<br>
</center>

The colored bars correspond to different power plant types in Germany. The y-axis shows my crude estimate of their average variable costs in the considered time period. The x-axis shows the plant types average share of electricity production in the considered period. The black line shows the average German spot market electricity price in that period. The left panel shows the averages for the years 2015-2020 and the right panel shows the data for the first 20 days in August 2022 where average electricity prices (black lines) have already exceeded 400 Euro / MWh.

The area of the colored bars show the average variable production cost for 1 MWh electricity in the period and the area under the black line shows the price that is on average paid for it on the spot market. 

While both costs and prices are massively higher in August 2022, prices have in absolute terms (also in relative terms) increased much more than average variable costs. The graph also illustrates the now huge infra-marginal rents that non-gas plants can on average achieve by selling on the spot market. The idea of my proposal is to reduce these rents by imposing the price cap for non-gas power plants. Note that already on the left hand side the much smaller rents were probably sufficient to cover fixed costs on average. Thus, there should be enough scope at the current or even higher future prices to implement such a price cap while still maintaining high investment incentives for renewables (and incentives to keep coal plants alive as long as needed).

Looking at the future prices of roughly 1000 Euro / MWh for 2023, the differences between electricity prices and average variable costs seem even much higher than for our Aug. 2022 data. The increase in future electricity prices seems to go beyond the expected increase in variable costs of CCGT plants (see e.g. [here](https://twitter.com/LionHirth/status/1563385668906074112)). That might be explained by expectations that next year OCGT plants will be more often price setting, yielding a higher gap between prices and average variable costs. Coal future prices for 2023 seem somewhat lower than currently in 2022 (see [here](https://www.barchart.com/futures/quotes/LQH23)).


Disclaimer concerning the plot above:

Note that the variable cost estimates are partially based on quite crude assumptions. For gas (CCGT and OCGT), coal and oil, I had time series of fuel prices available (more detail in my [previous post](http://skranz.github.io/r/2022/08/23/ElectricityPrices.html)).  But for nuclear, lignite, biomass, and "other", I relatively arbitrarily assumed that the ratio of their fuel costs relative to the oil price stays constant over time and I used the ratios implied by the power plant data used in the [DIW Dieter model](https://www.diw.de/de/diw_01.c.599753.de/modelle.html#c_803498).

Furthermore, available production data does not distinguish between production from CCGT and OCGT gas power plants. I also did not know how much of the roughly 30 GW German gas power capacity are the more efficient CCGT (combined cycle gas turbine) power plants and how much the cheaper to build OCGT (open cycle gas turbine). I just assumed that 25% of that capacity, i.e. 7.5 GW are CCGT and that the first 7.5 GW gas production always comes from CCGT before OCGT start to produce.

So while the details may be wrong, I think the main insight from the plot is relatively robust.

Author: Sebastian Kranz, Ulm University