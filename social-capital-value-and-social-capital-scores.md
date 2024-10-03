---
description: >-
  Learn what FarScores (formerly Social Capital Score), FarBoosts, and Cast
  Scores (formerly Social Capital Value) are -- and how they're used to identify
  high-quality casts with the TrendingCasts API.
---

# 😎 FarScores, FarBoost, and Cast Scores

## FarScore

Every Farcaster Member earns a **Far Score** based on their relative influence in the Farcaster network (formerly known as "Social Capital Score"). Far Scores are dynamic (constantly updating) and ever-increasing. There are no negative factors affecting Far Scores.

Airstack also publishes the **Far Rank** for each Farcaster Member (formerly Social Capital Rank) which ranks all members from 1 to N based on their Far Scores. Member with rank 1 has the highest Far Score, 2 has second highest Far Score, etc, etc.).

**As of 3 October, 2024**, FarScores do not factor in follows from users who have not been active on Farcaster in the previous 60 days, on a rolling basis.

### **FarBoost**

In addition, one can also increase their FarScore number by earning **FarBoost**, which is defined by the following formula:

$$
F_{boost} =  TVL_{boost} + MP_{boost} + LP_{boost}
$$

where:

* $$F_{boost}$$: Farboost of a Farcaster user
* $$TVL_{boost}$$: TVL Boost of a Farcaster user
* $$MP_{boost}$$: Moxie Power Boost of a Farcaster user
* $$LP_{boost}$$: Liquidity Boost of a Farcaster user

#### TVL Boost

**TVL Boost** is defined as the **Farboost** earned by increasing the total value locked (TVL) of Moxie token in the Moxie protocol.

The TVL is calculated as the summation of all Moxie token that you have used to buy Fan Tokens from the Bonding Curve contract and lock into the Moxie protocol. The TVL calculation will include the TVL contributed from all wallets that the user have, which includes Farcaster custodial wallet, Farcaster verified wallets, and their respective vesting contract.

For every **100,000 Moxie** added to the TVL, you will add **1 point** to your FarScore number.

#### Liquidity Boost

**Liquidity Boost** is defined as the **Farboost** earned by increasing the amount of liquidity provided into Liquidity Pools in DEXs (Uniswap & Aerodrome).

For every **100,000 Moxie** added to the Liquidity Pool, you will add **1 point** to your FarScore number. Regardless of whether the LP token is then staked (**Aerodrome** only) or not, they'll be taken into account into the Farboost calculation.

Thus, given that a user have provided liquidity to liqudity pool $$P_1, P_2, ..., P_n$$, the Liquidity Boost you'll earn will be define by the following formula:

$$
TVL_{boost} = \frac{\sum_{i=1}^{n}M_{i} + r_{i} \cdot O_{i} }{100000}
$$

where:

* $$M_{i}$$: The amount of Moxie in liquidity pool $$P_i$$
* $$O_{i}$$: The amount of other token in the Moxie LP in liquidity pool $$P_i$$
* $$r_{i}$$: The reserve ratio between Moxie and the other token in liquidity pool $$P_i$$

***

In the API, FarScore shall automatically include the Farboost value in the calculation and will be reflected in the **FarRank** value that a Farcaster member has.

#### Moxie Power Boost

**Moxie Power Boost** is defined as the **Farboost** earned by increasing the amount of Fan Token locked/staked in the Moxie staking contract.

For every **100,000 Moxie** value locked/staked into the staking contract, you will add **1.2 point** to your FarScore number.

Thus,  given that a user have staked their FT in lock $$L_1, L _2, ..., L_m$$, the Moxie Power Boost you'll earn will be define by the following formula:

$$
MP_{boost} = mul \cdot \frac{\sum_{j=1}^{m}  B_j \cdot p_j  }{100000}
$$

where:

* $$mul$$: The multiplier, currently set at 1.2
* $$B_{j}$$: The balance of the FT locked in lock $$L_j$$
* $$p_{j}$$: The unit price of the FT locked in lock $$L_j$$

## Cast Score

Every cast and reply on Farcaster earns a **Cast Score** based on the Far Scores of the users who engage with the content and the type of engagement.

***

You can fetch the Far Score (SCS), Cast Score (SCV), and Far Rank (SCR) [via the Airstack APIs](https://app.airstack.xyz).
