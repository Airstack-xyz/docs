---
description: >-
  Learn how to use Airstack to get token-bound (ERC6551) accounts by the owner
  of NFT that owns the accounts and vice versa.
---

# 📭 NFT Owners

## Get Token Bound Accounts By NFT Owner Address

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($owner: [Identity!]) {
  TokenBalances(
    input: {filter: {owner: {_in: $owner}}, blockchain: ethereum, limit: 200}
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

{% tab title="Variable" %}
```json
{
  "owner": "0xcf94ba8779848141d685d44452c975c2ddc04945"
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
                    "0x9ff8faf2c61f50d24677e9cb5aaf988c91525539"
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    "0x5f27c4c4f66fbb9d1bddbcef60ada4731757b128"
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    "0x5661094b8b369aff4075a9a75a1bcc51cdb5901e"
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    "0x28d3fb76ad7e1076735a2bac3cac260c6349f45b"
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    "0x7bc2cb8d74c2238a126fd495c7ce079ae36e6396"
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

## Get NFT Owner Address By Token Bound Accounts Address

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($address: [Identity!]) {
  Accounts(input: {filter: {address: {_in: $address}}, blockchain: ethereum}) {
    Account {
      nft {
        tokenBalances {
          owner {
            addresses
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
  "address": "0x9ff8faf2c61f50d24677e9cb5aaf988c91525539"
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
                    "0xcf94ba8779848141d685d44452c975c2ddc04945"
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
