---
description: >-
  Learn all the detailed references of TrendingTokens API that provide trending
  ERC20 tokens in a certain period of time, including the input filters,
  supported chains, and output fields.
---

# TrendingTokens API

The TrendingTokens API provides you with a list of trending tokens frequently transferred within a given time frame.

## Inputs

### filter

| Name      | Type                      | Description                           |
| --------- | ------------------------- | ------------------------------------- |
| `address` | `Trending_Comparator_Exp` | Token address of the trending tokens. |

### transferType

| Name             | Description                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------------- |
| `all`            | Configure to include airdropped & self-transferred tokens in the calculation & analysis.      |
| `self_initiated` |  Configure to exclude airdropped and self-transferred tokens from the calculation & analysis. |

### audience

| Name        | Description                                                               |
| ----------- | ------------------------------------------------------------------------- |
| `farcaster` | Get all trending tokens for **Farcaster user**s.                          |
| `all`       | Get all trending tokens for all users in the Ethereum ecosystem globally. |

### criteria

| Name              | Description                                                                 |
| ----------------- | --------------------------------------------------------------------------- |
| `unique_wallets`  | Sort the trending tokens by the number of unique wallets minting the token. |
| `total_transfers` | Sort the trending tokens by the number of total tokens minted.              |
| `unique_holders`  | Sort the trending tokens by the number of unique holders.                   |

### blockchain

| Name    | Description    |
| ------- | -------------- |
| `base`  | Base mainnet   |
| `degen` | Degen Chain L3 |

### timeFrame

| Name          | Description                               |
| ------------- | ----------------------------------------- |
| `one_hour`    | Get trending mints from the last 1 hour.  |
| `two_hours`   | Get trending mints from the last 2 hours. |
| `eight_hours` | Get trending mints from the last 8 hours. |
| `one_day`     | Get trending mints from the last 1 day.   |
| `two_days`    | Get trending mints from the last 2 days.  |
| `seven_days`  | Get trending mints from the last 7 days.  |

### swappable

| Name  | Description                                                                                                                             |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `_eq` | Equal operator, assign `true` to only fetch swappable tokens. Otherwise, all tokens, whether swappable or not in DEX, will be included. |

## Outputs

| Name            | Type                     | Description                                                                         |
| --------------- | ------------------------ | ----------------------------------------------------------------------------------- |
| `address`       | `String`                 | Trending token address.                                                             |
| `audience`      | `String`                 | Trending tokens based on given audience, either `farcaster` or `all`.               |
| `blockchain`    | `Blockchain`             | Blockchain where the trending token exist.                                          |
| `chainId`       | `String`                 | Chain ID of the blockchain.                                                         |
| `criteria`      | `String`                 | Criteria to evaluate trending tokens, either `unique_wallets` or `total_transfers`. |
| `criteriaCount` | `Int`                    | The total number for the criteria metric chosen.                                    |
| `id`            | `ID`                     | Airstack unique identifier.                                                         |
| `timeFrom`      | `Time`                   | The time when the first transfer within the time frame begins from.                 |
| `timeTo`        | `Time`                   | The time when the first transfer within the time frame ends.                        |
| `token`         | [`Token`](tokens-api.md) | \*\*Nested query\*\* - token contract level information                             |
