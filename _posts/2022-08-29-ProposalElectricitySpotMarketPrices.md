---
layout: post
title: "A proposal for capping exploding electricity spot market prices without subsidies or supply reduction"
date:   2022-08-29 10:00:00
categories: r
tags: [R, energy]
---

At the EEX, [German baseload electricity futures](https://www.eex.com/en/market-data/power/futures#%7B%22snippetpicker%22%3A%22EEX%20German%20Power%20Future%22%7D) for the year 2023 trade at a price of 950 Euro / MWh and peak load futures at 1275 Euro / MWh. Future prices for France are even higher. (Prices were looked up on 2022-08-28). 

In contrast, average German spot market prices 2015 to 2021 were below 50 Euro / MWh. That market participants seem to expect twenty-fold higher prices in 2023 than the average of those previous years seems really scary. (I deeply hope that at least a lot of firms have hedged their long term electricity demand when future prices were still low).

The current electricity markets operate in their core like other free markets where prices are determined by the intersection between supply and demand. (Electricity spot and future markets are also augmented by markets for balancing energy, some mechanism for redispatch to deal with network constraints, and in some countries capacity mechanisms). And in normal times, this market design seems in many dimensions superior to alternatives. Yet, given the current unforeseen, massive shock, some modifications could turn out helpful to dampen the impact. I know that it seems incredibly hard to implement sensible reforms within a short time frame, but still wanted to write up a proposal.<br><br> 
*[UPDATE 2022-08-31]: After the post went into circulation, I got extremely helpful feedback from several people. I have included several updates in the discussion section. Indeed, the discussion section might be more interesting than the proposal itself. It points out a growing list of potential quite serious issues that have to be tackled, not all of them with an obvious solution. Many issues will also arise in other proposals for a change in market design or for a taxation of infra-marginal profits. At the moment, I am even more convinced that these issues can unfortunately be extremely tricky and that warnings like [this one from EPEX](https://www.epexspot.com/sites/default/files/download_center_files/20220502_Policy%20note_Safeguarding%20the%20benefits%20of%20European%20power%20market.pdf) should be taken seriously. On the other hand, the risks of such stark electricity price increases are also huge. So one should definitely think a lot and carefully about whether there are sensible measures, we could implement.* 


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


## Discussion

Let us discuss key elements, critical issues of this proposal and some additional points:

- There is no explicit government subsidy. Everything is paid by the buyers in the spot market.

- The price cap should be set sufficiently above marginal costs of coal power plants (and perhaps oil power plants), so that they have incentives to always bid their full capacity. On the other hand, we don't need to incentivize new investments of coal power plants as there is the plan to phase out of coal energy. *[Update 2022-08-31] [Very good point by Aurel Wuensch](https://twitter.com/aurelwuensch/status/1564866521339404288): restarting coal power plants can take quite a long time, so that sometimes they run for several hours at minimal capacity at low spot prices in order to be able to deliver in later hours with high prices. If we have a binding price cap in these later hours, they may instead decide to shut down and the electricity later must be provided by gas power plants. Perhaps not a huge problem if the cap is sufficiently high, but I cannot make a good quantitative assessment. One could also think of a dynamic cap: after some hours of low energy prices the cap starts at a higher level. But that would be fairly complex and hard to calibrate.*

- Such a price cap should still provide sufficient incentives for wind and solar investments, as prices are still much higher than before the crisis. I don't think that the current investment bottlenecks are low electricity prices, but it is rather administrative hurdles and limited construction capacity. In general, it may anyway be a good idea to more often incentivize renewable investments with contracts for differences (see e.g. [here](https://www.windbranche.de/news/ticker/offshore-windenergie-bdew-plaediert-fuer-differenzvertraege-und-europaeischen-ansatz-artikel2606)), at least for off-shore wind.  Also note that the price cap probably affects wind and solar plants less than coal power plants, since in times with high wind and solar productions market prices are likely below the cap anyway.

- In principle, one could extend the system by specifying different price caps for different non-gas technologies (coal, lignite, wind, solar, etc.). But my gut feeling is that this is not necessary and not too much would be gained from this additional complexity.

- *UPDATED [2022-08-30] Pump storage plants or other form of storage should get the buyer's price when they sell electricity in a period (and also pay the buyers price when buying it). Initially, I proposed that they get the same price as gas power plants, but Justus Haucap rightly pointed out that there could be huge manipulation incentives if a pump storage plant could at the same time buy electricity cheaper than sell it.*

- *UPDATE [2022-08-31] [Another very good point by Aurel Wuensch](https://twitter.com/aurelwuensch/status/1564883715314024448): How do we deal with seasonal hydro storage? If seasonal storage owners expect price caps, they have incentives to empty their storages now as fast as possible. Yet, we probably really don't want empty hydro reservoirs in winter. I am not yet sure what would be a good solution. Allowing seasonal storage to sell at the buyer's price may not be sufficient to remove incentives to massively sell before a reform takes place. So we probably have to take out seasonal storage from the price cap. But can we do that without inducing manipulation incentives?* 

- Note that the mechanism still has substantial price variation between periods, which is important to provide incentives for increased demand flexibility.

- It is true that the lower electricity prices provide smaller incentives to reduce demand. But this is a measure to reduce the burden of possibly 10-20  higher electricity prices than in the past. Also with this measure prices will stay still very high. To induce demand reduction changing old household contracts with low prices to new contracts with high prices (for a compensation) as e.g. suggested [here](https://twitter.com/christianbaye13/status/1560293430697279489) might be much more efficient.



- One main question is whether such a design could be legally implemented. In the end the crux is to take away some of the extraordinary wind-fall profits from non-gas power plants. But is the state allowed to do so? I don't know what or how this can be legally done. 

- A second important question is how to make the design compatible with cross-border trades. If only German markets implement a price cap on coal and renewables, coal and renewable producers may tend to export their power while we import gas power at uncapped prices from our neighbors. This might substantially reduce the price reduction from this proposal. So one should (and probably legally has to) coordinate such a mechanism on EU level. In principle, I see that other EU countries like France would benefit from this mechanism in a similar fashion as Germany, so incentives for a coordinated implementation could be there.
*[Update 2022-08-30] for energy exports outside the EU or if there is no coordination within the EU, I would propose the following. If a coal power plant (or some other non-gas producer) sells to a foreign market, they are still only allowed to get the price cap of e.g. 300 Euro / MWh. If the foreign price is higher, the government gets the difference as a form of export tax. This prevents that plants try to circumvent the price cap via exports.*

- A third main question is whether it is technically & administratively possible to implement such a system sufficiently quickly. While fast implementation is probably tricky, the proposal seems considerably simpler than something like a tax on excessive profits of energy companies that is also discussed in politics.

- <a id="OTC"></a>*[UPDATE 2022-08-30] Another important issue is how to deal with OTC trades or other trades outside the regulated spot markets. (Thanks to Axel Ockenfels for first pointing out this issue to me). To think about it, I find it helpful to frame the proposal in a different way that is still equivalent to the original proposal. As example assume the price cap is 300 Euro / MWh, but in the given period the buyer's spot price in our new regulation is 400 Euro / MWh. You can think of the difference of 400-300 = 100 Euro / MWh as a fee imposed on the each unit of sold electricity that is used to cover the excess costs of gas power plants. My proposal is as follows: If two parties make an OTC trade outside the spot market and this leads to delivery from a non-gas power plant in some period, the producer shall be required to also pay this period's fee to the exchange or regulator and this fee payment is also used to pay the excess cost of gas power plants. (This means, to determine the buyer's price in the spot market these OTC trades will also be added to the total quantity). Since we know which plants produce how much electricity in each period, there should be actually almost no risk of a black market where producers can avoid the fee. There may also be no need of a regulator to know much details of OTC contracts, in order to determine which fee needs to be paid. One just needs to check who physically produces electricity that was not sold via the spot market.<br>Note: I have not completely thought through what to do if a gas power plant sells via an OTC contract. Possibly these contracts can be ignored without any fee to be paid by any party. I also have not completely thought about OTC imports and exports.*


<ul><li><p><em>[UPDATE 2022-08-31] <b>EXISTING FUTURE CONTRACTS. (A THORNY ISSUE)</b> Many market participants will likely have already hedged themselves with financially settled future contracts. If we change the market design, we need to specify what shall be the underlying spot price relevant for the settlement of a future contract. The price cap? The buyer's spot market price? The spot market price gas power plants receive? Let us assume, the relevant spot price on which payments of the future contract are based on is the buyer's spot market price.
<br>
Consider an example:
<br>
- Assume the price specified in the future is 250 Euro / MWh,<br>
- the price cap is 300 Euro / MWh<br>
- the spot price for buyers given our new regulation is 400 Euro / MWh,<br>
- under the original marginal cost pricing the spot price would be 600 Euro / MWh.<br>

Assume the buyer is an electricity consumer, but the seller of the future does not own a power plant. 
<br>
Then under our new proposal, the buyer will receive the difference between the spot price 400 Euro / MWh and the price specified in the future 250 Euro / MWh, i.e. 150 Euro / MWh from the seller of the financially settled future. As the electricity consumer needs to pay 400 Euro / MWh on the spot market, his net payment is actually 400-150 = 250 Euro / MWh. In the old market design he would get 600-250=350 Euro / MWh from the seller of the future, and would then have again a net payment of 600-350 = 250 Euro / MWh. This means a buyer of a future who is an electricity consumer would not be affected by the change in the market design. A seller of the future who is not an electricity producer, would be better off by the change of the market design: under the new market design, he would have to pay 400-250 = 150 Euro / MWh and under the old design 600-250 = 350 Euro / MWh. So is all fine? The buyer of a future is indifferent and the seller is even better off by the change of the market design?
<br>
I indeed initially thought that all is fine. But then I got an email with the good remark to consider the very common case that the seller of the future contract is the operator of a coal power plant (or another non-gas power plant) who sold its production in the future market as a hedge. Under the old regulation, the seller would need to pay the buyer 600 - 250 = 350 Euro / MWh, but would receive on the spot market 600 Euro / MWh by selling electricity from the coal power plant. So net the seller would receive 600-350 = 250 Euro / MWh, i.e. the price specified in the future. Under our new regulation the seller would need to pay 400 - 250 = 150 Euro / MWh for the financial settlement. Yet, when selling the electricity from the coal power plant on the spot market, he would now only get the price cap of 300 Euro / MWh. Hence, net the coal power plant would now only get 300 - 150 = 150 Euro / MWh. He would lose the difference between the price cap and the spot market price of the buyer. This means such a regulation could severely harm coal power plant operators who sold their production at low future prices. And that might be a lot of operators.
<br>
Such power plant operators would not be worse off from the regulation only if we say the relevant spot price used to determine the payments of a future contract is here equal to the price cap of 300 Euro / MWh. But in that case, buyers who have hedged themselves would be worse off from our new regulation. 
<br>
So, in the moment it looks to me that changing the market design according to my proposal runs the risk to either severely harm buyers on future markets or sellers on the future market who own a non-gas power plant. If not somehow solved, this issue may indeed be a show stopper for the proposed mechanism.
</em></p></li></ul>

- *[UPDATE 2022-08-31] We also have to think of how we deal with previously created forward contracts that determine a physical delivery at a point of time when the new regulation is in place. I would propose that such contracts should have under the new regulation the same financial impacts as if the seller and buyer had a financially settled future contract and then trade on the spot market. Of course, as long as we don't have a good solution for how to deal with existing future contracts (see previous point), we then neither will have a good solution for existing forward contracts with physical delivery. Let us still look at an example where the seller of the forward contract is a coal power plant.<br>Assume the delivery price in the forward contract is 250 Euro / MWh, the price cap is 300 Euro / MWh and the buyer's spot price in the delivery period is 400 Euro / MWh. Financial settlement would mean that the seller has to pay the buyer 400-250 = 150 Euro / MWh. The buyer would have an effective price of 400 - 150 = 250 Euro / MWh (the same as specified in the OTC contract). In the spot market, the coal plant gets a price of 300 Euro / MWh. Subtracting the payment obligation from the fictitious future contract, the seller would keep 300-150=150 Euro / MWh.  Hence, the forward contract would be equivalent for the seller to a future contract with spot sale if he only keeps 150 Euro / MWh instead of the  the 250 Euro / MWh specified in the forward contract. The solution is that the seller in the forward contract, has to pay the exchange / regulator the "fee" of 400-300 = 100 Euro / MWh for the gas power plants' excess cost in this period. More precisely, the fee will be charged from the producing non-gas power plant (see two bullet points above). We would have the same problem as for futures: the coal power plant owner who sold his electricity in a forward contract gets less money under the new regulation than under the original market design. If we would allow him to keep the 250 Euro / MWh, we would put trades outside the spot market in a better position than trading via the spot market. Then spot markets might very well run dry and our regulation would be ineffective.*    

- *[UPDATE 2022-08-31] Looking at the previous three points: the price cap on non-gas power plants has features of a tax on electricity production from non-gas power plants that is used to subsidize electricity production from gas power plants. But it would be a dynamic "tax" that is in each period equal to the difference between the buyers' price and the price cap in the spot market and unlike an actual tax, it would not increase the state's budget but reduce buyers' prices. If the electricity is sold via spot market, think that without the "tax" the non-gas producers would get the buyers' price, but with the "tax" they only get the price cap. If electricity production by a non-gas plant is triggered by an OTC contract, the same "tax" has to be paid. I know that Axel Ockenfels is floating a proposal of a tax on non-gas power production (I will add a link once the proposal is on the web). Also [Georg Zachman mad a tax-based proposal](https://twitter.com/GeorgZachmann/status/1564620708281565198). There may be many similarities between those proposals and a price cap (see also next point). I also need to look deeper into the relationship to the Iberian market design. If I understood correctly, they capped fuel prices for gas power plants and the fuel price subsidy was also financed by a fee. But probably also this mechanisms face a similar problem than mine: is there a way that neither buyers nor non-gas power plant producers that hedged themselves in future or forward markets will be worse off when changing the regulation?*

- *UPDATE [2022-08-30]: The mechanism as proposed reduces buyer prices compared to the status quo. One could also think of the following variation.  Buyers pay the marginal bid as price as in the current system, i.e. in the numerical example 1000 Euro / MWh, but non-gas plants still get at most the price cap. The extra earnings will be collected by the government that can use the money to support energy users (e.g. lump sum transfers based on historic electricity usage). The mechanism would then not reduce electricity prices but one would still collect some of the infra-marginal rents to have money that is needed to support energy consumers. While for households lump sum transfers e.g. based on historical consumption are probably even better than lowering prices, it might be extremely tricky to support firms in another way than by reducing price increases.*

- *[UPDATE 2022-08-30] I have not thought deeply enough of how to regulate sales of non-gas power plants on balancing energy markets (German "Regelenergie"), but some adaption may be necessary if we change the spot market design.*  

- What happens with people who already bought electricity futures for 1000 Euro / MWh? Won't they have a problem if now suddenly such a policy measure reduces prices? I guess market participants anticipate the possibility of some policy measures. So they probably won't have bought all their future electricity demand at those prices.

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