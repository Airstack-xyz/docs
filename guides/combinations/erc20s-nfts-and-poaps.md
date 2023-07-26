---
description: >-
  Learn how to fetch the list of common token holders of multiple ERC20s, NFTs,
  and POAPs.
---

# ERC20s, NFTs, and POAPs

## Pre-requisites

* [ ] Completed [Multiple ERC20s or NFTs](multiple-erc20s-or-nfts.md)
* [ ] Completed [Multiple POAPs](multiple-poaps.md)

## Common Holders of A Token (ERC20 or NFT) and A POAP

You can fetch the common holder of a token and a POAP by providing the token contract address to `$tokenAddress` and the POAP event ID to `$eventId`:

```graphql
query GetCommonHoldersOfTokenAndPOAP($tokenAddress: Address!, $eventId: String!) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenAddress}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        poaps(input: {filter: {eventId: {_eq: $eventId}}}) {
          owner {
            addresses
          }
        }
      }
    }
  }
}
```

## Common Holders of Any Tokens (ERC20s or NFTs) and POAPs

In order to add more tokens or POAPs, then more nested queries will be necessary.

Within the nested queries, you can either add `tokenBalances` with a new token contract address under the `owner` for adding new tokens:

<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfAnyTokensAndPOAP(
  $tokenAddressA: Address!,
<strong>  $tokenAddressB: Address!,
</strong>  # tokenAddressC, tokenAddressD, etc.
  $eventId: String!
) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenAddressA}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        poaps(input: {filter: {eventId: {_eq: $eventId}}}) {
          owner {
<strong>            tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenAddressB}}}) {
</strong>              owner {
                ... # more nested tokenBalances and so on
              }
            }
          }
        }
      }
    }
  }
}
</code></pre>

or add `poaps` to add new POAP with new POAP event ID:

<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfTokenAndAnyPOAPs(
  $tokenAddress: Address!,
  $eventIdA: String!,
<strong>  $eventIdB: String!
</strong>  # eventIdC, eventIdD, etc.
) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenAddress}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        poaps(input: {filter: {eventId: {_eq: $eventIdA}}}) {
          owner {
<strong>            poaps(input: {filter: {eventId: {_eq: $eventIdB}}}) {
</strong>              owner {
                ... # more nested poaps and so on
              }
            }
          }
        }
      }
    }
  }
}
</code></pre>

## Example #1: Get Common Holders of Nouns NFT and EthCC\[6] Attendee POAP

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetCommonHoldersOfNounsAndEthCC($tokenAddress: Address!, $eventId: String!) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenAddress}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        poaps(input: {filter: {eventId: {_eq: $eventId}}}) {
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
```graphql
{
  "tokenAddress": "0x9c8ff314c9bc7f6e59a9d9225fb22946427edc03",
  "eventId": "141910"
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
            "poaps": [
              {
                "owner": {
                  "addresses": [
                    "0xf6b6f07862a02c85628b3a9688beae07fea9c863"
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Have Nouns, but no EthCC[6] POAP
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

If you have any questions or need help regarding fetching token holders of multiple ERC20s, NFTs, and POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
