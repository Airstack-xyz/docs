---
description: Learn how to fetch POAPs in common from multiple users.
---

# POAPs

You can fetch common POAPs of multiple user(s) provided the user's 0x address, ENS domains, Lens profile, or Farcaster name or ID:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/0U2I2pZzcp" %}
Show common POAPs of two users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(
    input: {filter: {owner: {_eq: "0xeaf55242a90bb3289db8184772b0b98562053559"}}, blockchain: ALL, limit: 200}
  ) {
    Poap {
      poapEvent {
        poaps(input: {filter: {owner: {_eq: "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB"}}}) {
          poapEvent {
            eventName
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
                  "eventName": "You have met Patricio in April of 2023 (IRL)"
                }
              }
            ]
          }
        },
        {
          "poapEvent": {
            "poaps": [] // owned by 1st address, but not the 2nd address
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
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
