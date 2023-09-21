---
description: >-
  Learn how to construct queries to build a recommendation engine based on token
  transfers.
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

# 💸 Token Transfers

Frequent token transfers between two parties could be a good indication that the parties involved know each other. Therefore, you can also use token transfers to build your contact recommendation feature.

To build such a contact recommendation feature, Airstack provides a `TokenTransfers` API for you to fetch all ERC20 token transfer data from either a user or multiple users on both Ethereum on Polygon.

## Get Token Transfers Of A User(s) on Ethereum

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers($address: [Identity!]) {
  TokenTransfers(
    input: {filter: {_or: {from: {_in: $address}, to: {_in: $address}}}, blockchain: ethereum, limit: 50}
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          dappName
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
    }
  }
}
```
{% endtab %}

{% tab title="Variable" %}
```json
{
  "address": ["dwr.eth", "fc_fname:vbuterin"]
}
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Of A User(s) on Multiple Chains

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers($address: [Identity!]) {
  # first query on Ethereum
  ethereum: TokenTransfers(
    input: {filter: {_or: {from: {_in: $address}, to: {_in: $address}}}, blockchain: ethereum, limit: 50}
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          dappName
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
      blockchain
    }
  }
  # second query on Polygon
  polygon: TokenTransfers(
    input: {filter: {_or: {from: {_in: $address}, to: {_in: $address}}}, blockchain: polygon, limit: 50}
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          dappName
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
      blockchain
    }
  }
}
```
{% endtab %}

{% tab title="Variable" %}
```json
{
  "address": ["prxshant.eth", "fc_fname:vbuterin"]
}
```
{% endtab %}
{% endtabs %}

## Get The Most Recent Token Transfers Of A User(s)

This query is similar to the basic [Get Token Transfers Of A User(s)](token-transfers.md#get-token-transfers-of-a-user-s-on-ethereum) query shown above with the addition of a new variable `$blocktimestamp` that will filter and return the token transfers that occur prior to the given timestamp value in the variable.

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers($address: [Identity!], $blocktimestamp: Time) {
  TokenTransfers(
    input: {filter: {_or: {from: {_in: $address}, to: {_in: $address}, blockTimestamp: {_lt: $blocktimestamp}}}, blockchain: ethereum, limit: 50}
  ) {
    TokenTransfer {
      amount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          dappName
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
    }
  }
}
```
{% endtab %}
{% endtabs %}
