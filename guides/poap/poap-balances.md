---
description: >-
  Learn how to fetch POAPs held by various web3 identities, such as 0x
  addresses, ENS domains, Lens profiles, and Farcasters. You will also learn how
  to fetch commonly held POAPs between multiple users.
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

# ⚖ POAP Balances

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [POAP](https://poap.xyz/) applications and for integrating POAP on-chain data indexed directly from both Ethereum and Gnosis.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get POAPs Held By 0x Address(es)](poap-balances.md#get-poaps-held-by-0x-address-es)
- [Get POAPs Held By ENS Domain(s)](poap-balances.md#get-poaps-held-by-ens-domain-s)
- [Get POAPs Held By Lens Profile(s)](poap-balances.md#get-poaps-held-by-lens-profile-s)
- [Get POAPs Held By Farcaster User(s)](poap-balances.md#get-poaps-held-by-farcaster-user-s)
- [Get POAPs In Common Held By Multiple Users](poap-balances.md#get-poaps-in-common-held-by-multiple-users)

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

## Get POAPs Held By 0x Address(es)

You can fetch all the POAPs held by [0x address(es)](#user-content-fn-1)[^1] by providing it as an input to the `owner` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/CUZMZEMjDY" %}
Show me all poaps hold by 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: {
      filter: { owner: { _in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"] } }
      blockchain: ALL
    }
  ) {
    Poap {
      eventId
      tokenId
      poapEvent {
        eventName
        description
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
          "eventId": "80393",
          "tokenId": "5860961",
          "poapEvent": {
            "eventName": "You met Cryptocomical at Lisbon 2022",
            "description": "You met Cryptocomical at Lisbon 2022\r\n\r\nConheceu a Cryptocomical em Lisboa 2022"
          }
        },
        {
          "eventId": "79011",
          "tokenId": "5819739",
          "poapEvent": {
            "eventName": "ITU Blockchain - Devcon Satellite",
            "description": "This POAP is a memory of İTÜ Blockchain’s Devcon Satellite event."
          }
        },
        {
          "eventId": "15678",
          "tokenId": "3055722",
          "poapEvent": {
            "eventName": "Assets9 community members",
            "description": "Poap distributed for Assets9 community members"
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get POAPs Held By ENS Domain(s)

You can fetch all the POAPs held by ENS Domain(s) by providing it as an input to the `owner` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/zJGqXpGgC8" %}
Show me all POAPs hold by vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: { filter: { owner: { _in: ["vitalik.eth"] } }, blockchain: ALL }
  ) {
    Poap {
      eventId
      tokenId
      poapEvent {
        eventName
        description
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
          "eventId": "80393",
          "tokenId": "5860961",
          "poapEvent": {
            "eventName": "You met Cryptocomical at Lisbon 2022",
            "description": "You met Cryptocomical at Lisbon 2022\r\n\r\nConheceu a Cryptocomical em Lisboa 2022"
          }
        },
        {
          "eventId": "79011",
          "tokenId": "5819739",
          "poapEvent": {
            "eventName": "ITU Blockchain - Devcon Satellite",
            "description": "This POAP is a memory of İTÜ Blockchain’s Devcon Satellite event."
          }
        },
        {
          "eventId": "15678",
          "tokenId": "3055722",
          "poapEvent": {
            "eventName": "Assets9 community members",
            "description": "Poap distributed for Assets9 community members"
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get POAPs Held By Lens Profile(s)

You can fetch all the POAPs held by Lens profile(s) by providing either the Lens profile name or ID as an input to the `owner` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/p6X3LrqngO" %}
Show me all POAPs hold by lens/@vitalik
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: { filter: { owner: { _in: ["lens/@vitalik"] } }, blockchain: ALL }
  ) {
    Poap {
      eventId
      tokenId
      poapEvent {
        eventName
        description
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
          "eventId": "80393",
          "tokenId": "5860961",
          "poapEvent": {
            "eventName": "You met Cryptocomical at Lisbon 2022",
            "description": "You met Cryptocomical at Lisbon 2022\r\n\r\nConheceu a Cryptocomical em Lisboa 2022"
          }
        },
        {
          "eventId": "79011",
          "tokenId": "5819739",
          "poapEvent": {
            "eventName": "ITU Blockchain - Devcon Satellite",
            "description": "This POAP is a memory of İTÜ Blockchain’s Devcon Satellite event."
          }
        },
        {
          "eventId": "15678",
          "tokenId": "3055722",
          "poapEvent": {
            "eventName": "Assets9 community members",
            "description": "Poap distributed for Assets9 community members"
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get POAPs Held By Farcaster User(s)

You can fetch all the POAPs held by Farcaster(s) by providing the fname[^2] of fid as an input to the `owner` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/gsL8hEScZN" %}
Show me all poaps hold by Farcaster user fname vitalik.eth and fid 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: {
      filter: { owner: { _in: ["fc_fname:vitalik.eth", "fc_fid:602"] } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Poap {
      eventId
      tokenId
      poapEvent {
        eventName
        description
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
          "eventId": "80393",
          "tokenId": "5860961",
          "poapEvent": {
            "eventName": "You met Cryptocomical at Lisbon 2022",
            "description": "You met Cryptocomical at Lisbon 2022\r\n\r\nConheceu a Cryptocomical em Lisboa 2022"
          }
        },
        {
          "eventId": "79011",
          "tokenId": "5819739",
          "poapEvent": {
            "eventName": "ITU Blockchain - Devcon Satellite",
            "description": "This POAP is a memory of İTÜ Blockchain’s Devcon Satellite event."
          }
        },
        {
          "eventId": "15678",
          "tokenId": "3055722",
          "poapEvent": {
            "eventName": "Assets9 community members",
            "description": "Poap distributed for Assets9 community members"
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get POAPs In Common Held By Multiple Users

You can fetch all the POAPs commonly held by multiple users, in the following example take 2 users [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) and [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS):

### Try Demo

{% embed url="https://app.airstack.xyz/query/Yg6z4Mb6Q8" %}
Show me common POAPs hold by both betashop.eth and ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query CommonPOAPs {
  Poaps(
    input: {
      filter: { owner: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Poap {
      poapEvent {
        poaps(input: { filter: { owner: { _eq: "ipeciura.eth" } } }) {
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
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "poapEvent": {
            "poaps": [
              {
<strong>                "poapEvent": { // POAP owned by both betashop.eth and ipeciura.eth
</strong>                  "eventName": "You've met Lucas in Summer 23",
                  "eventId": "141662",
                  "endDate": "2023-09-21T00:00:00Z",
                  "country": "Worldwide",
                  "city": "Summer 23"
                }
              }
            ]
          }
        },
        {
          "poapEvent": {
<strong>            "poaps": [] // POAP hold by betashop.eth, but not hold by ipeciura.eth
</strong>          }
        }
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching POAP balances, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Poaps API Reference](../../api-references/api-reference/poaps-api/)
- [Poaps API Examples](../../api-references/api-reference/poaps-api/poaps-api-examples.md)
- [Combinations](../combinations/)
  - [Multiple POAPs](../combinations/multiple-poaps.md)
  - [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
- [Tokens In Common](../tokens-in-common/)
  - [POAPs](../tokens-in-common/poaps.md)

1. e.g. `vitalik.eth`
2. e.g. `fc_fid:602`

[^1]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^2]: e.g. `fc_fname:vitalik.eth`
