---
description: Learn fields that have nested queries implemented within the Airstack API.
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

# Nested Queries

The Airstack API supports nested queries on several fields with input filter types:

| Fields                | Filter Type                                                                   |
| --------------------- | ----------------------------------------------------------------------------- |
| `erc6551Accounts`     | [`AccountFilter`](api-reference/accounts-api/#inputs-and-filters)             |
| `poaps`               | [`PoapFilter`](api-reference/poaps-api/#inputs-and-filters)                   |
| `socials`             | [`SocialFilter`](api-reference/socials-api/#inputs-and-filters)               |
| `tokenBalances`       | [`TokenBalanceFilter`](api-reference/tokenbalances-api/#inputs-and-filters)   |
| `tokenTransfers`      | [`TokenTransferFilter`](api-reference/tokentransfers-api/#inputs-and-filters) |
| `tokenNfts`           | [`TokenNftFilter`](api-reference/tokennfts-api/#inputs-and-filters)           |
| `nftSaleTransactions` | [`NFTSaleTransactionFilter`](broken-reference)                                |
