---
description: >-
  Learn what FarScores (formerly Social Capital Score), FarBoosts, and Cast
  Scores (formerly Social Capital Value) are -- and how they're used to identify
  high-quality casts with the TrendingCasts API.
---

# 😎 FarScores, FarBoost, and Cast Scores

## FarScore

Every Farcaster Member earns a **Far Score** based on their relative influence in the Farcaster network (formerly known as "Social Capital Score"). Far Scores are dynamic (constantly updating) and ever-increasing. There are no negative factors affecting Far Scores.

Airstack also publishes the **Far Rank** for each Farcaster Member (formerly Social Capital Rank) which ranks all members from 1 to N based on their Far Scores. Member with rank 1 has the highest Far Score, 2 has second highest Far Score, etc, etc.)

### **FarBoost**

In addition, one can also increase their FarScore number by earning **FarBoost**. This can be done by **increasing the total value locked (TVL)** of Moxie token in the Moxie protocol.

The TVL is calculated as the summation of all Moxie token that you have used to buy Fan Tokens from the Bonding Curve contract and lock into the Moxie protocol. The TVL calculation will include the TVL contributed from all wallets that the user have, which includes Farcaster custodial wallet, Farcaster verified wallets, and their respective vesting contract.

For every **100,000 Moxie** added to the TVL, you will add **1 point** to your FarScore number.

In the API, FarScore shall automatically include the Farboost value in the calculation and will be reflected in the **FarRank** value that a Farcaster member has.

## Cast Score

Every cast and reply on Farcaster earns a **Cast Score** based on the Far Scores of the users who engage with the content and the type of engagement.

***

You can fetch the Far Score (SCS), Cast Score (SCV), and Far Rank (SCR) [via the Airstack APIs](https://app.airstack.xyz).
