---
description: >-
  Learn how to use Airstack to recommend trending ERC-20 tokens to swap for your
  users based on various conditions.
---

# 🔄 Trending Swaps

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to recommend all trending tokens to swap within a given time frame. _This API makes use of Airstack Abstractions to calculate the results in milliseconds; no compute or backend database is required on your end._&#x20;

Trending Swaps are currently available for the Base and Ethereum blockchains.

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

## Fetch Trending Swaps

First, define the following parameters to fetch the trending swaps data:

<table><thead><tr><th width="157">Name</th><th width="268">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>blockchain</code></td><td><code>ethereum</code> | <code>base</code></td><td>Blockchain to fetch the trending swaps, either Ethereum or Base.</td></tr><tr><td><code>timeFrame</code></td><td><code>one_hour</code> | <code>two_hours</code> | <code>eight_hours</code> | <code>one_day</code> | <code>two_days</code> | <code>seven_days</code></td><td>Only fetch trending swaps within the chosen time frame, e.g. <code>one_hour</code> will fetch trending mints for the last 1 hour.</td></tr><tr><td><code>criteria</code></td><td><code>buy_transaction_count</code> | <code>sell_transaction_count</code> | <code>total_transaction_count</code> | <code>buy_volume</code> | <code>sell_volume</code> | <code>total_volume</code> | <code>unique_buy_wallets</code>  | <code>unique_sell_wallets</code> | <code>total_unique_wallets</code></td><td><p>This will calculate and sort the tokens based on the chosen criteria:<br>- <code>buy_transaction_count</code>: Calculate the number of buy transactions</p><p>- <code>sell_transaction_count</code>: Calculate the number of sell transactions</p><p>- <code>total_transaction_count</code>: Calculate the number of buy &#x26; sell transactions<br>- <code>buy_volume</code>: Calculate the number of buying volumes.</p><p>- <code>sell_volume</code>: Calculate the number of selling volumes.</p><p>- <code>total_volume</code>: Calculate the number of buying &#x26; selling volumes.<br>- <code>unique_buy_wallets</code>: Calculate the number of unique buyer wallets<br>- <code>unique_sell_wallets</code>: Calculate the number of unique seller wallets<br>- <code>total_unique_wallets</code>: Calculate the number of unique buyer &#x26; seller wallets</p></td></tr></tbody></table>

Once you have the parameters prepared, simply use the query below and add the parameters as variables:&#x20;

### Try Demo

{% embed url="https://app.airstack.xyz/query/ZwD1VpZoqJ" %}
Show all trending swaps evaluated by the total buying and selling volume in the last 1 hour
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query TrendingSwaps(
  $blockchain: TrendingSwapsBlockchain!
  $criteria: TrendingSwapsCriteria!
  $timeFrame: TimeFrame!
) {
  TrendingSwaps(
    input: {
      timeFrame: $timeFrame
      criteria: $criteria
      blockchain: $blockchain
    }
  ) {
    TrendingSwap {
      address
      blockchain
      buyTransactionCount
      buyVolume
      sellTransactionCount
      sellVolume
      timeFrom
      timeTo
      totalTransactionCount
      totalUniqueWallets
      totalVolume
      uniqueBuyWallets
      uniqueSellWallets
      token {
        name
        symbol
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "blockchain": "base",
  "criteria": "total_volume",
  "timeFrame": "one_hour"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TrendingSwaps": {
      "TrendingSwap": [
        {
          "address": "0x80d50d69bd091745d1940716fe43a86f90384672",
          "blockchain": "base",
          "buyTransactionCount": 0,
          "buyVolume": 0,
          "sellTransactionCount": 1,
          "sellVolume": 999964319270804.5,
          "timeFrom": "2024-04-04T20:03:00Z",
          "timeTo": "2024-04-04T20:04:00Z",
          "totalTransactionCount": 1,
          "totalUniqueWallets": 1,
          "totalVolume": 999964319270804.5,
          "uniqueBuyWallets": 1,
          "uniqueSellWallets": 1,
          "token": {
            "name": "ROOST",
            "symbol": "$ROOST"
          }
        },
        // Other trending swaps
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

🎉 :partying\_face: Congratulations you've just fetched all the trending tokens to swap and integrate it into your application!

## **D**eveloper Support

If you have any questions or need help regarding integrating or building trending swaps into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TrendingSwaps API Reference](../api-references/api-reference/trendingtokens-api.md)
