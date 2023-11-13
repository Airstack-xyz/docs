---
description: >-
  Learn how to fetch the list of common token holders across ERC20, NFTs, and
  POAPS, that also have XMTP, Lens, or Farcaster.
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

# Socials Combinations

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching dapps and integrating on-chain and off-chain data from various blockchains.

In this tutorial, you will learn how to fetch the common holders of multiple ERC20s, NFTs, and POAPs.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Common Token Holders with XMTP](socials-stats.md#xmtp)
* [Common Token Holders with Lens](socials-stats.md#lens)
* [Common Token Holders with Farcaster](socials-stats.md#farcaster)

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

* [ ] Completed [ERC20s, NFTs, and POAPs](erc20s-nfts-and-poaps.md)

## XMTP

### Fetching

To check if XMTP is enabled, simply add `xmtp.isXMTPEnabled` under the `owner` field:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/Vm9z1aQjll" %}
Show common holders of 2 tokens have XMTP enabled
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersWithXMTP {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: "0x23581767a106ae21c074b2276D25e5C3e136a68b"}}, limit: 200}) {
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
                  "addresses": ["0x9680f3957510cf85751a096c2194520c36a4a003"],
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
                  "addresses": ["0x28c6c06298d514db089934071355e5743bf21d60"],
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

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.tokenBalances?.map(({ owner }) =>
      owner?.xmtp?.isXMTPEnabled ? owner?.addresses : null
    )
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
            if 'owner' in item and 'tokenBalances' in item['owner']:
                for token_balance in item['owner']['tokenBalances']:
                    if 'owner' in token_balance and 'xmtp' in token_balance['owner'] and token_balance['owner']['xmtp'].get('isXMTPEnabled', False) and 'addresses' in token_balance['owner']:
                        result.append(token_balance['owner']['addresses'])

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

## Lens

### Fetching

To show Lens profile in the responses, add `socials` with `lens` added to the `dappName` filter under the `owner` field:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/wGMBcUwkS3" %}
Show common holders of 2 tokens if they have any Lens profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersWithLens {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xb93ee8cdab36199c6debf5bbec53e5908fd8e4e1"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: "0x0a1bbd57033f57e7b6743621b79fcb9eb2ce3676"}}, limit: 200}) {
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
                  "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
                  "socials": [
                    {
                      "profileName": "lens/@vitalik",
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
                  "addresses": ["0xeaf55242a90bb3289dB8184772b0B98562053559"],
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

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.tokenBalances?.map(({ owner }) =>
      owner?.socials.length > 0 ? owner?.addresses : null
    )
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
            if 'owner' in item and 'tokenBalances' in item['owner']:
                for token_balance in item['owner']['tokenBalances']:
                    if 'owner' in token_balance and 'socials' in token_balance['owner'] and len(token_balance['owner']['socials']) > 0 and 'addresses' in token_balance['owner']:
                        result.append(token_balance['owner']['addresses'])

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

## Farcaster

### Fetching

To show Farcaster in the responses, add `socials` with `farcaster` added to the `dappName` filter under the `owner` field:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/aTQEJTTxIr" %}
Show common holders of 2 tokens if they have any Farcaster
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersWithFarcaster {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xb93ee8cdab36199c6debf5bbec53e5908fd8e4e1"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: "0x0a1bbd57033f57e7b6743621b79fcb9eb2ce3676"}}, limit: 200}) {
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
                  "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
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
                  "addresses": ["0xeaf55242a90bb3289dB8184772b0B98562053559"],
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

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.tokenBalances?.map(({ owner }) =>
      owner?.socials.length > 0 ? owner?.addresses : null
    )
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
            if 'owner' in item and 'tokenBalances' in item['owner']:
                for token_balance in item['owner']['tokenBalances']:
                    if 'owner' in token_balance and 'socials' in token_balance['owner'] and len(token_balance['owner']['socials']) > 0 and 'addresses' in token_balance['owner']:
                        result.append(token_balance['owner']['addresses'])

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

If you have any questions or need help regarding fetching holders or attendees of multiple POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
* [Resolve Identities](../resolve-identities/)
  * [ENS](../resolve-identities/ens.md)
  * [Lens](../resolve-identities/lens.md)
  * [Farcaster](../resolve-identities/farcaster.md)
* [Use Cases](broken-reference/)
  * [Universal Resolver](../../use-cases/xmtp/universal-resolver.md)
  * [Lens Resolver](../../use-cases/lens/universal-resolver.md)
  * [Farcaster Resolver](../../use-cases/farcaster/universal-resolver.md)
