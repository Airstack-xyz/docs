---
description: >-
  Learn all the detailed references of TrendingSwaps API that provide trending
  ERC20 tokens in a certain period of time, including the input filters,
  supported chains, and output fields.
---

# TrendingSwaps API

The TrendingSwaps API provides you with a list of trending swaps that are frequently swapped within a given time frame.

## Inputs

### filter

| Name      | Type                      | Description                          |
| --------- | ------------------------- | ------------------------------------ |
| `address` | `Trending_Comparator_Exp` | Token address of the trending swaps. |

### criteria

| Name                      | Description                                                                   |
| ------------------------- | ----------------------------------------------------------------------------- |
| `buy_transaction_count`   | Sort the trending swaps by the number of buy transactions.                    |
| `sell_transaction_count`  | Sort the trending swaps by the number of sell transactions.                   |
| `total_transaction_count` | Sort the trending swaps by the number of total buy & sell transactions.       |
| `buy_volume`              | Sort the trending swaps by the number of buy volume.                          |
| `sell_volume`             | Sort the trending swaps by the number of sell volume.                         |
| `total_volume`            | Sort the trending swaps by the number of total buy & sell volume.             |
| `unique_buy_wallets`      | Sort the trending swaps by the number of unique buyer wallets.                |
| `unique_sell_wallets`     | Sort the trending swaps by the number of unique seller wallets.               |
| `total_unique_wallets`    | Sort the trending swaps by the number of total unique buyer & seller wallets. |

### blockchain

| Name       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |
| `base`     | Base mainnet     |

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

| Name                    | Type                     | Description                                                                                               |
| ----------------------- | ------------------------ | --------------------------------------------------------------------------------------------------------- |
| `address`               | `String`                 | Trending swaps token address.                                                                             |
| `blockchain`            | `Blockchain`             | Blockchain where the trending token exist.                                                                |
| `buyTransactionCount`   | `Int`                    | The number of buy transactions for the trending swaps token during the specified time frame.              |
| `buyVolume`             | `Float`                  | The buying volume of the trending swaps token.                                                            |
| `chainId`               | `String`                 | Chain ID of the blockchain.                                                                               |
| `criteria`              | `String`                 | Criteria that is chosen to evaluate trending swaps.                                                       |
| `id`                    | `ID`                     | Airstack unique identifier.                                                                               |
| `sellTransactionCount`  | `Int`                    | The number of sell transactions for the trending swaps token during the specified time frame.             |
| `sellVolume`            | `Float`                  | The selling volume of the trending swaps token.                                                           |
| `timeFrom`              | `Time`                   | The time when the first swaps within the time frame begins from.                                          |
| `timeTo`                | `Time`                   | The time when the first swaps within the time frame ends.                                                 |
| `token`                 | [`Token`](tokens-api.md) | **Nested query** - token contract level information                                                       |
| `totalTransactionCount` | `Int`                    | The number of total buy & sell transactions for the trending swaps token during the specified time frame. |
| `totalUniqueWallets`    | `Int`                    | The number of unique wallets buying and selling the trending swaps token.                                 |
| `totalVolume`           | `Float`                  | The buying & selling volume of the trending swaps token.                                                  |
| `uniqueBuyWallets`      | `Int`                    | The number of unique wallets buying the trending swaps token.                                             |
| `uniqueSellWallets`     | `Int`                    | The number of unique wallets selling the trending swaps token.                                            |
