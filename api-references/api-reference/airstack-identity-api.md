# Airstack Identity API

Airstack Identity API has been integrated throughout all of the GraphQL queries! You can filter your queries using the identity inputs by passing the following values instead of a blockchain address:

#### **ENS** input - uses the resolved address to get the data.

You can enter ENS address directly in the “owner/address” field, for example:

* <mark style="color:red;">`vitalik.eth`</mark>
* <mark style="color:red;">`ens:vitalik.eth`</mark> - “ens:” prefix is optional

#### **Farcaster** input

You can enter Farcaster inputs in the “address” field as follows:

* <mark style="color:red;">`fc_fid:5650`</mark> - use “fc\_fid:” prefix followed by the Farcaster user ID
* <mark style="color:red;">`fc_fname:vbuterin`</mark> - use “fc\_fname:” prefix followed by the Farcaster user ID

Alternatively, you can enter the Farcaster profile name directly in the Socials profileName field.

#### Lens input - use the wallet address the Lens Profile NFT is located in.

You can enter Lens name directly in the “owner/address” field, for example:

* <mark style="color:red;">`vitalik.lens`</mark>
* <mark style="color:red;">`lens:vitalik.lens`</mark> - “lens:” prefix is optional

<details>

<summary>Show me all NFTs and their images currently being held by Farcaster user: dwr</summary>

```graphql
query ID1 {
  TokenBalances(input: {filter: {owner: {_eq: "fc_fname:dwr"}, tokenType: {_in: [ERC721, ERC1155]}}, blockchain: ethereum}) {
    TokenBalance {
      tokenNfts {
        address
        tokenId
        contentValue {
          image {
            medium
          }
        }
        token {
          name
        }
      }
    }
  }
}
```

</details>

<details>

<summary>Show me all stani.lens token transfers</summary>

```graphql
query tokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        _or: [
          { from: { _eq: "stani.lens" } },
          { to: { _eq: "stani.lens" } }
        ]
      },
      blockchain: ethereum
    }
  ) {
    TokenTransfer {
      amount
      tokenType
      tokenAddress
      tokenId
    }
  }
}
```

</details>
