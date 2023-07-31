---
description: >-
  Learn how to fetch ERC20, NFTs, or POAPs in common from multiple Farcaster
  users.
---

# 🤝 Tokens In Common

## ERC20 Tokens In Common of Farcaster User(s)

You can fetch common ERC20 tokens of multiple Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/QIFCtkidcx" %}
Show common ERC20 tokens held by users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_eq: "fc_fname:betashop.eth"}, tokenType: {_eq: ERC20}}, blockchain: polygon}
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {filter: {owner: {_eq: "fc_fname:sarvesh"}, tokenType: {_eq: ERC20}}}
        ) {
          id
          token {
            address
            symbol
            name
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
          "token": {
            "tokenBalances": [
              {
                "id": "51012959a2b4c04f8dba7ef03eff0ded99c7892e8367d023680d66825287fbba",
                "token": {
                  "address": "0xef556c61263135ad91a27c8c891e61e521560240",
                  "symbol": "usd-rewards.xyz",
                  "name": "USD"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by betashop.eth, but not owned by sarvesh
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## NFTs In Common

You can fetch common NFTs of multiple Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/chCICTPZo6" %}
Show common NFTs of two Farcaster users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_eq: "fc_fname:betashop.eth"}, tokenType: {_in: [ERC721, ERC1155]}}, blockchain: polygon}
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {filter: {owner: {_eq: "fc_fname:sarvesh"}, tokenType: {_in: [ERC721, ERC1155]}}}
        ) {
          id
          token {
            address
            symbol
            name
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
          "token": {
            "tokenBalances": [
              {
                "id": "05c4cece4238243f6f8fd6c22e0df78fa07450283322c476dc4af2e2a744c6d3",
                "token": {
                  "address": "0x4c23d718416164b2f52f88dde4cf0b1172b741b6",
                  "symbol": "swap-rewards.xyz",
                  "name": "swap-rewards.xyz"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by betashop.eth, but not owned by sarvesh
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## POAPs In Common

You can fetch common POAPs of multiple Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/7RtCNBtLUA" %}
Show common POAPs of two Farcaster users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(
    input: {filter: {owner: {_eq: "fc_fname:betashop.eth"}}, blockchain: ALL, order: {createdAtBlockNumber: DESC}}
  ) {
    Poap {
      poapEvent {
        poaps(input: {filter: {owner: {_eq: "fc_fname:ipeciura"}}}) {
          poapEvent {
            eventName
            eventId
            endDate
            country
            city
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
          "poapEvent": {
            "poaps": [
              {
                "poapEvent": {
                  "eventName": "Airstack Amazing Race Dubai",
                  "eventId": "145995",
                  "endDate": "2023-07-30T00:00:00Z",
                  "country": "UAE",
                  "city": "Dubai"
                }
              }
            ]
          }
        },
        {
          "poapEvent": {
            "poaps": [] // owned by betsahop.eth, but not ipeciura
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

If you have any questions or need help regarding fetching common ERC20 tokens, NFTs, or POAPs of multiple users, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Tokens In Common Guides](../tokens-in-common/)
* [Nested Queries](../../api-references/nested-queries.md)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
