---
description: Learn how to recommend mints that are trending among Farcaster users.
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

# 📈 Trending Mints

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to recommend all trending tokens that are minted by Farcaster users within a given time frame. To accomplish this, it will require 2 steps:

1. [Prepare All Required Variables](trending-mints.md#step-1-prepare-all-required-variables)
2. [Fetch Trending Mints Among Farcaster Users](trending-mints.md#step-2-fetch-trending-mints-among-farcaster-users)

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

## Step 1: Prepare All Required Variables

First, define the following parameters to fetch the trending mints data:

<table><thead><tr><th width="149">Name</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>timeFrame</code></td><td><code>one_hour</code> | <code>two_hours</code> | <code>eight_hours</code> | <code>one_day</code> | <code>two_days</code> | <code>seven_days</code></td><td>Only fetch trending mints within the chosen time frame, e.g. <code>one_hour</code> will fetch trending mints for the last 1 hour.</td></tr><tr><td><code>criteria</code></td><td><code>unique_wallets</code> | <code>total_mints</code></td><td>This will calculate and sort the tokens based on the chosen criteria:<br>- <code>unique_wallets</code>: Calculate the number of unique wallets minting the trending tokens<br>- <code>total_mints</code>: Calculate the number of times minting occurred on the trending token</td></tr></tbody></table>

## Step 2: Fetch Trending Mints Among Farcaster Users

Once you have the variables prepared, simply use the query below and add the variables defined in [Step 1](trending-mints.md#step-1-prepare-all-required-variables):&#x20;

### Try Demo

{% embed url="https://app.airstack.xyz/query/hwKj9H6kvi" %}
Show me all trending mints among Farcaster user in the last hour and sort by the number of unique wallets
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery(
  $timeFrame: TimeFrame!,
  $criteria: TrendingMintsCriteria!
) {
  TrendingMints(
    input: {
      timeFrame: $timeFrame,
      audience: farcaster,
      blockchain: base,
      criteria: $criteria
    }
  ) {
    TrendingMints {
      address
      erc1155TokenID
      criteriaCount
      timeFrom
      timeTo
      token {
        name
        symbol
        type
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "timeFrame": "one_hour",
  "criteria": "unique_wallets"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TrendingMints": {
      "TrendingMints": [
        {
          "address": "0x83b16258221bfa260ca4f1ef31567241242fec7a",
          "erc1155TokenID": "1",
          "criteriaCount": 51,
          "timeFrom": "2024-03-07T11:39:00Z",
          "timeTo": "2024-03-07T12:37:00Z",
          "token": {
            "name": "Rainbow Cats",
            "symbol": "",
            "type": "ERC1155"
          }
        },
        // Other trending mints
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

🎉 :partying\_face: Congratulations you've just integrated fetched all the trending tokens minted by Farcaster users and integrate it into your application!

## Developer Support

If you have any questions or need help regarding integrating or building trending mints by Farcaster users into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Airstack Onchain Kit for Farcaster Frames](airstack-onchain-kit-for-farcaster-frames.md)
* [Global Trending Mints](../trending-mints/global.md)
* [Trending Mints With Onchain Graph](../trending-mints/onchain-graph.md)
* [Trending Mints With Common Minters](../trending-mints/common-minters.md)
* [TrendingMints API Reference](../../api-references/api-reference/trendingmints-api.md)
