---
layout: post
title: "Electricity markets: Can the EU deal with forward hedging when taxing infra-marginal profits?"
date:   2022-09-02 14:50:00
categories: r
tags: [R, energy]
---

In a [leaked first draft](https://www.reuters.com/business/energy/eu-mulling-energy-price-cap-certain-generators-document-2022-09-01/) the EU proposes in its own words a

> Price cap for inframarginal technologies for the benefit of consumers

Sounds similar to [my recent proposal](http://skranz.github.io/r/2022/08/29/ProposalElectricitySpotMarketPrices.html). A difference to my main proposal is that the EU does not want to reduce the spot market prices, but suggests that governments rather collect the difference between a technology's price cap and the actual spot market as a tax that can be used to finance other support measures (e.g. lump-sum transfers to consumers).

To better understand the motivation for taxing infra-marginal profits, let me show an updated version of a plot from my previous post (now incorporating the complete data for August 2022, a description of assumptions and data source can be found [here](http://skranz.github.io/r/2022/08/23/ElectricityPrices.html)):

<center>
<img src="https://skranz.github.io/images/elprices/elec_prices_vs_cost_full_aug22.svg" style="max-width: 97%;margin-bottom: 0.5em;">
<br>
</center>

The area of the colored bars shows the estimated average variable cost of electricity production per MWh and the area under the black line the average price per MWh. The difference between the two areas roughly describes the producers' rents (mainly *infra-marginal rents* plus some rents for the most expensive marginal plant to cover its fixed costs). These rents must be big enough to cover investment costs and other fixed costs of power plants. 

In the time period 2015-2020 (on the left) these rents were on average 27.4 Euro / MWh and we may assume that this was roughly large enough to cover investment and fixed costs of power plants. For August 2022, I compute average rents of 371 Euro / MWh, i.e. rents increased by a factor of 13.6 (in contrast average variable cost increased "only" by a factor of 6.57 from 14.9 Euro / MWh to 98.2 Euro / MWh). 

These numbers suggest that there should be sufficient scope to reduce power plants' rents in order to relieve consumers' burden while keeping enough money in the system for investments and other fixed costs. Of course, not all electricity is traded on spot markets, but German baseload power future prices for 2023 trade today even higher at 515 Euro / MWh and peakload futures at 790 Euro / MWh (and those future prices probably already incorporate a positive probability that politics takes measures to reduce next year's spot prices).

Is there a way to shift a substantial part of these rents to consumers without disastrous, unintended consequences?

## The issue of hedged power plant operators with forward / future positions

The risk of unintended consequences would probably be much lower if all power plants sold all energy on the spot market and performed no hedging. We could then introduce either a maximum price that non-gas power plants receive when selling electricity on the spot market, or determine the size of the tax based on the difference between spot price and estimated variable cost of power plants (calculated for each 15 minute trading interval separately).

But consider the following situation. A coal power plant owner hedged its price risk by selling in 2020 cash settled future contracts for his expected electricity delivery in 2023 say at a price of 80 Euro / MWh. (I don't know the actual historic future prices). 

Now take a trading period in 2023 and assume spot market prices are 600 Euro / MWh. The coal power plant has to pay the buyer of the future 520 Euro / MWh, the difference between the 2023 spot market price and the delivery price specified in the future. But, without a price cap or tax, the coal power plant can just sell its electricity for 600 Euro / MWh on the spot market. So in total the coal plan operator gets 600 - 520 = 80 Euro / MWh, the same amount as specified in the future.

But now assume that the coal power plant faces a price cap in the spot market and is allowed to receive only 250 Euro / MWh (or equivalently it gets the 600 Euro / MWh but has to pay a tax of 350 Euro / MWh). Then the coal plant would lose 350-80 = 270 Euro / MWh plus its variable cost from the transaction and may quickly become insolvent.

It would clearly be very bad from an economic and legal perspective if this unintended side effect of the regulation causes power plant owners with a sound hedging strategy to become bankrupt.

I don't know how prevalent this case is, but unless it is a very rare exception (which I doubt), it must be addressed.

## What could one do?

I guess the core idea would be that for hedged electricity producers one must determine the "actual price" at which they sell electricity, taking into account future and forward positions. 

That may be a very tricky task as there may be different instruments for hedging (cash settled futures, forward contracts with physical delivery, options (?), or more complex contractual structures). Also hedging probably does not take place at the level of a single power plant but at the level of a firm which may have different types of power plants. So one probably needs some rule of how company wide hedging positions are boiled down to "actual prices" that the different producing plants receive.

OK, assume we have such a rule that maps hedging positions into prices for power plants. Now firms might have strong incentives to exploit that rule in order to reduce their tax burden. And history shows that firms can be quite creative in inventing schemes that reduce tax payments.

To limit such exploitation risk, one may need to look at firms' hedging positions at a date lying sufficiently far away in the past when firms had not yet incentives to manipulate their positions in order to reduce new taxes on infra-marginal rents.

Can it be done without too much excess burden? I don't know, as I have very little detailed institutional knowledge.

## Is the hedging problem really so severe?

Again I lack institutional details to assess how severe the hedging problem actually is for the goal of shifting infra-marginal profits to consumers. Possibly some factors might make the hedging problem less severe:

- If I understand correctly commercial wind and solar farms are often financed by [Power Purchase Agreements (PPA)](https://en.wikipedia.org/wiki/Power_purchase_agreement) that fix a long term price for each MWh electricity produced from that actual wind or solar farm. Under PPA it might be simply to know the price a solar and wind farm actually achieves: it's just the price specified in the PPA. (But maybe PPA are more complicated than I think.)

- Coal power plant owners hopefully have not sold forward all their potential production. If the price cap is sufficiently loose, they may still make substantial profits on their excess production sold in the spot market which balance potential losses from their hedged positions. I mean, in the end someone probably gets high infra-marginal profits (unless all spot market buyers hedged themselves at low prices in which case there would be no problem). It seems unlikely that some electricity producers were fully hedged and others not at all. 

- Also a varied plant portfolio of power plant companies might help to balance profits and losses from hedged positions.

So if we are lucky, a simple rule might suffice to ensure that producers with perhaps 95% of market capacity will not get financial troubles when taking away infra-marginal profits and one has to apply more complex procedures only for a small share of companies. Are we lucky? I don't know.

## Feedback

Given my limited knowledge of institutional details, I would appreciate feedback very much. In particular, if you the know the reality of electricity markets much better than me and have suggestions please don't hesitate to send me an email. I try to update the post regularly if I get relevant new insights over time.

Author: Sebastian Kranz, Ulm University