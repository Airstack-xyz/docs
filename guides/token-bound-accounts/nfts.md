---
description: >-
  Learn how to use Airstack to get token-bound (ERC6551) accounts by NFTs that
  own the accounts and vice versa
---

# ♦ NFTs

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching ERC6551 dapps and integrating on-chain and off-chain data.

In this tutorial, you will learn how to fetch the NFT that owns a given ERC6551 account and vice versa, fetch all the ERC6551 accounts owned by an NFT.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get Token Bound Accounts (ERC6551) By NFT Collection Address(es)](nfts.md#get-token-bound-accounts-erc6551-by-nft-collection-address-es)
* [Get Token Bound Accounts By Specific NFT](nfts.md#get-token-bound-accounts-by-specific-nft)
* [Get Owner NFT of a Token Bound Account](nfts.md#get-owner-nft-of-a-token-bound-account)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
#### React

```sh
npm install @airstack/airstack-react
```

#### Node

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
#### React

```sh
yarn add @airstack/airstack-react
```

#### Node

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
#### React

```sh
pnpm install @airstack/airstack-react
```

#### Node

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack asyncio
```
{% endtab %}
{% endtabs %}

Then, add the following snippets to your code:

{% tabs %}
{% tab title="React" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);
  
  if (data) {
    return <p>Data: {JSON.stringify(data)}</p>;
  }

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
import { init, fetchQuery } from "@airstack/node";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const { data, error } = await fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = """YOUR_QUERY""" # Replace with GraphQL Query

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

## Get Token Bound Accounts (ERC6551) By NFT Collection Address(es)

You can get all the ERC6551 accounts owned by a given [NFT collection address(es)](#user-content-fn-1)[^1]:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/Rezf4zOTBz" %}
Get Token Bound Accounts (ERC6551) By NFT Collection Address(es)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {filter: {tokenAddress: {_in: ["0x26727ed4f5ba61d3772d1575bca011ae3aef5d36"]}}, blockchain: ethereum, limit: 200}
  ) {
    Account {
      address {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
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
    "Accounts": {
      "Account": [
        {
          "address": {
            "addresses": [
              "0x776c56774a83a80c6ab608385418f6ffb2e088fb"
            ],
            "domains": [
              {
                "isPrimary": true,
                "name": "skynft.eth"
              }
            ],
            "socials": [
              {
                "profileName": "0xjst.lens",
                "dappName": "lens"
              },
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

## Get Token Bound Accounts By Specific NFT

You can get all the token bound accounts given by a specific NFT with contract address `tokenAddress` and token ID `tokenId`:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/j4yjmfO6wq" %}
Get Token Bound Accounts By Specific NFT (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {filter: {tokenAddress: {_eq: "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36"}, tokenId: {_eq: "1"}}, blockchain: ethereum, limit: 200}
  ) {
    Account {
      address {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
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
    "Accounts": {
      "Account": [
        {
          "address": {
            "addresses": [
              "0x718a9d173e66c411f48e41d3da2fa6f0ce8f5d3c"
            ],
            "domains": [
              {
                "isPrimary": true,
                "name": "skynft.eth"
              }
            ],
            "socials": [
              {
                "profileName": "0xjst.lens",
                "dappName": "lens"
              },
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

## Get Owner NFT of a Token Bound Account

You can get the [owner NFT](#user-content-fn-2)[^2] of a specific token bound account `address`:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/5EE9p28NXP" %}
Get Owner NFT of a Token Bound Account (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {blockchain: ethereum, filter: {address: {_in: "0xa1afb6a11ef500229538bfb38d5a0b8c1b61b425"}}}
  ) {
    Account {
      nft {
        address
        metaData {
          name
          description
          image
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
    "Accounts": {
      "Account": [
        {
          "nft": {
            "address": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
            "metaData": {
              "name": "Sapienz #3504",
              "description": "SAPIENZ is a collection of 15,000 networked playable characters created in partnership between STAPLEVERSE and RHYMEZLIKEDIMEZ. Imagine a reality where the marriage of art and commerce is seamless, where creativity is the currency and collaboration the fuel that powers our collective evolution. This is not merely a vision of the future, but a tangible reality within your grasp with the SAPIENZ world. The goal is to build the next 100+ years of street culture, are you in?",
              "image": "https://sapienz.imgix.net/rendered/3504.png?s=5d9251a2fe167597a1f5fbdf12e84ab3"
            }
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}



[^1]: This is represented as `tokenAddress` parameter in the GraphQL query.

[^2]: Owner NFT refers to the NFT that owns a particular ERC6551 Token-bound account.
