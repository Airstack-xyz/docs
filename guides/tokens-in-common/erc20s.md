---
description: Learn how to fetch ERC20 tokens in common from multiple users.
---

# ERC20s

You can fetch common ERC20 tokens of multiple user(s) provided the user's 0x address, ENS domains, Lens profile, or Farcaster name or ID:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/q4TkJCjV5f" %}
Show common ERC20 tokens held by users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_eq: "betashop.eth"}, tokenType: {_eq: ERC20}}, blockchain: polygon}
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {filter: {owner: {_eq: "tokenstaker.eth"}, tokenType: {_eq: ERC20}}}
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
            "tokenBalances": []
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
