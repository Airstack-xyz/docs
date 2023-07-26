---
description: >-
  Learn how to fetch the list of common POAP holders or event attendees of
  multiple POAPs.
---

# Multiple POAPs

## Common Holders of 2 POAPs

You can fetch the common holders of two given POAP event IDs:

<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfTwoPOAPs($eventIdA: String!, $eventIdB: String!) {
  Poaps(input: {filter: {eventId: {_eq: $eventIdA}}, blockchain: ALL, limit: 200}) {
    Poap {
      owner {
        poaps(input: {blockchain: ALL, filter: {eventId: {_eq: $eventIdB}}}) {
          owner {
<strong>            addresses
</strong>          }
        }
      }
    }
  }
}
</code></pre>

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

## Common Holders of More Than 2 POAPs

Fetching common holders for more than 2 ERC20 tokens works the same way as fetching only 2 ERC20 tokens with the addition of more POAP event ID parameters `$eventIdC`, `$eventIdD`, `$eventIdE`, etc:

```graphql
query GetCommonHoldersOfMoreThanTwoPOAPs(
  $eventIdA: String!,
  $eventIdB: String!,
  $eventIdC: String!
  # $eventIdD, $eventIdE, etc.
) {
  Poaps(input: {filter: {eventId: {_eq: $eventIdA}}, blockchain: ALL, limit: 200}) {
    Poap {
      owner {
        poaps(input: {blockchain: ALL, filter: {eventId: {_eq: $eventIdB}}}) {
          owner {
            poaps(input: {blockchain: ALL, filter: {eventId: {_eq: $eventIdC}}}) {
              owner {
                ... # more nested poaps and so on
              }
            }
          }
        }
      }
    }
  }
}
```

## Example #1: Get Common Holders/Attendees of EthGlobal Lisbon 2023 Partner Attendee POAP & EthCC\[6] Attendee POAP

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetCommonHoldersOfEthGlobalLisbonAndEthCC($eventIdA: String!, $eventIdB: String!) {
  Poaps(input: {filter: {eventId: {_eq: $eventIdA}}, blockchain: ALL, limit: 200}) {
    Poap {
      owner {
        poaps(input: {blockchain: ALL, filter: {eventId: {_eq: $eventIdB}}}) {
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
  "eventIdA": "127462",
  "eventIdB": "141910"
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
            "poaps": [
              {
                "owner": {
                  "addresses": [
                    "0xa960bcc07fdd90c94cbb40de105ad909f7012a9c"
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Have EthGlobal Lisbon, but no EthCC[6]
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

If you have any questions or need help regarding fetching holders or attendees of multiple POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
