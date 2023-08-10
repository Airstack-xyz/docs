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
Show ERC20 tokens on Ethereum and Polygon owned by bradorbradley.lens and Lens profile id 100275
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query ERC20OwnedByLensProfiles {
  Ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens", "lens_id:100275"]}, tokenType: {_eq: ERC20}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
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
    input: {filter: {owner: {_in: ["bradorbradley.lens", "lens_id:100275"]}, tokenType: {_eq: ERC20}}, blockchain: polygon, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "vitalik.lens",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "45934484403886362668",
          "tokenAddress": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "token": {
            "name": "Wrapped Ether",
            "symbol": "WETH"
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
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
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
Show NFT on Ethereum and Polygon owned by bradorbradley.lens and Lens profile id 100275
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query NFTsOwnedByLensProfiles {
  Ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens", "lens_id:100275"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
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
    input: {filter: {owner: {_in: ["bradorbradley.lens", "lens_id:100275"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: polygon, limit: 50}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "vitalik.lens",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
          "tokenId": "93631715144692179688067815556165775057916676179424585455268666624027958254283",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/extra_small.svg",
                "small": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/small.svg",
                "medium": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/medium.svg",
                "large": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/large.svg"
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
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
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
Show POAPs owned by bradorbradley.lens and Lens profile id 100275
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByLensProfiles {
  Poaps(
    input: {filter: {owner: {_in: ["bradorbradley.lens", "lens_id:100275"]}}, blockchain: ALL}
  ) {
    Poap {
      eventId
      owner {
        socials(input: { filter: { dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
      }
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
          "eventId": "80393",
          "owner": {
            "socials": [
              {
                "profileName": "vitalik.lens",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "poapEvent": {
            "eventName": "You met Cryptocomical at Lisbon 2022",
            "eventURL": "https://cards.io",
            "startDate": "2022-10-30T00:00:00Z",
            "endDate": "2022-11-15T00:00:00Z",
            "country": "Portugal",
            "city": "Lisbon",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/small.png"
              }
            }
          }
        },
        {
          "eventId": "47553",
          "owner": {
            "socials": [
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "poapEvent": {
            "eventName": "Proof of rAAVE - ETHCC Paris 2022",
            "eventURL": "https://aave.com",
            "startDate": "2022-07-01T00:00:00Z",
            "endDate": "2022-07-01T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/small.png"
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
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
