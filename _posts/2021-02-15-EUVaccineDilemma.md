---
layout: post
title: "EU Vaccine Procurement and a Public Goods Dilemma"
cover: 
date:   2021-02-15 15:00:00
categories: r
tags: [R, economics]
---

Many EU citizens (including me) are frustrated that due to lower supply, vaccinations in EU countries currently proceed substantially slower than in the US or the UK. In this post I make some simple economic arguments (illustrated with R) for how the common approach of the EU members might have been improved by mitigating a public goods dilemma that reduced member states' incentives to sponsor early capacity expansion.

In this [press release](https://ec.europa.eu/commission/presscorner/detail/en/QANDA_21_48) the EU commission motivates the joint procurement as follows:

> Vaccines have to be affordable. This is also part of the rationale for doing this together as a team: this reduces costs for everyone and gives us a stronger negotiating position.

According to some [leaked information](https://www.theguardian.com/world/2020/dec/18/belgian-minister-accidentally-tweets-eus-covid-vaccine-price-list) the EU indeed seems to have been successful in negotiating comparatively low prices. Yet, I guess that even if one would pay twice, thrice or five-fold the negotiated prices, the benefits of widespread vaccinations would still vastly outweigh the costs of extended lock-downs (see e.g. [this assessment](https://fortune.com/2021/02/03/eu-slow-vaccine-roll-out-lockdowns-cost/)).

I guess in hindsight, the EU should not have focused as much on negotiating low prices but rather offer considerably more money to make firms commit to larger and faster development of production capacities (which would likely also benefit countries outside the EU), and while not necessarily trying to grab all vaccines itself, at least ensure that firms don't prioritize other countries in early supply.

That being said, other western industrial countries made similar mistakes. For example, [Switzerland is only slightly ahead](https://www.admin.ch/gov/en/start/documentation/media-releases.msg-id-82224.html) the EU vaccination speed despite having a large pharmaceutical industry and [Canada is even slower](https://www.bbc.com/news/world-us-canada-56035306). Similar to the EU, both countries seem not to have strongly focused on fostering production capacities.

So while not desirable, it is also not too surprising that the coordinated EU approach made similar mistakes as Canada or Switzerland. Yet, given that any outcome in the EU aggregates diverse views of its member states, probably some of the 27 member states would also initially have preferred that more money is spend to quickly increase production capacity and ensure early delivery.

In the common approach all EU member states pay the same price and vaccines are distributed roughly proportionally to a member states population (unless a country opts out of buying a particular vaccine). Also, the president of the EU commission Ursula von der Leyen [made clear](https://www.reuters.com/article/us-health-coronavirus-eu-vaccine-idUSKBN29D13Y) (after the UK has left) that "EU countries are not allowed to negotiate separate vaccine deals with pharmaceutical companies in parallel to the efforts of the European Union as a whole". 

EU member countries could still subsidize expansion of production capacity, e.g. in September [Biontech received a 375 Mio. Euro grant from Germany](https://investors.biontech.de/news-releases/news-release-details/biontech-receive-eu375m-funding-german-federal-ministry) partially used to ramp up production capacity. Yet, if I understand correctly, member states were not allowed to tie funding of new production facilities with priority delivery from the resulting additional capacities. The problem is that this restriction may have substantially reduced member states' incentives to fund themselves capacity expansions.

Let us illustrate the problem with some simple calculations in R:

```r
# Population data was collected from 
# https://appsso.eurostat.ec.europa.eu/nui/submitViewTableAction.do
dat = read.csv("eu_pop.csv") %>%
  mutate(
    share.pop  = population / sum(population),
    min.return.factor = round((1/share.pop),1),
    population = round(population / 1e6,1),
    share.pop = round(100*share.pop,1)
  ) %>%
  arrange(desc(population))
dat
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 table.data-frame-table tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">country</th><th class="data-frame-th">population</th><th class="data-frame-th">share.pop</th><th class="data-frame-th">min.return.factor</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Germany</td><td class="data-frame-td" nowrap bgcolor="#dddddd">83.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">18.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5.4</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">France</td><td class="data-frame-td" nowrap bgcolor="#ffffff">67.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">15</td><td class="data-frame-td" nowrap bgcolor="#ffffff">6.6</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Italy</td><td class="data-frame-td" nowrap bgcolor="#dddddd">59.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">13.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">7.5</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Spain</td><td class="data-frame-td" nowrap bgcolor="#ffffff">47.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">9.5</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Poland</td><td class="data-frame-td" nowrap bgcolor="#dddddd">38</td><td class="data-frame-td" nowrap bgcolor="#dddddd">8.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">11.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Romania</td><td class="data-frame-td" nowrap bgcolor="#ffffff">19.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">4.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">23.1</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Netherlands</td><td class="data-frame-td" nowrap bgcolor="#dddddd">17.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">25.7</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Belgium</td><td class="data-frame-td" nowrap bgcolor="#ffffff">11.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">38.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Czechia</td><td class="data-frame-td" nowrap bgcolor="#dddddd">10.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">41.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Greece</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10.7</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.4</td><td class="data-frame-td" nowrap bgcolor="#ffffff">41.7</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Portugal</td><td class="data-frame-td" nowrap bgcolor="#dddddd">10.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">43.4</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Sweden</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">43.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Hungary</td><td class="data-frame-td" nowrap bgcolor="#dddddd">9.8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">45.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Austria</td><td class="data-frame-td" nowrap bgcolor="#ffffff">8.9</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Bulgaria</td><td class="data-frame-td" nowrap bgcolor="#dddddd">7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">64.3</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Denmark</td><td class="data-frame-td" nowrap bgcolor="#ffffff">5.8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">76.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Slovakia</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">82</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Finland</td><td class="data-frame-td" nowrap bgcolor="#ffffff">5.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">81</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Ireland</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">90.1</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Croatia</td><td class="data-frame-td" nowrap bgcolor="#ffffff">4.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.9</td><td class="data-frame-td" nowrap bgcolor="#ffffff">110</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Lithuania</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">160</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Slovenia</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">213</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Latvia</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">234</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Estonia</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">337</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Cyprus</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">504</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Luxembourg</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">714</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">Malta</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.5</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.1</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">869</td></tr>
</table>

The table shows the EU 27 countries sorted by their population. For example, France has 67.3 million inhabitants which account for 15% of the total EU population. If France would subsidize capacity expansions that would lead to say 100 million vaccine dosages being available earlier, France would get at most 15% of these earlier dosages. That means for France itself such subsidies would pay-off only if the total expected social return of such subsidies is at least 6.6 (=1/0.15) times as large as the subsidy. The column `min.return.factor` shows this factor for all EU countries. For example, for Finland with 1.2% of the EU population such subsidies would be worthwhile only if the total expected benefits are 81 times larger than the costs, and for Luxembourg expected benefits would need to be 714 times larger. We essentially have a classic [public goods dilemma](https://en.wikipedia.org/wiki/Collective_action_problem#:~:text=A%20public%20goods%20dilemma%20is,riding%E2%80%9D%20if%20enough%20others%20contribute.&text=In%20economics%2C%20the%20literature%20around,as%20the%20free%20rider%20problem.).

In contrast, a country like the US or UK is free to make deals that tie subsidies (also in the form of higher purchase prices) to priority deliveries for themselves, which makes funds to increase capacity worthwhile already if the expected benefits would be only slightly larger than the costs.

Of course, the cleanest solution would have been that the EU would have spend more money quickly enough to foster capacity expansions. Yet, it seems not too surprising that the EU as a whole will act slower and on a smaller scale than the US or UK, given the diverging assessments of member states and the need to find mutual agreement. 

In my view the EU should have realized and openly communicated the natural limitations of its joint approach and tried to design it in a way that encourages willing member states to provide additional funding in a way that does not harm other member states.

What about this approach: The EU explicitly encourages willing member states (individually or in groups) to move ahead and provide additional funding for capacity expansions. Importantly, members are allowed to keep directly up to 50% of vaccines produced from the additionally created capacities, 30% must be supplied according to the joint EU deal, and 20% can be delivered to third countries.

Let us compute numbers for this alternative framework:

```r
dat = dat %>%  mutate(
    keep.share = 0.5+0.3*(share.pop/100),
    new.return.factor = round(1 / keep.share,1),
    keep.share = round(100*keep.share,1)
  ) 
dat
```

<style> table.data-frame-table {	border-collapse: collapse;  display: block; overflow-x: auto;}
 td.data-frame-td {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%;}
 td.data-frame-td-bottom {font-family: Verdana,Geneva,sans-serif; margin: 0px 3px 1px 3px; padding: 1px 3px 1px 3px; border-left: solid 1px black; border-right: solid 1px black; text-align: left;font-size: 80%; border-bottom: solid 1px black;}
 th.data-frame-th {font-weight: bold; margin: 3px; padding: 3px; border: solid 1px black; text-align: center;font-size: 80%;}
 table.data-frame-table tbody>tr:last-child>td {
      border-bottom: solid 1px black;
    }
</style><table class="data-frame-table">
<tr><th class="data-frame-th">country</th><th class="data-frame-th">population</th><th class="data-frame-th">share.pop</th><th class="data-frame-th">min.return.factor</th><th class="data-frame-th">keep.share</th><th class="data-frame-th">new.return.factor</th></tr><tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Germany</td><td class="data-frame-td" nowrap bgcolor="#dddddd">83.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">18.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">55.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">France</td><td class="data-frame-td" nowrap bgcolor="#ffffff">67.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">15</td><td class="data-frame-td" nowrap bgcolor="#ffffff">6.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">54.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.8</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Italy</td><td class="data-frame-td" nowrap bgcolor="#dddddd">59.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">13.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">7.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">54</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.9</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Spain</td><td class="data-frame-td" nowrap bgcolor="#ffffff">47.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">9.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">53.2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.9</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Poland</td><td class="data-frame-td" nowrap bgcolor="#dddddd">38</td><td class="data-frame-td" nowrap bgcolor="#dddddd">8.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">11.8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">52.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.9</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Romania</td><td class="data-frame-td" nowrap bgcolor="#ffffff">19.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">4.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">23.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">51.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.9</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Netherlands</td><td class="data-frame-td" nowrap bgcolor="#dddddd">17.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">3.9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">25.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">51.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Belgium</td><td class="data-frame-td" nowrap bgcolor="#ffffff">11.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">38.8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Czechia</td><td class="data-frame-td" nowrap bgcolor="#dddddd">10.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">41.8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Greece</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10.7</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.4</td><td class="data-frame-td" nowrap bgcolor="#ffffff">41.7</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.7</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Portugal</td><td class="data-frame-td" nowrap bgcolor="#dddddd">10.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">43.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Sweden</td><td class="data-frame-td" nowrap bgcolor="#ffffff">10.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">43.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.7</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Hungary</td><td class="data-frame-td" nowrap bgcolor="#dddddd">9.8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">45.8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Austria</td><td class="data-frame-td" nowrap bgcolor="#ffffff">8.9</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Bulgaria</td><td class="data-frame-td" nowrap bgcolor="#dddddd">7</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">64.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Denmark</td><td class="data-frame-td" nowrap bgcolor="#ffffff">5.8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">76.8</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.4</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Slovakia</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5.5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">82</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Finland</td><td class="data-frame-td" nowrap bgcolor="#ffffff">5.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.2</td><td class="data-frame-td" nowrap bgcolor="#ffffff">81</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.4</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Ireland</td><td class="data-frame-td" nowrap bgcolor="#dddddd">5</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">90.1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.3</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Croatia</td><td class="data-frame-td" nowrap bgcolor="#ffffff">4.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.9</td><td class="data-frame-td" nowrap bgcolor="#ffffff">110</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Lithuania</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2.8</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.6</td><td class="data-frame-td" nowrap bgcolor="#dddddd">160</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Slovenia</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.5</td><td class="data-frame-td" nowrap bgcolor="#ffffff">213</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Latvia</td><td class="data-frame-td" nowrap bgcolor="#dddddd">1.9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.4</td><td class="data-frame-td" nowrap bgcolor="#dddddd">234</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Estonia</td><td class="data-frame-td" nowrap bgcolor="#ffffff">1.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.3</td><td class="data-frame-td" nowrap bgcolor="#ffffff">337</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#dddddd">Cyprus</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.9</td><td class="data-frame-td" nowrap bgcolor="#dddddd">0.2</td><td class="data-frame-td" nowrap bgcolor="#dddddd">504</td><td class="data-frame-td" nowrap bgcolor="#dddddd">50.1</td><td class="data-frame-td" nowrap bgcolor="#dddddd">2</td></tr>
<tr><td class="data-frame-td" nowrap bgcolor="#ffffff">Luxembourg</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.6</td><td class="data-frame-td" nowrap bgcolor="#ffffff">0.1</td><td class="data-frame-td" nowrap bgcolor="#ffffff">714</td><td class="data-frame-td" nowrap bgcolor="#ffffff">50</td><td class="data-frame-td" nowrap bgcolor="#ffffff">2</td></tr>
<tr><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">Malta</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.5</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">0.1</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">869</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">50</td><td class="data-frame-td-bottom" nowrap bgcolor="#dddddd">2</td></tr>
</table>

We see now in the column `new.return.factor` that every country would have incentives to fund capacity expansions if the expected total benefits are as least as twice as high as the cost. Of course, countries could form groups and small countries without own pharmaceutical industry may well subsidize capacity expansions of firms situated in another country.

Sure, that approach would have lead to some inequality ex-post. EU members that funded more capacity expansions and bet on the right companies would get somewhat faster vaccinations than others. Yet, given that 30% of generated extra supply would be distributed across all member states, still everybody would benefit if a subset of countries funded capacity expansions and also countries outside of the EU would benefit if 20% of production is exported from the EU.

In an interview about the EU's vaccination speed Ursula von der Leyen stated:

> I am aware that alone a country can be a speedboat, while the EU is more like a tanker.

I believe that the European seas are big enough for a tanker and 27 speed boats and sincerely hope that in future crises the EU finds ways that do not force the speed boats to massively slow down but rather makes them effectively help pull along the tanker even if it means that some speed boats will be faster than others.

## Afterword: EU vs AstraZeneca

The criticism over EU's vaccine procurement flared up when AstraZeneca announced that from January to March it will deliver roughly 60% less dosages than it promised due to production delay in EU plants, while no delivery shortages for the UK were announced.

While I am no legal expert that behavior strikes me indeed as odd when reading [the contract](https://ec.europa.eu/info/sites/info/files/eu_apa_-_executed_-_az_redactions.pdf). In Section 5.4 it seems clearly written that also production from UK plants is planned to be delivered to the EU:

<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/covid/contract_54.png" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

OK, there is the term "Best Reasonable Efforts". Perhaps somewhere in the contract AstraZeneca wrote that it has earlier contractual obligations that prioritize other countries (e.g. UK for production from UK plants) in case of supply problems? Yet, in Section 13.1 of the contract it is written that there are no other obligations that would impede fulfillment of this contract:

<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/covid/contract_13_1.png" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

<center>
<a href="ejd.econ.mathematik.uni-ulm.de"><img src="http://skranz.github.io/images/covid/contract_13_1_b.png" style="max-width: 100%; margin-bottom: 0.5em;"></a>
</center>

So I don't see how one exerts "Best Reasonable Efforts" if one takes the EU's advance payment, signs a contract that stipulates deliveries also from UK plants and also states there are no other obligations that impede fulfillment, possibly even exports early produced dosages from the EU to the UK, and then when delivery shall take place in the EU suddenly cuts down supply by 60% only for the EU without cutting UK supply and refuses to supply the EU from UK plants. Maybe lawyers read this differently, but such an interpretation of "Best Reasonable Efforts" strikes me as very strange and I fear to ever learn what "normal efforts" mean.

That being said, if I would not have read the contract, as an economist I could very well understand that AstraZeneca feels in comparison little obligation to supply the EU early. Seemingly the EU paid [only $2.16 per shot](https://www.theguardian.com/world/2021/jan/22/south-africa-paying-more-than-double-eu-price-for-oxford-astrazeneca-vaccine), was unwilling to take over liability from producers by allowing faster emergency use authorizations, and took its time to sign the contract. So it looks as if AstraZeneca did not get very favorable conditions from the EU compared to other countries. 

Yet, if AstraZeneca was unhappy with the EU terms, why did AstraZeneca not just state clearly in the contract that the UK gets priority delivery from UK plants? If that would have been stated clearly, the EU might well have procured more dosages from more expensive but hopefully more reliable suppliers who then might have faster increased their production capacities.
