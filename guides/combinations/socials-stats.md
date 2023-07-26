---
description: >-
  Learn how to fetch the list of common token holders that has XMTP, Lens, or
  Farcasters.
---

# Socials Stats

## Pre-requisites

* [ ] Completed [ERC20s, NFTs, and POAPs](erc20s-nfts-and-poaps.md)

## XMTP

To check if XMTP is enabled, simply add `xmtp.isXMTPEnabled` under the `owner` field:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfTwoTokensOrNfts($tokenA: Address!, $tokenB: Address!, $cursor: String) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200, cursor: $cursor}
  ) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenB}}, limit: 200}) {
          owner {
            addresses
            xmtp {
<strong>              isXMTPEnabled
</strong>            }
          }
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Variable" %}
```json
{
  "tokenA": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
  "tokenB": "0x23581767a106ae21c074b2276D25e5C3e136a68b"
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
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0x9680f3957510cf85751a096c2194520c36a4a003"
                  ],
                  "xmtp": [
                    {
                      "isXMTPEnabled": true // XMTP enabled
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0x28c6c06298d514db089934071355e5743bf21d60"
                  ],
                  "xmtp": [] // XMTP not enabled yet
                }
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

## Lens

To show Lens profile in the responses, add `socials` with `lens` added to the `dappName` filter under the `owner` field:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfTwoTokensOrNfts($tokenA: Address!, $tokenB: Address!, $cursor: String) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200, cursor: $cursor}
  ) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenB}}, limit: 200}) {
          owner {
            addresses
<strong>            socials(input: {filter: {dappName: {_eq: lens}}}) {
</strong><strong>              profileName
</strong><strong>              dappName
</strong>            }
          }
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Variable" %}
```json
{
  "tokenA": "0xb93ee8cdab36199c6debf5bbec53e5908fd8e4e1",
  "tokenB": "0x0a1bbd57033f57e7b6743621b79fcb9eb2ce3676"
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
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                  ],
                  "socials": [
                    {
                      "profileName": "vitalik.lens",
                      "dappName": "lens"
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0xeaf55242a90bb3289dB8184772b0B98562053559"
                  ],
                  "socials": null // no Lens profile
                }
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

## Farcaster

To show Farcaster in the responses, add `socials` with `farcaster` added to the `dappName` filter under the `owner` field:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfTwoTokensOrNfts($tokenA: Address!, $tokenB: Address!, $cursor: String) {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200, cursor: $cursor}
  ) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenB}}, limit: 200}) {
          owner {
            addresses
<strong>            socials(input: {filter: {dappName: {_eq: farcaster}}}) {
</strong><strong>              profileName
</strong><strong>              dappName
</strong>            }
          }
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Variable" %}
```json
{
  "tokenA": "0xb93ee8cdab36199c6debf5bbec53e5908fd8e4e1",
  "tokenB": "0x0a1bbd57033f57e7b6743621b79fcb9eb2ce3676"
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
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                  ],
                  "socials": [
                    {
                      "profileName": "vbuterin",
                      "dappName": "farcaster"
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0xeaf55242a90bb3289dB8184772b0B98562053559"
                  ],
                  "socials": null // no Farcaster
                }
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

## Developer Support

If you have any questions or need help regarding fetching holders or attendees of multiple POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
* [Resolve Identities](../resolve-identities/)
  * [ENS](../resolve-identities/ens.md)
  * [Lens](../resolve-identities/lens.md)
  * [Farcaster](../resolve-identities/farcaster.md)
* [Use Cases](broken-reference)
  * [Universal Resolver](../../use-cases/xmtp/universal-resolver.md)
  * [Lens Resolver](../../use-cases/lens/universal-resolver.md)
  * [Farcaster Resolver](../../use-cases/farcaster/universal-resolver.md)
