---
description: >-
  Learn how to fetch the list of common token holders of multiple ERC20 tokens
  or NFTs (ERC721s & ERC1155s).
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# 🪙 Multiple ERC20s or NFTs

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching dapps and integrating on-chain and off-chain data from various blockchains.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Common Holders of 2 ERC20 Tokens](multiple-erc20s-or-nfts.md#common-holders-of-2-erc20-tokens)
* [Common Holders of 2 NFTs](multiple-erc20s-or-nfts.md#common-holders-of-2-nfts)
* [Common Holders of NFT That Held A Minimum Amount of ERC20 Token](multiple-erc20s-or-nfts.md#common-holders-of-nft-that-held-a-minimum-amount-of-erc20-token)
* [Common Holders of A Token on Ethereum and A Token on Polygon (Cross-Chain)](multiple-erc20s-or-nfts.md#common-holders-of-a-token-on-ethereum-and-a-token-on-polygon-cross-chain)
* [Common Holders of A Token on Ethereum and A Token on Base (Cross-Chain)](multiple-erc20s-or-nfts.md#common-holders-of-a-token-on-ethereum-and-a-token-on-base-cross-chain)
* [Common Holders of A Token on Polygon and A Token on Base (Cross-Chain)](multiple-erc20s-or-nfts.md#common-holders-of-a-token-on-polygon-and-a-token-on-base-cross-chain)
* [Common Holders of More Than 2 ERC20 Tokens or NFTs](multiple-erc20s-or-nfts.md#common-holders-of-more-than-2-erc20-tokens-or-nfts)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
**React**

```sh
npm install @airstack/airstack-react
```

**Node**

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
**React**

```sh
yarn add @airstack/airstack-react
```

**Node**

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
**React**

```sh
pnpm install @airstack/airstack-react
```

**Node**

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack
```
{% endtab %}
{% endtabs %}

Then, add the following snippets to your code:

{% tabs %}
{% tab title="React" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }
};
```
{% endtab %}

{% tab title="Node" %}
```javascript
import { init, fetchQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const { data, error } = fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = "YOUR_QUERY" # Replace with GraphQL Query

async def main():
    execute_query_client = api_client.create_execute_query_object(
        query=query)

    query_response = await execute_query_client.execute_query()
    print(query_response.data)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

#### Other Programming Languages

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Best Practices

With [nested queries](../../api-references/nested-queries.md), Airstack finds the intersection of common token holders of multiple ERC20 tokens or NFTs in the following order:

1. Filtering token holders on the 1st outermost query
2. Comparing token holders of 1st outermost query against the token holders on 2nd outermost query
3. Comparing token holders of the 1st and 2nd outermost query against the token holders on the 3rd outermost query
4. Comparing token holders of 1st, 2nd, ..., nth outermost query against the token holders on (n + 1)-th outermost query

Since the number of objects returned in the responses will be dependent on the number of token holders on the 1st outermost query, it's most efficient that you have the **token with the least amount of holders** on the 1st outermost query.

{% hint style="info" %}
Suppose there are two tokens:

* Token A: 100,000 holders
* Token B: 1,000 holders.

If Token A is the input on the 1st outermost query, then the end result will be **100,000 objects** in the response array.

On the other hand, if Token B is the input on the 1st outermost query, then the end result will be instead **ONLY** 1,000 objects in the response array.

The latter approach will be more efficient and easier for further formatting.
{% endhint %}

## Common Holders of 2 ERC20 Tokens

### Fetching

You can fetch the common holders of two given ERC20, e.g. [USDT](https://explorer.airstack.xyz/token-holders?address=0xdac17f958d2ee523a2206206994597c13d831ec7\&blockchain=ethereum\&rawInput=%23%E2%8E%B1Tether+USD%E2%8E%B1%280xdac17f958d2ee523a2206206994597c13d831ec7+TOKEN+ethereum+null%29+\&inputType=ADDRESS) and [USDC](https://explorer.airstack.xyz/token-holders?address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48\&blockchain=ethereum\&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29+\&inputType=ADDRESS):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/oQ9QlZmTCm" %}
Show the common holders of both USDT and USDC
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfUSDTAndUSDC {
<strong>  TokenBalances(input: {filter: {tokenAddress: {_eq: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"}}, blockchain: ethereum, limit: 200}) {
</strong>    TokenBalance {
      owner {
<strong>        tokenBalances(input: {filter: {tokenAddress: {_eq: "0xdAC17F958D2ee523a2206206994597C13D831ec7"}}, limit: 200}) {
</strong>          owner {
            addresses
          }
        }
      }
    }
  }
}
</code></pre>
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
                  "addresses": ["0x28c6c06298d514db089934071355e5743bf21d60"]
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

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(
    ({ owner }) => owner?.tokenBalances?.[0]?.owner?.addresses
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner'] and len(item['owner']['tokenBalances']) > 0 and 'owner' in item['owner']['tokenBalances'][0] and 'addresses' in item['owner']['tokenBalances'][0]['owner']:
                result.append(item['owner']['tokenBalances']
                              [0]['owner']['addresses'])
    result = [item for sublist in result for item in sublist]
    result = list(set(result))
    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0xc77d249809ae5a118eef66227d1a01a3d62c82d4",
  "0x3291e96b3bff7ed56e3ca8364273c5b4654b2b37",
  "0xe348c7959e47646031cea7ed30266a6702d011cc",
  // ...other token holders
  "0xa69babef1ca67a37ffaf7a485dfff3382056e78c",
  "0x46340b20830761efd32832a74d7169b29feb9758",
  "0x2008b6c3d07b061a84f790c035c2f6dc11a0be70"
]
```

## Common Holders of 2 NFTs

### Fetching

You can fetch the common holders of two given NFTs, e.g. [BAYC](https://explorer.airstack.xyz/token-holders?address=0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d\&rawInput=0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d\&inputType=ADDRESS\&tokenType=ERC721) and [Moonbirds](https://explorer.airstack.xyz/token-holders?address=0x23581767a106ae21c074b2276D25e5C3e136a68b\&blockchain=ethereum\&rawInput=%23%E2%8E%B1Moonbirds%E2%8E%B1%280x23581767a106ae21c074b2276D25e5C3e136a68b+NFT\_COLLECTION+ethereum+null%29+\&inputType=ADDRESS):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/VxXVkCBwDY" %}
Show common holders of both BAYC and Moonbirds
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfBAYCAndMoonBirds {
<strong>  TokenBalances(input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}) {
</strong>    TokenBalance {
      owner {
<strong>        tokenBalances(input: {filter: {tokenAddress: {_eq: "0x23581767a106ae21c074b2276D25e5C3e136a68b"}}, limit: 200}) {
</strong>          owner {
            addresses
          }
          tokenId
        }
      }
    }
  }
}
</code></pre>
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
                  "addresses": ["0x020ca66c30bec2c4fe3861a94e4db4a498a35872"]
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

All the common holders' addresses will be returned inside the innermost `owner.addresses` field and `tokenId` is optional for determining which NFT is specifically held by the user as there can be multiple NFTs held by a single user.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(
    ({ owner }) => owner?.tokenBalances?.[0]?.owner?.addresses
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner'] and len(item['owner']['tokenBalances']) > 0 and 'owner' in item['owner']['tokenBalances'][0] and 'addresses' in item['owner']['tokenBalances'][0]['owner']:
                result.append(item['owner']['tokenBalances']
                              [0]['owner']['addresses'])
    result = [item for sublist in result for item in sublist]
    result = list(set(result))
    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0xc77d249809ae5a118eef66227d1a01a3d62c82d4",
  "0x3291e96b3bff7ed56e3ca8364273c5b4654b2b37",
  "0xe348c7959e47646031cea7ed30266a6702d011cc",
  // ...other token holders
  "0xa69babef1ca67a37ffaf7a485dfff3382056e78c",
  "0x46340b20830761efd32832a74d7169b29feb9758",
  "0x2008b6c3d07b061a84f790c035c2f6dc11a0be70"
]
```

## Common Holders of NFT That Held A Minimum Amount of ERC20 Token

### Fetching

You can fetch common holders of an NFT with a specific amount held for the ERC20, e.g. [Moonbirds](https://explorer.airstack.xyz/token-holders?address=0x23581767a106ae21c074b2276D25e5C3e136a68b\&blockchain=ethereum\&rawInput=%23%E2%8E%B1Moonbirds%E2%8E%B1%280x23581767a106ae21c074b2276D25e5C3e136a68b+NFT\_COLLECTION+ethereum+null%29+\&inputType=ADDRESS) holders with more than 10 [USDT](https://explorer.airstack.xyz/token-holders?address=0xdac17f958d2ee523a2206206994597c13d831ec7\&blockchain=ethereum\&rawInput=%23%E2%8E%B1Tether+USD%E2%8E%B1%280xdac17f958d2ee523a2206206994597c13d831ec7+TOKEN+ethereum+null%29+\&inputType=ADDRESS):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/PB2Q1GsUjT" %}
Show holders of Moonbirds NFT that also hold more than 10 USDT
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfMoonbirdsAndMoreThanTenUSDT {
<strong>  TokenBalances(input: {filter: {tokenAddress: {_eq: "0x23581767a106ae21c074b2276D25e5C3e136a68b"}}, blockchain: ethereum, limit: 200}) {
</strong>    TokenBalance {
      owner {
        tokenBalances(
          input: {
            filter: {
<strong>              tokenAddress: {_eq: "0xdAC17F958D2ee523a2206206994597C13D831ec7"},
</strong><strong>              formattedAmount: {_gt: 10}
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
                  "addresses": ["0x88156facf7c584bf658badba8bf4d17af6fb150e"]
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

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(
    ({ owner }) => owner?.tokenBalances?.[0]?.owner?.addresses
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner'] and item['owner']['tokenBalances'] is not None and 'owner' in item['owner']['tokenBalances'][0] and 'addresses' in item['owner']['tokenBalances'][0]['owner']:
                result.append(item['owner']['tokenBalances']
                              [0]['owner']['addresses'])
    result = [item for sublist in result for item in sublist]
    result = list(set(result))
    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0xc77d249809ae5a118eef66227d1a01a3d62c82d4",
  "0x3291e96b3bff7ed56e3ca8364273c5b4654b2b37",
  "0xe348c7959e47646031cea7ed30266a6702d011cc",
  // ...other token holders
  "0xa69babef1ca67a37ffaf7a485dfff3382056e78c",
  "0x46340b20830761efd32832a74d7169b29feb9758",
  "0x2008b6c3d07b061a84f790c035c2f6dc11a0be70"
]
```

## Common Holders of A Token on Ethereum and A Token on Polygon (Cross-Chain)

### Fetching

You can fetch common holders of NFT from different chains, e.g. [BAYC](https://explorer.airstack.xyz/token-holders?address=0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d\&rawInput=0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d\&inputType=ADDRESS\&tokenType=ERC721) on Ethereum and [Cuddleverse](https://explorer.airstack.xyz/token-holders?address=0xb59bd2c3f24afa4a3177d0e886abe072ef9c8eb0\&rawInput=0xb59bd2c3f24afa4a3177d0e886abe072ef9c8eb0\&inputType=ADDRESS\&tokenType=ERC721) on Polygon:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/RrmS3nK5y8" %}
Show holders of BAYC on Ethereum that also holds CuddleVerse NFT on Polygon
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfBAYCAndCuddleVerse {
<strong>  TokenBalances(input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}) {
</strong>    TokenBalance {
      owner {
<strong>        tokenBalances(input: {filter: {tokenAddress: {_eq: "0xb59bd2c3f24afa4a3177d0e886abe072ef9c8eb0"}}, blockchain: polygon, limit: 200}) {
</strong>          owner {
            addresses
          }
          tokenId
        }
      }
    }
  }
}
</code></pre>
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
                  "addresses": ["0x020ca66c30bec2c4fe3861a94e4db4a498a35872"]
                },
                "tokenId": "352"
              }
            ]
          }
        },
        {
          "owner": {
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

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(
    ({ owner }) => owner?.tokenBalances?.[0]?.owner?.addresses
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner'] and len(item['owner']['tokenBalances']) > 0 and 'owner' in item['owner']['tokenBalances'][0] and 'addresses' in item['owner']['tokenBalances'][0]['owner']:
                result.append(item['owner']['tokenBalances']
                              [0]['owner']['addresses'])
    result = [item for sublist in result for item in sublist]
    result = list(set(result))
    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0xc77d249809ae5a118eef66227d1a01a3d62c82d4",
  "0x3291e96b3bff7ed56e3ca8364273c5b4654b2b37",
  "0xe348c7959e47646031cea7ed30266a6702d011cc",
  // ...other token holders
  "0xa69babef1ca67a37ffaf7a485dfff3382056e78c",
  "0x46340b20830761efd32832a74d7169b29feb9758",
  "0x2008b6c3d07b061a84f790c035c2f6dc11a0be70"
]
```

## Common Holders of A Token on Ethereum and A Token on Base (Cross-Chain)

### Fetching

You can fetch common holders of NFT from different chains, e.g. [BAYC](https://explorer.airstack.xyz/token-holders?address=0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d\&rawInput=0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d\&inputType=ADDRESS\&tokenType=ERC721) on Ethereum and [BasePaint](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xba5e05cb26b78eda3a2f8e3b3814726305dcac83\&tokenType=\&rawInput=%23%E2%8E%B1BasePaint%E2%8E%B1%280xba5e05cb26b78eda3a2f8e3b3814726305dcac83+NFT\_COLLECTION+base+null%29\&inputType=NFT\_COLLECTION\&activeTokenInfo=\&activeSnapshotInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&spamFilter=\&activeSocialInfo=) on Base:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/esKjB5ODR7" %}
Show holders of BAYC on Ethereum that also holds BasePaint NFT on Base
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfBAYCAndCuddleVerse {
<strong>  TokenBalances(input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}) {
</strong>    TokenBalance {
      owner {
<strong>        tokenBalances(input: {filter: {tokenAddress: {_eq: "0xba5e05cb26b78eda3a2f8e3b3814726305dcac83"}}, blockchain: base, limit: 200}) {
</strong>          owner {
            addresses
          }
          tokenId
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "tokenBalances": [
              {
                "owner": {
                // Address that holds both
<strong>                  "addresses": ["0x81b55fbe66c5ffbb8468328e924af96a84438f14"]
</strong>                },
                "tokenId": "114"
              }
            ]
          }
        },
        {
          "owner": {
<strong>            "tokenBalances": [] // Hold BAYC, but no BasePaint
</strong>          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(
    ({ owner }) => owner?.tokenBalances?.[0]?.owner?.addresses
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner'] and len(item['owner']['tokenBalances']) > 0 and 'owner' in item['owner']['tokenBalances'][0] and 'addresses' in item['owner']['tokenBalances'][0]['owner']:
                result.append(item['owner']['tokenBalances']
                              [0]['owner']['addresses'])
    result = [item for sublist in result for item in sublist]
    result = list(set(result))
    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0x81b55fbe66c5ffbb8468328e924af96a84438f14",
  "0xb45cde30c957939fe6c06a6e14570f01e8b9b8cf",
  "0x001fd093d89b24f7de35bc47b6d01c750f398707",
  "0x0c1e99991dd2f7d374bff13f9f52284ce6cfdae5",
  "0xe53ebe8ded621a3e5a1789bbd2605378f8591c87",
  // ...other token holders
]
```

## Common Holders of A Token on Polygon and A Token on Base (Cross-Chain)

### Fetching

You can fetch common holders of NFT from different chains, e.g. [Poly Birds](https://explorer.airstack.xyz/token-holders?activeView=\&address=0x1532521561F588fc10c3Aa32e688e48544CF8e33\&tokenType=\&rawInput=%23%E2%8E%B1Poly+Birds%E2%8E%B1%280x1532521561F588fc10c3Aa32e688e48544CF8e33+NFT\_COLLECTION+polygon+null%29\&inputType=NFT\_COLLECTION\&activeTokenInfo=\&activeSnapshotInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&spamFilter=\&activeSocialInfo=) on Polygon and [BasePaint](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xba5e05cb26b78eda3a2f8e3b3814726305dcac83\&tokenType=\&rawInput=%23%E2%8E%B1BasePaint%E2%8E%B1%280xba5e05cb26b78eda3a2f8e3b3814726305dcac83+NFT\_COLLECTION+base+null%29\&inputType=NFT\_COLLECTION\&activeTokenInfo=\&activeSnapshotInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&spamFilter=\&activeSocialInfo=) on Base:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/y5UhNihsrW" %}
Show holders of Poly Birds on Polygon that also holds BasePaint NFT on Base
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfBAYCAndCuddleVerse {
<strong>  TokenBalances(input: {filter: {tokenAddress: {_eq: "0x1532521561F588fc10c3Aa32e688e48544CF8e33"}}, blockchain: polygon, limit: 200}) {
</strong>    TokenBalance {
      owner {
<strong>        tokenBalances(input: {filter: {tokenAddress: {_eq: "0xba5e05cb26b78eda3a2f8e3b3814726305dcac83"}}, blockchain: base, limit: 200}) {
</strong>          owner {
            addresses
          }
          tokenId
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "tokenBalances": [
              {
                "owner": {
                // Address that holds both
<strong>                  "addresses": ["0x945f6c41516224ffef1f5c24f108b6ddd7e0c828"]
</strong>                },
                "tokenId": "114"
              }
            ]
          }
        },
        {
          "owner": {
<strong>            "tokenBalances": [] // Hold BAYC, but no BasePaint
</strong>          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(
    ({ owner }) => owner?.tokenBalances?.[0]?.owner?.addresses
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner'] and len(item['owner']['tokenBalances']) > 0 and 'owner' in item['owner']['tokenBalances'][0] and 'addresses' in item['owner']['tokenBalances'][0]['owner']:
                result.append(item['owner']['tokenBalances']
                              [0]['owner']['addresses'])
    result = [item for sublist in result for item in sublist]
    result = list(set(result))
    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0x945f6c41516224ffef1f5c24f108b6ddd7e0c828",
  "0x305938c5c6abb6440f1402cc7488b56fed153c0e",
  "0x305938c5c6abb6440f1402cc7488b56fed153c0e",
  "0x42e289eeb4ced357b9de1d86d02c2f4ace6b951c",
  "0xc6cea3ab255d285650dde82530dbca75e0385dd9",
  // ...other token holders
]
```

## Common Holders of More Than 2 ERC20 Tokens or NFTs

### Fetching

Fetching common holders for more than 2 ERC20 tokens or NFTs works the same way as fetching only 2 ERC20 tokens or NFTs with the addition of more token address parameters and nesting:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/YZ9LJDDCvQ" %}
Show common holders of more than 2 ERC20 tokens or NFTs
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfMoreThanTwoTokensOrNfts {
  TokenBalances(
<strong>    input: {filter: {tokenAddress: {_eq: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"}}, blockchain: ethereum, limit: 200}
</strong>  ) {
    TokenBalance {
      owner {
        tokenBalances(
<strong>          input: {filter: {tokenAddress: {_eq: "0xdAC17F958D2ee523a2206206994597C13D831ec7"}}, limit: 200}
</strong>        ) {
          owner {
            tokenBalances(
<strong>              input: {filter: {tokenAddress: {_eq: "0x2791bca1f2de4661ed88a30c99a7a9449aa84174"}}, limit: 200, blockchain: polygon}
</strong>            ) {
              owner {
                addresses
              }
            }
          }
        }
      }
    }
  }
}
</code></pre>
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
                  "tokenBalances": [
                    {
                      "owner": {
                        "addresses": [
                          "0x205e94337bc61657b4b698046c3c2c5c1d2fb8f1"
                        ]
                      }
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
                  "tokenBalances": [] // Doesn't have all 3 tokens
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

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(
    ({ owner }) =>
      owner?.tokenBalances?.[0]?.owner?.tokenBalances?.[0]?.owner?.addresses
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner'] and len(item['owner']['tokenBalances']) > 0 and 'owner' in item['owner']['tokenBalances'][0] and 'tokenBalances' in item['owner']['tokenBalances'][0]['owner'] and len(item['owner']['tokenBalances'][0]['owner']['tokenBalances']) > 0 and 'owner' in item['owner']['tokenBalances'][0]['owner']['tokenBalances'][0] and 'addresses' in item['owner']['tokenBalances'][0]['owner']['tokenBalances'][0]['owner']:
                result.append(item['owner']['tokenBalances'][0]['owner']
                              ['tokenBalances'][0]['owner']['addresses'])

    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0xc77d249809ae5a118eef66227d1a01a3d62c82d4",
  "0x3291e96b3bff7ed56e3ca8364273c5b4654b2b37",
  "0xe348c7959e47646031cea7ed30266a6702d011cc",
  // ...other token holders
  "0xa69babef1ca67a37ffaf7a485dfff3382056e78c",
  "0x46340b20830761efd32832a74d7169b29feb9758",
  "0x2008b6c3d07b061a84f790c035c2f6dc11a0be70"
]
```

## Developer Support

If you have any questions or need help regarding fetching token holders of multiple ERC20s or NFTs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
* [Token Holders Tutorial for Lens Devs](../lens/token-holders.md)
* [Token Holders Tutorial for Farcaster Devs](../farcaster/token-holders.md)
