---
description: >-
  Learn how to fetch the list of common token holders across various ERC20s,
  NFTs, and POAPs.
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

# 💰 Combinations of ERC20s, NFTs, and POAPs

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching dapps and integrating on-chain and off-chain data from various blockchains.

In this tutorial, you will learn how to fetch the common holders of multiple ERC20s, NFTs, and POAPs.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Common Holders of A Token (ERC20 or NFT) and A POAP](erc20s-nfts-and-poaps.md#common-holders-of-a-token-erc20-or-nft-and-a-poap)
* [Common Holders of Any Tokens (ERC20s or NFTs) and POAPs](erc20s-nfts-and-poaps.md#common-holders-of-any-tokens-erc20s-or-nfts-and-poaps)

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

## Pre-requisites

* [ ] Completed [Multiple ERC20s or NFTs](multiple-erc20s-or-nfts.md)
* [ ] Completed [Multiple POAPs](multiple-poaps.md)

## Best Practices

With [nested queries](../../api-references/nested-queries.md), Airstack finds the intersection of common holders of various ERC20s, NFTs, and POAPs in the following order:

1. Filtering holders on the 1st outermost query
2. Comparing holders of 1st outermost query against the holders of 2nd outermost query
3. Comparing holders of the 1st and 2nd outermost query against the holders on the 3rd outermost query
4. Comparing holders of 1st, 2nd, ..., nth outermost query against the holders on (n + 1)-th outermost query

Since the number of objects returned in the responses will be dependent on the number of holders on the 1st outermost query, it's most efficient that you have the **ERC20, NFT, or POAP with the least amount of holders** on the 1st outermost query.

{% hint style="info" %}
Suppose there is a token (ERC20 or NFT) and a POAP:

* Token A: 100,000 holders
* POAP B: 1,000 holders.

If Token A is the input on the 1st outermost query, then the end result will be **100,000 objects** in the response array.

On the other hand, if POAP B is the input on the 1st outermost query, then the end result will be instead **ONLY** 1,000 objects in the response array.

The latter approach will be more efficient and easier for further formatting.
{% endhint %}

## Common Holders of A Token (ERC20 or NFT) and A POAP

### Fetching

You can fetch the common holder of a token and a POAP by providing the token contract address and the POAP event ID:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/xzsDL1RHeL" %}
Show common holders of Nouns NFT and EthCC\[6] POAPs
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfNounsAndEthCC {
  TokenBalances(
<strong>    input: {filter: {tokenAddress: {_eq: "0x9c8ff314c9bc7f6e59a9d9225fb22946427edc03"}}, blockchain: ethereum, limit: 200}
</strong>  ) {
    TokenBalance {
      owner {
<strong>        poaps(input: {filter: {eventId: {_eq: "141910"}}}) {
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
            "poaps": [
              {
                "owner": {
                  "addresses": ["0xf6b6f07862a02c85628b3a9688beae07fea9c863"]
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

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) => owner?.addresses)
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'poaps' in item['owner']:
                for poap in item['owner']['poaps']:
                    if 'owner' in poap and 'addresses' in poap['owner']:
                        result.append(poap['owner']['addresses'])

    result = [item for sublist in result for item in sublist]
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

## Common Holders of Any Tokens (ERC20s or NFTs) and POAPs

### Fetching

In order to add more tokens or POAPs, then more nested queries will be necessary.

Within the nested queries, you can either add `tokenBalances` with a new token contract address under the `owner` for adding new tokens:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/Hf5PZ5kd3i" %}
Show common holders of any tokens (ERC20s or NFTs) and POAPs
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfTokenAndAnyPOAPs {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x9c8ff314c9bc7f6e59a9d9225fb22946427edc03"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        poaps(input: {filter: {eventId: {_eq: "141910"}}}) {
          owner {
<strong>            poaps(input: {filter: {eventId: {_eq: "3687"}}}) {
</strong>              owner {
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
            "poaps": [
              {
                "owner": {
                  "addresses": ["0xf6b6f07862a02c85628b3a9688beae07fea9c863"]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Does not have all the tokens & POAPs
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

or add `poaps` to add new POAP with new POAP event ID:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfAnyTokensAndPOAP {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x9c8ff314c9bc7f6e59a9d9225fb22946427edc03"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        poaps(input: {filter: {eventId: {_eq: "141910"}}}) {
          owner {
<strong>            tokenBalances(input: {filter: {tokenAddress: {_eq: "0xbd139ec36b0288f510d1b53423b5a03a50be2afa"}}}) {
</strong>              owner {
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
            "poaps": [
              {
                "owner": {
                  "addresses": ["0xf6b6f07862a02c85628b3a9688beae07fea9c863"]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Does not have all the tokens & POAPs
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
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) =>
      owner?.tokenBalances?.map(({ owner }) => owner?.addresses)
    )
  )
    .filter(Boolean)
    .flat(3)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'poaps' in item['owner']:
                for poap in item['owner']['poaps']:
                    if 'owner' in poap and 'tokenBalances' in poap['owner']:
                        for token_balance in poap['owner']['tokenBalances']:
                            if 'owner' in token_balance and 'addresses' in token_balance['owner']:
                                result.append(token_balance['owner']['addresses'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
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

If you have any questions or need help regarding fetching token holders of various ERC20s, NFTs, and POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
* [Token Holders Tutorial for Lens Devs](../lens/token-holders.md)
* [Token Holders Tutorial for Farcaster Devs](../farcaster/token-holders.md)
