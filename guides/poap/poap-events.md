---
description: >-
  Learn how to use Airstack to fetch POAP events and its details by a specified
  time period or city where it takes place.
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

# 🥳 POAP Events

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [POAP](https://poap.xyz/) applications and for integrating POAP on-chain data indexed directly from both Ethereum and Gnosis.

### Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get POAP Events In A Specific Period Of Time](poap-events.md#get-poap-events-in-a-given-city)
* [Get POAP Events In A Given City](poap-events.md#get-poap-events-in-a-specific-period-of-time)

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

## Get POAP Events In A Specific Period Of Time

You can fetch all the POAP events started in a specific period of time, e.g. the last 30 days:

### Try Demo

{% embed url="https://app.airstack.xyz/query/r8Jj0MQAoI" %}
Show me all POAP events in the last 30 days
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  PoapEvents(
    input: {
      filter: { startDate: { _gte: "2023-08-20", _lt: "2023-09-19" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    PoapEvent {
      eventName
      eventId
      startDate
      endDate
      country
      city
      isVirtualEvent
      contentValue {
        image {
          extraSmall
          large
          medium
          original
          small
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
```json
{
  "data": {
    "PoapEvents": {
      "PoapEvent": [
        {
          "eventName": "El Karo Show S4 Oro",
          "eventId": "150074",
          "startDate": "2023-09-14T00:00:00Z",
          "endDate": "2023-09-15T00:00:00Z",
          "country": "",
          "city": "",
          "isVirtualEvent": true,
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/poap/aEZ0t8ZmrdAub1I+3JbUyQ==/extra_small.png",
              "large": "https://assets.airstack.xyz/image/poap/aEZ0t8ZmrdAub1I+3JbUyQ==/large.png",
              "medium": "https://assets.airstack.xyz/image/poap/aEZ0t8ZmrdAub1I+3JbUyQ==/medium.png",
              "original": "https://assets.airstack.xyz/image/poap/aEZ0t8ZmrdAub1I+3JbUyQ==/original_image.png",
              "small": "https://assets.airstack.xyz/image/poap/aEZ0t8ZmrdAub1I+3JbUyQ==/small.png"
            }
          }
        },
        {
          "eventName": "Optimism Español | #OptimismxLATAM visita Ethereum Bogota 🇨🇴",
          "eventId": "148676",
          "startDate": "2023-09-01T00:00:00Z",
          "endDate": "2023-09-02T00:00:00Z",
          "country": "",
          "city": "",
          "isVirtualEvent": true,
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/poap/ZzDUrH6Sv6jlw8YVPXlJEw==/extra_small.png",
              "large": "https://assets.airstack.xyz/image/poap/ZzDUrH6Sv6jlw8YVPXlJEw==/large.png",
              "medium": "https://assets.airstack.xyz/image/poap/ZzDUrH6Sv6jlw8YVPXlJEw==/medium.png",
              "original": "https://assets.airstack.xyz/image/poap/ZzDUrH6Sv6jlw8YVPXlJEw==/original_image.png",
              "small": "https://assets.airstack.xyz/image/poap/ZzDUrH6Sv6jlw8YVPXlJEw==/small.png"
            }
          }
        },
        {
          "eventName": "2023 WADE SIDE IRL SEOUL",
          "eventId": "146053",
          "startDate": "2023-09-03T00:00:00Z",
          "endDate": "2023-09-03T00:00:00Z",
          "country": "Korea",
          "city": "Seoul",
          "isVirtualEvent": false,
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/poap/Iz3VhkqwAP4ArQtAb02Dug==/extra_small.png",
              "large": "https://assets.airstack.xyz/image/poap/Iz3VhkqwAP4ArQtAb02Dug==/large.png",
              "medium": "https://assets.airstack.xyz/image/poap/Iz3VhkqwAP4ArQtAb02Dug==/medium.png",
              "original": "https://assets.airstack.xyz/image/poap/Iz3VhkqwAP4ArQtAb02Dug==/original_image.png",
              "small": "https://assets.airstack.xyz/image/poap/Iz3VhkqwAP4ArQtAb02Dug==/small.png"
            }
          }
        }
        // Other POAP events started in the last 30 days
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get POAP Events In A Given City

You can fetch all the POAP events in a given city, e.g. Dubai:

### Try Demo

{% embed url="https://app.airstack.xyz/query/quBsRP74S2" %}
Show me all POAPs that happened in Dubai
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  PoapEvents(
    input: { filter: { city: { _eq: "Dubai" } }, blockchain: ALL, limit: 200 }
  ) {
    PoapEvent {
      eventName
      eventId
      startDate
      endDate
      country
      city
      isVirtualEvent
      contentValue {
        image {
          extraSmall
          large
          medium
          original
          small
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
```json
{
  "data": {
    "PoapEvents": {
      "PoapEvent": [
        {
          "eventName": "Justin Peyton @ Dubai Web3 Marketing Association Meetup",
          "eventId": "84456",
          "startDate": "2022-11-21T00:00:00Z",
          "endDate": "2022-11-22T00:00:00Z",
          "country": "",
          "city": "Dubai",
          "isVirtualEvent": false,
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/poap/s81P1OdItILuD+djZr0m2g==/extra_small.png",
              "large": "https://assets.airstack.xyz/image/poap/s81P1OdItILuD+djZr0m2g==/large.png",
              "medium": "https://assets.airstack.xyz/image/poap/s81P1OdItILuD+djZr0m2g==/medium.png",
              "original": "https://assets.airstack.xyz/image/poap/s81P1OdItILuD+djZr0m2g==/original_image.png",
              "small": "https://assets.airstack.xyz/image/poap/s81P1OdItILuD+djZr0m2g==/small.png"
            }
          }
        },
        {
          "eventName": "GamesCoin Momentum Upload Party",
          "eventId": "39630",
          "startDate": "2022-04-21T00:00:00Z",
          "endDate": "2022-04-21T00:00:00Z",
          "country": "United Arab Emirates",
          "city": "Dubai",
          "isVirtualEvent": false,
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/poap/hd17JA0HdMyivLYRxd3EwQ==/extra_small.png",
              "large": "https://assets.airstack.xyz/image/poap/hd17JA0HdMyivLYRxd3EwQ==/large.png",
              "medium": "https://assets.airstack.xyz/image/poap/hd17JA0HdMyivLYRxd3EwQ==/medium.png",
              "original": "https://assets.airstack.xyz/image/poap/hd17JA0HdMyivLYRxd3EwQ==/original_image.png",
              "small": "https://assets.airstack.xyz/image/poap/hd17JA0HdMyivLYRxd3EwQ==/small.png"
            }
          }
        },
        {
          "eventName": "Chorus One Dubai Retreat Q1 2023",
          "eventId": "98514",
          "startDate": "2023-01-13T00:00:00Z",
          "endDate": "2023-01-20T00:00:00Z",
          "country": "UAE",
          "city": "Dubai",
          "isVirtualEvent": false,
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/poap/iZcdtaaTQF6x9j1ptlAhZQ==/extra_small.png",
              "large": "https://assets.airstack.xyz/image/poap/iZcdtaaTQF6x9j1ptlAhZQ==/large.png",
              "medium": "https://assets.airstack.xyz/image/poap/iZcdtaaTQF6x9j1ptlAhZQ==/medium.png",
              "original": "https://assets.airstack.xyz/image/poap/iZcdtaaTQF6x9j1ptlAhZQ==/original_image.png",
              "small": "https://assets.airstack.xyz/image/poap/iZcdtaaTQF6x9j1ptlAhZQ==/small.png"
            }
          }
        }
      ]
      // Other POAP events happening in Dubai
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching POAP balances, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Poaps API Reference](../../api-references/api-reference/poaps-api/)
* [Poaps API Examples](../../api-references/api-reference/poaps-api/poaps-api-examples.md)
* [Combinations](../combinations/)
  * [Multiple POAPs](../combinations/multiple-poaps.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
* [Tokens In Common](../tokens-in-common/)
  * [POAPs](../tokens-in-common/poaps.md)
