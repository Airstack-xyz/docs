---
description: >-
  Learn all the detailed references of TrendingMints API that provide trending
  ERC20/721/1155 token mints in a certain period of t ime, including the input
  filters, supported chains, and output fields.
---

# TrendingMints API

The TrendingMints API provides you with a list of trending tokens minted within a given time frame.

## Inputs

### filter

| Name      | Type                      | Description                          |
| --------- | ------------------------- | ------------------------------------ |
| `address` | `Trending_Comparator_Exp` | Token address of the trending mints. |

### audience

| Name        | Description                                                                |
| ----------- | -------------------------------------------------------------------------- |
| `farcaster` | Get all trending mints among **Farcaster user**s.                          |
| `all`       | Get all trending mints among all users in the Ethereum ecosystem globally. |

### criteria

| Name             | Description                                                                 |
| ---------------- | --------------------------------------------------------------------------- |
| `unique_wallets` | Sort the trending tokens by the number of unique wallets minting the token. |
| `total_mints`    | Sort the trending tokens by the number of total tokens minted.              |

### blockchain

| Name   | Description  |
| ------ | ------------ |
| `base` | Base mainnet |

### timeFrame

| Name          | Description                               |
| ------------- | ----------------------------------------- |
| `one_hour`    | Get trending mints from the last 1 hour.  |
| `two_hours`   | Get trending mints from the last 2 hours. |
| `eight_hours` | Get trending mints from the last 8 hours. |
| `one_day`     | Get trending mints from the last 1 day.   |
| `two_days`    | Get trending mints from the last 2 days.  |
| `seven_days`  | Get trending mints from the last 7 days.  |

## Outputs

| Name             | Type                     | Description                                                                    |
| ---------------- | ------------------------ | ------------------------------------------------------------------------------ |
| `address`        | `String`                 | Trending mints token address.                                                  |
| `audience`       | `String`                 | Trending mints based on given audience, either `farcaster` or `all`.           |
| `blockchain`     | `Blockchain`             | Blockchain where the trending token mints exist.                               |
| `chainId`        | `String`                 | Chain ID of the blockchain.                                                    |
| `criteria`       | `String`                 | Criteria to evaluate trending mints, either `unique_wallets` or `total_mints`. |
| `criteriaCount`  | `Int`                    | The total number for the criteria metric chosen.                               |
| `erc1155TokenID` | `String`                 | ERC1155 token ID. For other token type, this field will be an empty string.    |
| `id`             | `ID`                     | Airstack unique identifier for the token transfer                              |
| `timeFrom`       | `Time`                   | The time when the first mints within the time frame begins from.               |
| `timeTo`         | `Time`                   | The time when the first mints within the time frame ends.                      |
| `token`          | [`Token`](tokens-api.md) | \*\*Nested query\*\* - token contract level information                        |
