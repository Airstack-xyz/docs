# Wallet API Examples

<details>

<summary>For ENS dwr.eth, get Farcaster name, Farcaster account details, connected address, and all token balances and images</summary>

```graphql
query identity {
  Wallet(input: {identity: "dwr.eth", blockchain: ethereum}) {
    socials {
      dappName
      profileName
      profileCreatedAtBlockTimestamp
      userAssociatedAddresses
    }
    tokenBalances {
      tokenAddress
      amount
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            original
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

<summary>For stani.lens show all his web3 socials and NFTs on Polygon</summary>

```graphql
query staniLensSocialsAndNFTs {
  Wallet(input: {identity: "stani.lens", blockchain: polygon}) {
    socials {
      dappName
      profileName
    }
    tokenBalances {
      tokenType
      tokenNfts {
        contentValue {
          image {
            original
            extraSmall
            large
            medium
            small
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
