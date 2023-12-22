---
description: Learn how to use Airstack to get all holders of NFT or POAP that have XMTP.
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

# 🗃 NFT & POAP Holders

## 🗃 NFT & POAP Holders

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [XMTP](https://xmtp.org) applications and integrating on-chain and off-chain data with [XMTP](https://xmtp.org).

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to check if holders of a given NFT or POAP have XMTP enabled:

- [NFT Holders](nft-and-poap-holders.md#nft-holders)
- [POAP Holders](nft-and-poap-holders.md#poap-holders)

### Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
- Basic knowledge of GraphQL
- Basic knowledge of [XMTP](https://xmtp.org)

### Get Started

**JavaScript/TypeScript/Python**

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

**Other Programming Languages**

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

### NFT Holders

Get the NFT holders that have XMTP using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api.md) API and provide an NFT contract address for the `$tokenAddress` input:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/203pV6Pyns" %}
Get the NFT holders that have XMTP (Demo)
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: { _eq: "0xc0f95066899efd7c0540b9474f81355a83e6f578" }
      }
      blockchain: ethereum
    }
  ) {
    TokenBalance {
      owner {
        xmtp {
          isXMTPEnabled
        }
        addresses
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
          "owner": {
            "xmtp": [
              {
                "isXMTPEnabled": true // XMTP is enabled
              }
            ],
            "addresses": ["0xa64af7f78de39a238ecd4fff7d6d410dbace2df0"]
          }
        },
        {
          "owner": {
            "xmtp": [], // XMTP is not enabled
            "addresses": ["0x7a3c17937749780432db64f6569b6671c0b45e1b"]
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

### POAP Holders

Get the POAP holders that have XMTP using the [Poaps](../../api-references/api-reference/poaps-api.md) API and provide an POAP event ID for the <mark style="color:red;">`$eventId`</mark> input:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/EtrCwWln2c" %}
Get the POAP holders that have XMTP (Demo)
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query POAPEventHoldersWithXMTP {
  Poaps(input: { filter: { eventId: { _eq: "141910" } }, blockchain: ALL }) {
    Poap {
      owner {
        addresses
        xmtp {
          isXMTPEnabled
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
          "owner": {
            "addresses": ["0xda85048c977134b09fc05cd3d1abd3a63e8edf4d"],
            "xmtp": [] // XMTP is not enabled
          }
        },
        {
          "owner": {
            "addresses": ["0x546457bbddf5e09929399768ab5a9d588cb0334d"],
            "xmtp": [
              {
                "isXMTPEnabled": true // XMTP is enabled
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

### Developer Support

If you have any questions or need help regarding checking XMTP for holders of a given NFT or POAP, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

- [XMTPs API Reference](../../api-references/api-reference/xmtps-api.md)
- [Has XMTP For Lens Developers](../lens/has-xmtp.md)
- [Has XMTP For Farcaster Developers](../farcaster/has-xmtp.md)
