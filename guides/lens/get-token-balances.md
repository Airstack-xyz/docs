---
description: >-
  Learn how to get ERC20, 721, 1155, and POAPs Of Lens Profile(s), including
  images and metadata, on Ethereum, Polygon, and Gnosis (POAPs).
---

# 💰 Get Token Balances

## Topics

* [Get All ERC20s Owned By Lens Profile(s)](get-token-balances.md#get-all-erc20s-owned-by-lens-profile-s)
* [Get All NFTs Owned By Lens Profile(s)](get-token-balances.md#get-all-nfts-owned-by-lens-profile-s)
* [Get All POAPs Owned By Lens Profile(s)](get-token-balances.md#get-all-poaps-owned-by-lens-profile-s)

## Get All ERC20s Owned By Lens Profile(s)

You can fetch all ERC20 tokens on Ethereum and Polygon owned by any Lens Profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/z5mwViK3bq" %}
Show ERC20 tokens on Ethereum and Polygon owned by bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query ERC20sOwnedByLensProfiles {
  Ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}, tokenType: {_eq: ERC20}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      token {
        name
        symbol
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Polygon: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}, tokenType: {_eq: ERC20}}, blockchain: polygon, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      token {
        name
        symbol
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

{% tab title="Response" %}
```json
{
  "data": {
    "Ethereum": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "bradorbradley.lens",
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "amount": "200000000000000000",
          "tokenAddress": "0x4d5f47fa6a74757f35c14fd3a6ef8e3c9bc514e8",
          "token": {
            "name": "Aave Ethereum WETH",
            "symbol": "aEthWETH"
          }
        }
      ]
    },
    "Polygon": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "bradorbradley.lens",
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "amount": "457218374987121000000",
          "tokenAddress": "0x086373fad3447f7f86252fb59d56107e9e0faafa",
          "token": {
            "name": "Yup",
            "symbol": "YUP"
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All NFTs Owned By Lens Profile(s)

You can fetch all NFTs on Ethereum and Polygon owned by any Lens Profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/skXQHLkXin" %}
Show NFT on Ethereum and Polygon owned by bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query NFTsOwnedByLensProfiles {
  Ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Polygon: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: polygon, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
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

{% tab title="Response" %}
```json
{
  "data": {
    "Ethereum": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "bradorbradley.lens",
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0xf5806ba5635911aa1d2b7b794172d55c731ca860",
          "tokenId": "8",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/1/0xf5806ba5635911aa1d2b7b794172d55c731ca860/8/extra_small",
                "small": "https://assets.airstack.xyz/image/nft/1/0xf5806ba5635911aa1d2b7b794172d55c731ca860/8/small",
                "medium": "https://assets.airstack.xyz/image/nft/1/0xf5806ba5635911aa1d2b7b794172d55c731ca860/8/medium",
                "large": "https://assets.airstack.xyz/image/nft/1/0xf5806ba5635911aa1d2b7b794172d55c731ca860/8/large"
              }
            }
          }
        }
      ]
    },
    "Polygon": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "bradorbradley.lens",
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x5ef718b8360ef2b82fb971b50350913e2bad4783",
          "tokenId": "2525",
          "tokenType": "ERC1155",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/large.png"
              }
            }
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All POAPs Owned By Lens Profile(s)

You can fetch all POAPs tokens owned by any Lens Profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/T4g7vpBWkX" %}
Show POAPs owned by bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByLensProfiles {
  Poaps(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}}, blockchain: ALL}
  ) {
    Poap {
      eventId
      poapEvent {
        eventName
        eventURL
        startDate
        endDate
        country
        city
        contentValue {
          image {
            extraSmall
            large
            medium
            original
            small
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "eventId": "47553",
          "poapEvent": {
            "eventName": "Proof of rAAVE - ETHCC Paris 2022",
            "eventURL": "https://aave.com",
            "startDate": "2022-07-01T00:00:00Z",
            "endDate": "2022-07-01T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/47553/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/47553/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/47553/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/47553/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/47553/small.png"
              }
            }
          }
        },
        {
          "eventId": "80122",
          "poapEvent": {
            "eventName": "You have met Patricio in November 2022 (IRL)",
            "eventURL": "http://poap.xyz",
            "startDate": "2022-11-01T00:00:00Z",
            "endDate": "2022-11-30T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/80122/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/80122/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/80122/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/80122/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/80122/small.png"
              }
            }
          }
        },
        {
          "eventId": "37964",
          "poapEvent": {
            "eventName": "AaveFam #18",
            "eventURL": "https://www.aave.com",
            "startDate": "2022-04-08T00:00:00Z",
            "endDate": "2022-04-08T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/37964/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/37964/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/37964/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/37964/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/37964/small.png"
              }
            }
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching token balances of Lens profile(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
