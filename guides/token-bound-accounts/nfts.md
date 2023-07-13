---
description: >-
  Learn how to use Airstack to get token-bound (ERC6551) accounts by NFTs that
  own the accounts and vice versa
---

# ♦ NFTs

## Get Token Bound Accounts (ERC6551) By NFT address(es)

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($tokenAddress: [Address!]) {
  Accounts(
    input: {filter: {tokenAddress: {_in: $tokenAddress}}, blockchain: ethereum, limit: 200}
  ) {
    Account {
      address {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
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
  "tokenAddress": ["0x26727ed4f5ba61d3772d1575bca011ae3aef5d36"]
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
          "address": {
            "addresses": [
              "0x776c56774a83a80c6ab608385418f6ffb2e088fb"
            ],
            "domains": [
              {
                "isPrimary": true,
                "name": "skynft.eth"
              }
            ],
            "socials": [
              {
                "profileName": "0xjst.lens",
                "dappName": "lens"
              },
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

## Get Token Bound Accounts By Specific NFT

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($tokenAddress: Address!, $tokenId: String!) {
  Accounts(
    input: {filter: {tokenAddress: {_eq: $tokenAddress}, tokenId: {_eq: $tokenId}}, blockchain: ethereum, limit: 200}
  ) {
    Account {
      address {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
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
  "tokenAddress": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
  "tokenId": "1"
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
          "address": {
            "addresses": [
              "0x718a9d173e66c411f48e41d3da2fa6f0ce8f5d3c"
            ],
            "domains": [
              {
                "isPrimary": true,
                "name": "skynft.eth"
              }
            ],
            "socials": [
              {
                "profileName": "0xjst.lens",
                "dappName": "lens"
              },
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

## Get Owner NFT of a Token Bound Account

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($erc6551Address: [Identity!]) {
  Accounts(
    input: {blockchain: ethereum, filter: {address: {_in: $erc6551Address}}}
  ) {
    Account {
      nft {
        address
        metaData {
          name
          description
          image
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
  "erc6551Address": "0xa1afb6a11ef500229538bfb38d5a0b8c1b61b425"
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
            "address": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
            "metaData": {
              "name": "Sapienz #3504",
              "description": "SAPIENZ is a collection of 15,000 networked playable characters created in partnership between STAPLEVERSE and RHYMEZLIKEDIMEZ. Imagine a reality where the marriage of art and commerce is seamless, where creativity is the currency and collaboration the fuel that powers our collective evolution. This is not merely a vision of the future, but a tangible reality within your grasp with the SAPIENZ world. The goal is to build the next 100+ years of street culture, are you in?",
              "image": "https://sapienz.imgix.net/rendered/3504.png?s=5d9251a2fe167597a1f5fbdf12e84ab3"
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

