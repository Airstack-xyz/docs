---
description: >-
  Learn how to get ERC6551 Accounts owned By Farcaster user(s). Get Farcaster
  users who own NFTs owned by ERC6551 accounts.
---

# 🪆 Tokenbound ERC6551 Accounts

## Topics

* [Get ERC6551 Account Addresses of Farcaster user(s)](tokenbound-erc6551-accounts.md#get-erc6551-account-addresses-of-farcaster-user-s)
* [Get Assets Owned by ERC6551 Accounts of Farcaster user(s)](tokenbound-erc6551-accounts.md#get-assets-owned-by-erc6551-accounts-of-farcaster-user-s)
* [Get Farcaster of Owner that Owns NFT that Owns ERC6551 Accounts](tokenbound-erc6551-accounts.md#get-farcaster-of-owner-that-owns-nft-that-owns-erc6551-accounts)

## Get ERC6551 Account Addresses of Farcaster user(s)

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/QW7bsKk9tX" %}
Show all ERC6551 accounts owned by Farcaster user name jayden
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_in: ["fc_fname:jayden"]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      tokenNfts {
        erc6551Accounts {
          address {
            addresses
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": null
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Assets Owned by ERC6551 Accounts of Farcaster user(s)

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/Sn45RpQ4Mh" %}
Show all assets owned by ERC6551 accounts owned by Farcaster user name jayden
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_in: ["fc_fname:jayden"]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      tokenNfts {
        erc6551Accounts {
          address {
            addresses
            tokenBalances {
              tokenAddress
              tokenId
              amount
              token {
                name
                symbol
                tokenNfts {
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
                  ],
                  "tokenBalances": [
                    {
                      "tokenAddress": "0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1",
                      "tokenId": "9707",
                      "amount": "1",
                      "token": {
                        "name": "STAPLEVERSE - FEED CLAN",
                        "symbol": "FEED",
                        "tokenNfts": [
                          {
                            "contentValue": {
                              "image": {
                                "extraSmall": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/extra_small",
                                "large": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/large",
                                "medium": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/medium",
                                "original": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/original_image",
                                "small": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/small"
                              }
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": null
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster of Owner that Owns NFT that Owns ERC6551 Accounts

You can know whether an NFT is owned by an ERC6551 accounts and the NFT that own the account:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/Kx7NGiw3i8" %}
Show owner of NFT that owns ERC6551 account with address 0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {filter: {address: {_in: "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"}}, blockchain: ethereum}
  ) {
    Account {
      nft {
        tokenBalances {
          owner {
            addresses
            socials(input: {filter: {dappName: {_eq: farcaster}}}) {
              profileName
              userId
            }
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
    "Accounts": {
      "Account": [
        {
          "nft": {
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0xa75b7833c78eba62f1c5389f811ef3a7364d44de"
                  ],
                  "socials": [
                    {
                      "profileName": "jayden",
                      "userId": "843"
                    }
                  ]
                }
              }
            ]
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

If you have any questions or need help regarding fetching token bound ERC6551 accounts, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Tokenbound ERC6551 Guides](../token-bound-accounts/)
* [Accounts API References](../../api-references/api-reference/accounts-api/)
* [TokenBalances API References](../../api-references/api-reference/tokenbalances-api/)
