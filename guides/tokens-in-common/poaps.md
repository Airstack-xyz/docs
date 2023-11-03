---
description: Learn how to fetch POAPs in common from multiple users.
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

# POAPs

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching dapps and integrating on-chain and off-chain data from various blockchains.

In this tutorial, you will learn how to fetch POAPs in common from multiple users.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [POAPs In Common](poaps.md#poaps-in-common)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
- Basic knowledge of GraphQL

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

## POAPs In Common

You can fetch common POAPs of multiple user(s) provided the user's 0x address, ENS domains, Lens profile, or Farcaster name or ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/0U2I2pZzcp" %}
Show common POAPs of two users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: {
      filter: { owner: { _eq: "0xeaf55242a90bb3289db8184772b0b98562053559" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Poap {
      poapEvent {
        poaps(
          input: {
            filter: {
              owner: { _eq: "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB" }
            }
          }
        ) {
          poapEvent {
            eventName
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
                  "eventName": "You have met Patricio in April of 2023 (IRL)"
                }
              }
            ]
          }
        },
        {
          "poapEvent": {
            "poaps": [] // owned by 1st address, but not the 2nd address
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

If you have any questions or need help regarding fetching common POAPs of multiple users, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Nested Queries](../../api-references/nested-queries.md)
- [POAPs API Reference](../../api-references/api-reference/poaps-api/)
- [Tokens In Common For Lens Developers](../lens/tokens-in-common.md)
- [Tokens In Common For Farcaster Developers](../farcaster/tokens-in-common.md)
