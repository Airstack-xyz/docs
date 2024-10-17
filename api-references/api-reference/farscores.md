---
description: >-
  Learn all the detailed references of FarScores API that provide Farcaster
  users' FarScores detailed information, including the input filters and output
  fields.
---

# FarScores

## Inputs

### filter

| Name               | Type                   | Description                                                  |
| ------------------ | ---------------------- | ------------------------------------------------------------ |
| `organicScore`     | `Float_Comparator_Exp` | Filter Farcaster users by the Organic FarScores.             |
| `organicScoreRank` | `Int_Comparator_Exp`   | Filter Farcaster users by the rank of Organic FarScores.     |
| `tvlBoost`         | `Float_Comparator_Exp` | Filter Farcaster users by the TVL boost.                     |
| `lpBoost`          | `Float_Comparator_Exp` | Filter Farcaster users by the LP boost.                      |
| `powerBoost`       | `Float_Comparator_Exp` | Filter Farcaster users by the Power Boost.                   |
| `farScore`         | `Float_Comparator_Exp` | Filter Farcaster users by their total FarScores.             |
| `farRank`          | `Int_Comparator_Exp`   | Filter Farcaster users by the rank of their total FarScores. |
| `fid`              | `Int_Comparator_Exp`   | Filter by Farcaster user's FID.                              |

### blockchain

{% hint style="info" %}
For **MoxieUserPortfolios** API, it will return user's Fan Tokens portfolio, whether the Fan Tokens are staked or not, on the Base chain.

You just need to specify the input for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

### order

| Name               | Description                                      |
| ------------------ | ------------------------------------------------ |
| `organicScore`     | Sort results by the organic FarScores.           |
| `organicScoreRank` | Sort results by the rank of organic FarScores.   |
| `tvlBoost`         | Sort results by the TVL Boosts.                  |
| `lpBoost`          | Sort results by the LP Boosts.                   |
| `powerBoost`       | Sort results by the Power Boosts.                |
| `farScore`         | Sort results by the total FarScores.             |
| `farRank`          | Sort results by the rank of the total FarScores. |

## Outputs

| Name               | Type                        | Descirption                                                   |
| ------------------ | --------------------------- | ------------------------------------------------------------- |
| `organicScore`     | `Float`                     | The organic FarScores of the Farcaster user.                  |
| `organicScoreRank` | `Int`                       | The rank of the organic FarScores of the Farcaster user.      |
| `tvlBoost`         | `Float`                     | The TVL boost of the Farcaster user.                          |
| `lpBoost`          | `Float`                     | The LP Boost of the Farcaster user.                           |
| `powerBoost`       | `Float`                     | The Power Boost of the Farcaster user.                        |
| `farScore`         | `Float`                     | The total FarScore of the Farcaster user.                     |
| `farRank`          | `Int`                       | The rank of the total FarScore of the Farcaster user.         |
| `social`           | [`Socials`](socials-api.md) | **Nested Query** – The profile details of the Farcaster user. |

