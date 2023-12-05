---
description: >-
  Learn how to fetch ERC20, NFTs, or POAPs in common from multiple Farcaster
  users.
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

# 🤝 Tokens In Common

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and for integrating onchain and offchain data with Farcaster.

# Table Of Contents

In this guide you will learn how to use Airstack to:

- [ERC20 Tokens In Common Owned By Farcaster User(s)](tokens-in-common.md#erc20-tokens-in-common-owned-by-farcaster-user-s)
- [NFTs In Common Owned By Farcaster User(s)](tokens-in-common.md#nfts-in-common-owned-by-farcaster-user-s)
- [POAPs Tokens In Common Owned By Farcaster User(s)](tokens-in-common.md#poaps-in-common-owned-by-farcaster-user-s)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
- Basic knowledge of GraphQL

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

## ERC20 Tokens In Common Owned By Farcaster User(s)

You can fetch common ERC20 tokens of multiple Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/QIFCtkidcx" %}
Show common ERC20 tokens of two Farcaster users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _eq: "fc_fname:betashop.eth" }
        tokenType: { _eq: ERC20 }
      }
      blockchain: polygon
    }
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {
            filter: {
              owner: { _eq: "fc_fname:sarvesh" }
              tokenType: { _eq: ERC20 }
            }
          }
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
            "tokenBalances": [] // Owned by betashop.eth, but not owned by sarvesh
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## NFTs In Common Owned By Farcaster User(s)

You can fetch common NFTs of multiple Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/chCICTPZo6" %}
Show common NFTs of two Farcaster users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _eq: "fc_fname:betashop.eth" }
        tokenType: { _in: [ERC721, ERC1155] }
      }
      blockchain: polygon
    }
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {
            filter: {
              owner: { _eq: "fc_fname:sarvesh" }
              tokenType: { _in: [ERC721, ERC1155] }
            }
          }
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
                "id": "05c4cece4238243f6f8fd6c22e0df78fa07450283322c476dc4af2e2a744c6d3",
                "token": {
                  "address": "0x4c23d718416164b2f52f88dde4cf0b1172b741b6",
                  "symbol": "swap-rewards.xyz",
                  "name": "swap-rewards.xyz"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by betashop.eth, but not owned by sarvesh
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## POAPs In Common Owned By Farcaster User(s)

You can fetch common POAPs of multiple Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/7RtCNBtLUA" %}
Show common POAPs of two Farcaster users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: {
      filter: { owner: { _eq: "fc_fname:betashop.eth" } }
      blockchain: ALL
      order: { createdAtBlockNumber: DESC }
    }
  ) {
    Poap {
      poapEvent {
        poaps(input: { filter: { owner: { _eq: "fc_fname:ipeciura" } } }) {
          poapEvent {
            eventName
            eventId
            endDate
            country
            city
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
                  "eventName": "Airstack Amazing Race Dubai",
                  "eventId": "145995",
                  "endDate": "2023-07-30T00:00:00Z",
                  "country": "UAE",
                  "city": "Dubai"
                }
              }
            ]
          }
        },
        {
          "poapEvent": {
            "poaps": [] // owned by betsahop.eth, but not ipeciura
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

If you have any questions or need help regarding fetching common ERC20 tokens, NFTs, or POAPs of multiple Farcaster users, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Tokens In Common Guides](../tokens-in-common/)
- [Nested Queries](../../api-references/nested-queries.md)
- [POAPs API Reference](../../api-references/api-reference/poaps-api/)
- [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
