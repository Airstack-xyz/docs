---
description: Learn how to recommend users based on NFTs
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

# ♦️ NFTs

NFTs are often built around communities of common interests and can be a solid basis for contact recommendations.

Building such a contract recommendation feature based on NFTs takes two steps:

1. Query Get All NFTs Held By A User(s)
2. Using the array of NFTs returned from step 1, use it as an input to query Get All NFT Holders And Images Of An NFT Collection(s)

[Airstack](https://www.airstack.xyz/) provides you the `TokenBalances` API to fetch the NFT held by a user(s) and the `TokenNFTs` API to fetch all the holders of a certain NFT collection.

With [Airstack](https://airstack.xyz), it's possible to do [cross-chain queries](../contact-recommendation/broken-reference/) that get NFT results from **multiple chains, e.g. Ethereum, Gold, Base, and Zora**, in a single call.

## Get All NFTs Held By A User(s)

First, you will fetch all the NFTs held by a user(s) to create the contact recommendation. All you need to provide the `address` variable, which takes the user(s) address that the recommendation will be given to, to the following query:

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTs($Identity: [Identity!]) {
  ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _in: $Identity }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      tokenAddress
      amount
      formattedAmount
      tokenType
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
    pageInfo {
      nextCursor
      prevCursor
      hasNextPage
      hasPrevPage
    }
  }
  polygon: TokenBalances(
    input: {
      filter: {
        owner: { _in: $Identity }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      tokenAddress
      amount
      formattedAmount
      tokenType
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
    pageInfo {
      nextCursor
      prevCursor
      hasNextPage
      hasPrevPage
    }
  }
  base: TokenBalances(
    input: {
      filter: {
        owner: { _in: $Identity }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      tokenAddress
      amount
      formattedAmount
      tokenType
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
    pageInfo {
      nextCursor
      prevCursor
      hasNextPage
      hasPrevPage
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "Identity": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"]
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
    input: { filter: { address: { _in: $address } }, blockchain: ethereum }
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

{% tab title="Variables" %}
```
{
  "address": ["0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03"]
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating or building recommendation engine with NFT data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)
* [On-Chain Graph](../onchain-graph.md)
