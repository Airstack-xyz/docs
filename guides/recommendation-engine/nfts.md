---
description: >-
  Learn how to construct queries to build a recommendation engine based on NFTs
  held by the user(s).
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

# ♦ NFTs

NFTs tend to be built around the community or DAO members holding it and the holders of the same NFT likely have the same interests that can be a solid basis for contact recommendations.

Building such a contract recommendation feature based on NFTs takes two steps:

1. Query Get All NFTs Held By A User(s)
2. Using the array of NFTs returned from step 1, use it as an input to query Get All NFT Holders And Images Of An NFT Collection(s)

[Airstack](https://www.airstack.xyz/) provides you the `TokenBalances` API to fetch the NFT held by a user(s) and the `TokenNFTs` API to fetch all the holders of a certain NFT collection.

With [Airstack](https://airstack.xyz), it's possible to do [cross-chain queries](../contact-recommendation/broken-reference/) that get NFT results from **both Ethereum and Polygon** in a single call.

## Get All NFTs Held By A User(s)

First, you will fetch all the NFTs held by a user(s) to create the contact recommendation. All you need to provide the `address` variable, which takes the user(s) address that the recommendation will be given to, to the following query:

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTs($address: [Identity!]) {
  ethereum: TokenBalances(
    input: {filter: {owner: {_in: $address}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 10}
  ) {
    TokenBalance {
      tokenAddress
      amount
      formattedAmount
      tokenType
      owner {
        addresses
        socials {
          profileName
          userAssociatedAddresses
        }
      }
      tokenNfts {
        address
        tokenId
        blockchain
        contentValue {
          image {
            original
          }
        }
      }
    }
  }
  polygon: TokenBalances(
    input: {filter: {owner: {_in: $address}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: polygon, limit: 10}
  ) {
    TokenBalance {
      tokenAddress
      amount
      formattedAmount
      tokenType
      owner {
        addresses
        socials {
          profileName
          userAssociatedAddresses
        }
      }
      tokenNfts {
        address
        tokenId
        blockchain
        contentValue {
          image {
            original
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variable" %}
```json
{
  "address": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"]
}
```
{% endtab %}
{% endtabs %}

## Get All NFT Holders And Images Of An NFT Collection(s)

Once you have the list of NFT collections that was fetched from the first query above, you can format and compile the list of all NFT contract addresses and provide it as an `address` input to the query below.

The query below will fetch all the NFT holders’ web3 identities, which include the Ethereum addresses, ENS, Lens, and Farcasters.

In addition, the query will also fetch all NFT images that you can use to be displayed in your application.

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTHoldersAndImages($address: [Address!]) {
  ethereum: TokenNfts(
    input: {filter: {address: {_in: $address}}, blockchain: ethereum}
  ) {
    TokenNft {
      address
      tokenId
      contentValue {
        image {
          extraSmall
          small
          medium
          large
          original
        }
      }
      tokenBalances {
        owner {
          domains {
            name
            isPrimary
          }
          identity
          socials {
            profileName
            dappName
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  polygon: TokenNfts(
    input: {filter: {address: {_in: $address}}, blockchain: polygon}
  ) {
    TokenNft {
      address
      tokenId
      contentValue {
        image {
          extraSmall
          small
          medium
          large
          original
        }
      }
      tokenBalances {
        owner {
          domains {
            name
            isPrimary
          }
          identity
          socials {
            profileName
            dappName
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}
{% endtabs %}

<details>

<summary>Example #1: Get All @Nouns Holders ENS and Images</summary>

```graphql
query GetNFTHoldersAndImages {
  TokenNfts(input: {filter: {address: {_eq: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03"}}, blockchain: ethereum}) {
    TokenNft {
      address
      tokenId
      contentValue {
        image {
          extraSmall
          small
          medium
          large
          original
        }
      }
      tokenBalances {
        owner {
          domains {
            name
            isPrimary
          }
          identity
          socials {
            profileName
            dappName
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```

</details>

## Developer Support

If you have any questions or need help regarding integrating or building recommendation engine with NFT data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
* [On-Chain Graph](../onchain-graph.md)
