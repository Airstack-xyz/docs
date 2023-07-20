---
description: Learn how to use Airstack to get all holders of NFT or POAP that has XMTP.
---

# 🗃 NFT & POAP Holders

## NFT Holders

Get the NFT holders that have XMTP using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API and provide an NFT contract address for the `$tokenAddress` input.

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($tokenAddress: Address!) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenAddress}}, blockchain: ethereum}
  ) {
    TokenBalance {
      owner {
        xmtp {
          isXMTPEnabled
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
  "tokenAddress": "0xc0f95066899efd7c0540b9474f81355a83e6f578" // NFT collection address
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
          "owner": {
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        {
          "owner": {
            "xmtp": []
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## POAP Holders

Get the POAP holders that have XMTP using the [Poaps](../../api-references/api-reference/poaps-api/) API and provide an POAP event ID for the `$eventId` input.

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPEventHoldersWithXMTP($eventId: String!) {
  Poaps(input: {filter: {eventId: {_eq: $eventId}}, blockchain: ALL}) {
    Poap {
      owner {
        addresses
        xmtp {
          isXMTPEnabled
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
  "eventId": "141910" // POAP event ID
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
          "owner": {
            "addresses": [
              "0xda85048c977134b09fc05cd3d1abd3a63e8edf4d"
            ],
            "xmtp": []
          }
        },
        {
          "owner": {
            "addresses": [
              "0x546457bbddf5e09929399768ab5a9d588cb0334d"
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
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
