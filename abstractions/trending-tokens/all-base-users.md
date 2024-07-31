---
description: Learn how to recommend ERC-20 tokens that are trending amongst all Base users.
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

# 🔵 All Base Users

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to recommend all trending tokens among all Base users within a given time frame. _This API makes use of Airstack Abstractions to calculate the results in milliseconds; no compute or backend database required on your end._&#x20;

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
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

## Fetch Trending Tokens On Base

First, define the following parameters to fetch the trending tokens data:

<table><thead><tr><th width="149">Name</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>transferType</code></td><td><code>all</code> | <code>self_initiated</code></td><td>This will either include all types of transactions (including airdrops &#x26; self-transfers) or only self-initiated transactions.</td></tr><tr><td><code>timeFrame</code></td><td><code>one_hour</code> | <code>two_hours</code> | <code>eight_hours</code> | <code>one_day</code> | <code>two_days</code> | <code>seven_days</code></td><td>Only fetch trending tokens within the chosen time frame, e.g. <code>one_hour</code> will fetch trending mints for the last 1 hour.</td></tr><tr><td><code>criteria</code></td><td><code>unique_wallets</code> | <code>total_transfers</code> | <code>unique_holders</code></td><td>This will calculate and sort the tokens based on the chosen criteria:<br>- <code>unique_wallets</code>: Calculate the number of unique wallets transfer the trending tokens<br>- <code>total_transfers</code>: Calculate the number of times transfers occurred on the trending token<br>- <code>unique_holders</code>: Calculate the number of unique wallets that hold the trending tokens</td></tr></tbody></table>

Once you have the parameters prepared, simply use the query below and add the parameters as variables:&#x20;

### Try Demo

{% embed url="https://app.airstack.xyz/query/hYewVUxEXR" %}
Show all trending tokens among all Base users by the number of unique wallets in the last 1 hour
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery(
  $transferType: TrendingTokensTransferType!,
  $timeFrame: TimeFrame!,
  $criteria: TrendingTokensCriteria!
) {
  TrendingTokens(
    input: {
      transferType: $transferType,
      timeFrame: $timeFrame,
      audience: all,
      blockchain: base,
      criteria: $criteria
    }
  ) {
    TrendingToken {
      address
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
  "transferType": "self_initiated",
  "timeFrame": "one_hour",
  "criteria": "unique_wallets"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TrendingTokens": {
      "TrendingToken": [
        {
          "address": "0x940181a94a35a4569e4529a3cdfb74e38fd98631",
          "criteriaCount": 90,
          "timeFrom": "2024-03-15T09:40:00Z",
          "timeTo": "2024-03-15T10:40:00Z",
          "token": {
            "name": "Aerodrome",
            "symbol": "AERO",
            "type": "ERC20"
          }
        },
        // Other trending tokens
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

🎉 :partying\_face: Congratulations you've just fetched all the trending tokens among all Base users and integrate it into your application!

## **D**eveloper Support

If you have any questions or need help regarding integrating or building trending tokens into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Trending Tokens By Farcaster Users](farcaster-users.md)
* [Trending Tokens By All Degen L3 Users](../trending-mints/all-degen-l3-users.md)
* [TrendingTokens API Reference](../../api-references/api-reference/trendingtokens-api-1.md)
