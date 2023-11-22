---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# NFTStats API

The NFTStats API provides aggregated data for a specific NFT token within a collection (both ERC721 and ERC1155).

{% hint style="success" %}
Timeframe filter details (all times are in UTC) for when the sale transactions are captured for the aggregation:

Daily - starts from 00:00 am to 11:59 pm every day

Weekly - starts from 00:00 am on Monday to 11:59 pm on the same week Sunday

Monthly - 00:00 am on the first calendar day of the month to 11:59 pm on the last calendar day of the month. If querying the current month, the aggregation would be from the first calendar day to the most recent daily aggregation.

Yearly - 00:00 am on the first day of the year to 11:59 pm on the last day of the year. If querying the current year, the aggregation would be from the first calendar day to the most recent daily aggregation.

Lifetime - from the first recorded sale of the NFT to the most recent daily aggregation.
{% endhint %}

## Inputs

### Filters

| Name                             | Type                                 | Description                                               |
| -------------------------------- | ------------------------------------ | --------------------------------------------------------- |
| `averageSalePriceInUSDC`         | `Float_Comparator_Exp`               | NFT collection's average sale price in terms of USDC      |
| `dappName`                       | `MarketplaceDappName_Comparator_Exp` | Marketplace DApp name                                     |
| `dappSlug`                       | `MarketplaceDappSlug_Comparator_Exp` | Marketplace DApp slug (contract version)                  |
| `firstTransactionBlockTimestamp` | `Time_Comparator_Exp`                | Timestamp when NFT is first traded in Marketplace         |
| `highestSalePriceInUSDC`         | `Float_Comparator_Exp`               | NFT collection's highest sale price in terms of USDC      |
| `lastTransactionBlockTimestamp`  | `Time_Comparator_Exp`                | Timestamp when NFT is most recently traded in Marketplace |
| `lowestSalePriceInUSDC`          | `Float_Comparator_Exp`               | NFT collection's lowest sale price in terms of USDC       |
| `tokenAddress`                   | `Address_Comparator_Exp`             | NFT contract address on the blockchain                    |
| `tokenId`                        | `String_Comparator_Exp`              | NFT token ID                                              |
| `totalSalesCount`                | `Float_Comparator_Exp`               | NFT collection's total sale count                         |
| `totalSaleVolumeInUSDC`          | `Float_Comparator_Exp`               | NFT collection's total sale volume in terms of USDC       |
| `timeframe`                      | `TimeFrames!`                        | DAILY / WEEKLY / MONTHLY / YEARLY / LIFETIME              |

## Outputs

```graphql
type CollectionStat {
averageSalePriceInNativeToken: Float
averageSalePriceInUSDC: Float
blockchain: Blockchain # Blockchain where the NFT sale transaction took place
chainId: String # Unique blockchain identifier
dappName: # Marketplace DApp name
dappSlug: # Marketplace DApp slug (contract version)
dappVersion: String # Airstack unique dappVersion number
firstTransactionBlockTimestamp: Time
highestSalePriceInNativeToken: Float
highestSalePriceInUSDC: Float
highestSaleTransactionId: String
id: ID! # Airstack unique identifier for this particular element
lastTransactionBlockTimestamp: Time
lowestSalePriceInNativeToken: Float
lowestSalePriceInUSDC: Float
lowestSaleTransactionId: String
timeFrame: TimeFrames # DAILY / WEEKLY / MONTHLY / YEARLY / LIFETIME
tokenAddress: Address! # NFT contract address on the blockchain
tokenNft: # **Nested query** allowing to get NFT token data (images, traits, etc.)
tokenId: # Unique NFT token ID
totalFeeVolumeInNativeToken: Float
totalFeeVolumeInUSDC: Float
totalRoyaltyFeeVolumeInNativeToken: Float
totalRoyaltyFeeVolumeInUSDC: Float
totalSalesCount: Int
totalSaleVolumeInNativeToken: Float
totalSaleVolumeInUSDC: Float
}
```
