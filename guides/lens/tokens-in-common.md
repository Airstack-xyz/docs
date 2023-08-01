---
description: >-
  Learn how to fetch ERC20, NFTs, or POAPs in common from multiple Lens
  profiles.
---

# 🤝 Tokens In Common

## Topics

* [ERC20 Tokens In Common Owned By Lens Profile(s)](tokens-in-common.md#erc20-tokens-in-common-owned-by-lens-profile-s)
* [NFTs In Common Owned By Lens Profile(s)](tokens-in-common.md#nfts-in-common-owned-by-lens-profile-s)
* [POAPs In Common Owned By Lens Profile(s)](tokens-in-common.md#poaps-in-common-owned-lens-profile-s)

## ERC20 Tokens In Common Owned By Lens Profile(s)

You can fetch common ERC20 tokens of multiple Lens profiles:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/gHp07gzekM" %}
Show common ERC20 tokens held by Lens profiles
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_eq: "bradorbradley.lens"}, tokenType: {_eq: ERC20}}, blockchain: polygon}
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {filter: {owner: {_eq: "christina.lens"}, tokenType: {_eq: ERC20}}}
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
                "id": "bd292d232f10973466326db331c17ca93bff3be12fec404dc6b611001caacac9",
                "token": {
                  "address": "0x086373fad3447f7f86252fb59d56107e9e0faafa",
                  "symbol": "YUP",
                  "name": "Yup"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by bradorbradley.lens, but not owned by christina.lens
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## NFTs In Common Owned By Lens Profile(s)

You can fetch common NFTs of multiple Lens profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/Uj52BnGzaR" %}
Show common NFTs of two Lens profiles
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_eq: "bradorbradley.lens"}, tokenType: {_in: [ERC721, ERC1155]}}, blockchain: polygon}
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {filter: {owner: {_eq: "christina.lens"}, tokenType: {_in: [ERC721, ERC1155]}}}
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
                "id": "d51975aef4b2c638e9045bfd59f984035c49c0452bca092d1bc79ce5143b395a",
                "token": {
                  "address": "0x5c9d8fcd43847af51810cdfaeabee4ffad82d331",
                  "symbol": "meyt-Fl",
                  "name": "meytab.lens-Follower"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by bradorbradley.lens, but not owned by christina.lens
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## POAPs In Common Owned Lens Profile(s)

You can fetch common POAPs of multiple Lens profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/mqcXKeXp19" %}
Show common POAPs of two Lens profiles
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(
    input: {filter: {owner: {_eq: "bradorbradley.lens"}}, blockchain: ALL, order: {createdAtBlockNumber: DESC}}
  ) {
    Poap {
      poapEvent {
        poaps(input: {filter: {owner: {_eq: "christina.lens"}}}) {
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
                  "eventName": "Avara Genesis Fam",
                  "eventId": "105708",
                  "endDate": "2023-02-24T00:00:00Z",
                  "country": "",
                  "city": ""
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by bradorbradley.lens, but not owned by christina.lens
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

If you have any questions or need help regarding fetching common ERC20 tokens, NFTs, or POAPs of multiple Lens profiles, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Tokens In Common Guides](../tokens-in-common/)
* [Nested Queries](../../api-references/nested-queries.md)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
