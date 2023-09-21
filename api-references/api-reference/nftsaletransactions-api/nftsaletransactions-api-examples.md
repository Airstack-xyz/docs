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

# NFTSaleTransactions API Examples

<details>

<summary>Get the sales history and royalties for the Doodles NFT collection on Ethereum</summary>

```graphql
{
  NFTSaleTransactions(
    input: {filter: {dappName: {_eq: opensea}, nfts: {tokenAddress: {_eq: "0x8a90cab2b38dba80c64b7734e58ee1db38b8992e"}}}, blockchain: ethereum, limit: 10}
  ) {
    NFTSaleTransaction {
      id
      from {
        identity
      }
      to {
        identity
      }
      paymentAmount
      paymentToken {
        symbol
      }
      blockTimestamp
      nfts {
        tokenId
      }
      royalties {
        beneficiaryAddress
        amount
        formattedAmount
      }
    }
    pageInfo {
      prevCursor
      nextCursor
    }
  }
}
```

</details>
