---
description: Learn how to fetch NFTs in common from multiple users.
---

# NFTs

You can fetch common NFTs of multiple user(s) provided the user's 0x address, ENS domains, Lens profile, or Farcaster name or ID:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/yKjShYXlAV" %}
Show common NFTs of two users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_eq: "betashop.eth"}, tokenType: {_in: [ERC721, ERC1155]}}, blockchain: polygon}
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {filter: {owner: {_eq: "tokenstaker.eth"}, tokenType: {_in: [ERC721, ERC1155]}}}
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
            "tokenBalances": [] // Owned by betashop.eth, but not owned by tokenstaker.eth
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

* [Nested Queries](../../api-references/nested-queries.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
