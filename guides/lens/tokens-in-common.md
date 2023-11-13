---
description: >-
  Learn how to fetch ERC20, NFTs, or POAPs in common from multiple Lens
  profiles.
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

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to fetch ERC20, NFTs, or POAPs in common from multiple Lens profiles.

In this guide you will learn how to use Airstack to:

- [ERC20 Tokens In Common Owned By Lens Profile(s)](tokens-in-common.md#erc20-tokens-in-common-owned-by-lens-profile-s)
- [NFTs In Common Owned By Lens Profile(s)](tokens-in-common.md#nfts-in-common-owned-by-lens-profile-s)
- [POAPs In Common Owned By Lens Profile(s)](tokens-in-common.md#poaps-in-common-owned-lens-profile-s)

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

## ERC20 Tokens In Common Owned By Lens Profile(s)

You can fetch common ERC20 tokens of multiple Lens profiles:

### Try Demo

{% embed url="https://app.airstack.xyz/query/gHp07gzekM" %}
Show common ERC20 tokens held by Lens profiles
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _eq: "lens/@bradorbradley" }
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
              owner: { _eq: "lens/@christina" }
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
                "id": "bd292d232f10973466326db331c17ca93bff3be12fec404dc6b611001caacac9",
                "token": {
                  "address": "0x086373fad3447f7f86252fb59d56107e9e0faafa",
                  "symbol": "YUP",
                  "name": "Yup"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by lens/@bradorbradley, but not owned by lens/@christina
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## NFTs In Common Owned By Lens Profile(s)

You can fetch common NFTs of multiple Lens profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/Uj52BnGzaR" %}
Show common NFTs of two Lens profiles
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _eq: "lens/@bradorbradley" }
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
              owner: { _eq: "lens/@christina" }
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
                "id": "67da11cfbad4a45244500bbe55925a31b3a0cdcf4b6444c3538a8fc7ba54f904",
                "token": {
                  "address": "0xaa5885fcf948365fc470d428e65c68fbc1c2e1a8",
                  "symbol": "Pi APE Case",
                  "name": "Pi APE Case"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by lens/@bradorbradley, but not owned by lens/@christina
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## POAPs In Common Owned Lens Profile(s)

You can fetch common POAPs of multiple Lens profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/mqcXKeXp19" %}
Show common POAPs of two Lens profiles
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: {
      filter: { owner: { _eq: "lens/@bradorbradley" } }
      blockchain: ALL
      order: { createdAtBlockNumber: DESC }
    }
  ) {
    Poap {
      poapEvent {
        poaps(input: { filter: { owner: { _eq: "lens/@christina" } } }) {
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
                  "eventName": "Avara Genesis Fam",
                  "eventId": "105708",
                  "endDate": "2023-02-24T00:00:00Z",
                  "country": "",
                  "city": ""
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by lens/@bradorbradley, but not owned by lens/@christina
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

If you have any questions or need help regarding fetching common ERC20 tokens, NFTs, or POAPs of multiple Lens profiles, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Tokens In Common Guides](../tokens-in-common/)
- [Nested Queries](../../api-references/nested-queries.md)
- [POAPs API Reference](../../api-references/api-reference/poaps-api/)
- [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
