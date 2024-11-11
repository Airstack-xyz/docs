---
description: >-
  FarcasterScore type provides detailed the arcasterf score of a Farcaster user
  to identify highly influential Farcaster users on the network.
---

# FarcasterScore

## Fields

| Name             | Type     | Description                                                                                                 |
| ---------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| `farcasterScore` | `String` | The [farcaster score](../../social-capital-value-and-social-capital-scores.md) of a user.                   |
| `farRank`        | `Int`    | The [far rank](../../social-capital-value-and-social-capital-scores.md) of a user.                          |
| `farScoreRaw`    | `Float`  | The raw [farcaster score](../../social-capital-value-and-social-capital-scores.md) of a user in Float type. |
| `liquidityBoost` | `Float`  | The Liquidity boost of a Farcaster user from providing Moxie liquidity in DEX.                              |
| `powerBoost`     | `Float`  | The Moxie Power boost of a Farcaster user from staking Moxie.                                               |
| `tvlBoost`       | `Float`  | The TVL boost of a Farcaster user from holding Fan Tokens.                                                  |
| `heroBoost`      | `Float`  | The Moxie Hero boost of a Farcaster user from being a Moxie Hero.                                           |
| `farBoost`       | `Float`  | The Farboost of the user, representing the total amount of boost user received on their organic FarScores.  |
| `tvl`            | `Float`  | The total value locked by users in Fan Tokens, both staked and not staked.                                  |
