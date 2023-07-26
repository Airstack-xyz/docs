---
description: >-
  Learn how to fetch the list of common token holders of multiple ERC20 tokens
  or NFTs (ERC721s & ERC1155s).
---

# Multiple ERC20s or NFTs

## Topics

* [Common Holders of 2 ERC20 Tokens or NFTs](multiple-erc20s-or-nfts.md#common-holders-of-2-erc20-tokens-or-nfts)
* [Common Holders of More Than 2 ERC20 Tokens or NFTs](multiple-erc20s-or-nfts.md#common-holders-of-more-than-2-erc20-tokens)
* [Holders of NFT A That Holds More Than X Amount Of Token B](multiple-erc20s-or-nfts.md#example-3-get-common-holders-of-moonbirds-that-has-more-than-10-usdt)

## Common Holders of 2 ERC20 Tokens or NFTs

You can fetch the common holders of two given ERC20 or NFT contract addresses:

<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfTwoTokensOrNfts($tokenA: Address!, $tokenB: Address!, $cursor: String) {
  TokenBalances(input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200, cursor: $cursor}) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenB}}, limit: 200}) {
          owner {
<strong>            addresses
</strong>          }
          tokenId # Only for ERC721/1155 NFTs
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
</code></pre>

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

## Common Holders of More Than 2 ERC20 Tokens

Fetching common holders for more than 2 ERC20 tokens works the same way as fetching only 2 ERC20 tokens with the addition of more token address parameters `$tokenC`, `$tokenD`, `$tokenE`, etc:

```graphql
query GetCommonHoldersOfMoreThanTwoTokensOrNfts(
  $tokenA: Address!,
  $tokenB: Address!,
  $tokenC: Address!,
  # $tokenD, $tokenE, etc.
  $cursor: String
) {
  TokenBalances(input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200, cursor: $cursor}) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenB}}, limit: 200}) {
          owner {
            tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenC}}, limit: 200}) {
              owner {
                ... # more nested tokenBalances and so on
              }
              tokenId # Only for ERC721/1155 NFTs
            }
          }
          tokenId # Only for ERC721/1155 NFTs
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```

## Example #1: Get Common Holders of USDT and USDC

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetCommonHoldersOfUSDTAndUSDC($tokenA: Address!, $tokenB: Address!) {
  TokenBalances(input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200}) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenB}}, limit: 200}) {
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
  "tokenA": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
  "tokenB": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
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
                    "0x28c6c06298d514db089934071355e5743bf21d60"
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "tokenBalances": [] // Have USDT, but no USDC
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Example #2: Get Common Holders of BAYC & Moonbirds

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetCommonHoldersOfBAYCAndMoonBirds($tokenA: Address!, $tokenB: Address!) {
  TokenBalances(input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200}) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: $tokenB}}, limit: 200}) {
          owner {
            addresses
          }
          tokenId
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
                    "0x020ca66c30bec2c4fe3861a94e4db4a498a35872"
                  ]
                },
                "tokenId": "411"
              }
            ]
          }
        },
        {
          "owner": {
            "tokenBalances": [] // Have BAYC, but no Moonbirds
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Example #3: Get Common Holders of Moonbirds That Has More Than 10 USDT

### Code

For filtering based on ERC20 token held, add `formattedAmount` to the input as shown below:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfMoonbirdsAndMoreThanTenUSDT(
  $tokenA: Address!,
  $tokenB: Address!,
<strong>  $formattedAmount: Float!
</strong>) {
  TokenBalances(input: {filter: {tokenAddress: {_eq: $tokenA}}, blockchain: ethereum, limit: 200}) {
    TokenBalance {
      owner {
        tokenBalances(
          input: {
            filter: {
              tokenAddress: {_eq: $tokenB},
<strong>              formattedAmount: {_gt: $formattedAmount}
</strong>            },
            limit: 200
          }
        ) {
          owner {
            addresses
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
  "tokenA": "0x23581767a106ae21c074b2276D25e5C3e136a68b",
  "tokenB": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
  "formattedAmount": 10
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
                    "0x88156facf7c584bf658badba8bf4d17af6fb150e"
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "tokenBalances": [] // Have Moonbirds, but less than 10 USDT held
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

If you have any questions or need help regarding fetching token holders of multiple ERC20s or NFTs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
