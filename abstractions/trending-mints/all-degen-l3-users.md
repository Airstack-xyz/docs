---
description: Learn how to recommend mints that are trending among all Degen L3 users.
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

# 🎩 All Degen L3 Users

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to recommend all trending tokens that are minted by all Degen L3 users within a given time frame.

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

## Fetch Trending Mints Globally

First, define the following parameters to fetch the trending mints data:

<table><thead><tr><th width="149">Name</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>timeFrame</code></td><td><code>one_hour</code> | <code>two_hours</code> | <code>eight_hours</code> | <code>one_day</code> | <code>two_days</code> | <code>seven_days</code></td><td>Only fetch trending mints within the chosen time frame, e.g. <code>one_hour</code> will fetch trending mints for the last 1 hour.</td></tr><tr><td><code>criteria</code></td><td><code>unique_wallets</code> | <code>total_mints</code></td><td>This will calculate and sort the tokens based on the chosen criteria:<br>- <code>unique_wallets</code>: Calculate the number of unique wallets minting the trending tokens<br>- <code>total_mints</code>: Calculate the number of times minting occurred on the trending token</td></tr></tbody></table>

Once you have the parameters prepared, simply use the query below and add the parameters as variables:&#x20;

### Try Demo

{% embed url="https://app.airstack.xyz/query/B12QmUCsAb" %}
Show me all trending mints globally on Degen L3 in the last hour and sort by the number of unique wallets
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
      audience: all,
      blockchain: degen,
      criteria: $criteria
    }
  ) {
    TrendingMint {
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
      "TrendingMint": [
        {
          "address": "0x637cb7e5cecde9a69972bd9f39b10a152ed5ed7a",
          "erc1155TokenID": "",
          "criteriaCount": 189,
          "timeFrom": "2024-05-10T12:36:00Z",
          "timeTo": "2024-05-10T13:33:00Z",
          "token": {
            "name": "Pepe Mfers",
            "symbol": "FARQUEST",
            "type": "ERC721"
          }
        },
        // Other trending mints on Degen L3
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

🎉 :partying\_face: Congratulations you've just fetched all the trending tokens minted among all Degen L3 users and integrate it into your application!

## **D**eveloper Support

If you have any questions or need help regarding integrating or building trending mints into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Trending Mints With Farcaster Users](farcaster-users.md)
* [Trending Mints With Onchain Graph](onchain-graph.md)
* [Trending Mints With Common Minters](common-minters.md)
* [Token Mints Guides](../../guides/token-mints.md)
* [TrendingMints API Reference](../../api-references/api-reference/trendingmints-api.md)
