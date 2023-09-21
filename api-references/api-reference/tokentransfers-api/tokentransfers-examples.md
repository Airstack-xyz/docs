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

# TokenTransfers Examples

<details>

<summary>Get the token transfer history of the Mutant Apes NFT Collection on Ethereum</summary>

```graphql
query MyQuery {
  TokenTransfers(
    input: {filter: {tokenAddress: {_eq: "0x60e4d786628fea6478f785a6d7e704777c86a7c6"}}, blockchain: ethereum}
  ) {
    TokenTransfer {
      amount
      amounts
      blockNumber
      blockTimestamp
      blockchain
      chainId
      from {
        addresses
      }
      to {
        addresses
      }
      tokenType
    }
  }
}
```

</details>

<details>

<summary>Get token transfer history of stani.lens on Polygon</summary>

```graphql
query GetTokenTransfersFromStaniOnPolygon {
  polygonTransfers: TokenTransfers(
    input: {
      filter: {
        _or: [
          {from: {_eq: "stani.lens"}},
          {to: {_eq: "stani.lens"}}
        ]
      },
      blockchain: polygon, 
      limit: 10
    }
  ) {
    TokenTransfer {
      amount
      blockNumber
      blockTimestamp
      from {
        addresses
      }
      to {
        addresses
      }
      tokenAddress
      transactionHash
      tokenId
      tokenType
      blockchain
    }
  }
}

```

</details>
