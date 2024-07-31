---
description: >-
  Learn how to fetch Farcaster Moxie earnings from different Farcaster users,
  channels, and the Farcaster network. In addition, learn how to sort by the
  earnings to get the top earners of Moxie.
---

# 💜 Farcaster Moxie Earnings

## Table Of Contents

* [Get Moxie Earning For Certain User](farcaster-moxie-earnings.md#get-moxie-earning-for-certain-user)
* [Get Moxie Earning For Certain Channel](farcaster-moxie-earnings.md#get-moxie-earning-for-certain-channel)
* [Get Moxie Earning For Farcaster Network](farcaster-moxie-earnings.md#get-moxie-earning-for-farcaster-network)
* [Top Moxie Earning Entities Based On Highest Moxie Earnings](farcaster-moxie-earnings.md#top-moxie-earning-entities-based-on-highest-moxie-earnings)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL
* Read [Moxie Whitepaper](https://build.moxie.xyz)

## Get Started

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

## Get Moxie Earning For Certain User

You can fetch the Moxie earnings for a certain user by specifying `entityType` as `USER` and add the FID of the user in `entityId`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/HtVpLr6G7Z" %}
Show me the lifetime Moxie earning of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieEarningStats(
    input: {
      timeframe: LIFETIME,
      blockchain: ALL,
      filter: {
        # specify entity as USER
<strong>        entityType: {_eq: USER},
</strong>        # specify the FID of the user
<strong>        entityId: {_eq: "3"}
</strong>      }
    }
  ) {
    FarcasterMoxieEarningStat {
      allEarningsAmount
      allEarningsAmountInWei
      castEarningsAmount
      castEarningsAmountInWei
      entityId
      entityType
      frameDevEarningsAmount
      frameDevEarningsAmountInWei
      otherEarningsAmount
      otherEarningsAmountInWei
      timeframe
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterMoxieEarningStats": {
      "FarcasterMoxieEarningStat": [
        {
          "allEarningsAmount": 6434663.061487857,
          "allEarningsAmountInWei": "6434663061487857000000000",
          "castEarningsAmount": 6434663.061487857,
          "castEarningsAmountInWei": "6434663061487857000000000",
          "entityId": "3",
          "entityType": "USER",
          "frameDevEarningsAmount": 0,
          "frameDevEarningsAmountInWei": "0",
          "otherEarningsAmount": 0,
          "otherEarningsAmountInWei": "0",
          "timeframe": "LIFETIME"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Moxie Earning For Certain Channel

You can fetch the Moxie earnings for a certain channel by specifying `entityType` as `CHANNEL` and add the channel ID in `entityId`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/vST0A5ZjtQ" %}
show me the Moxie earnings of /airstack channel
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieEarningStats(
    input: {
      timeframe: LIFETIME,
      blockchain: ALL,
      filter: {
        # Specify entity as CHANNEL
<strong>        entityType: {_eq: CHANNEL},
</strong>        # Specify the channel ID of the Farcaster channel
<strong>        entityId: {_eq: "airstack"}
</strong>      }
    }
  ) {
    FarcasterMoxieEarningStat {
      allEarningsAmount
      allEarningsAmountInWei
      castEarningsAmount
      castEarningsAmountInWei
      entityId
      entityType
      frameDevEarningsAmount
      frameDevEarningsAmountInWei
      otherEarningsAmount
      otherEarningsAmountInWei
      timeframe
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterMoxieEarningStats": {
      "FarcasterMoxieEarningStat": [
        {
          "allEarningsAmount": 102340.89672629537,
          "allEarningsAmountInWei": "102340896726295370000000",
          "castEarningsAmount": 84181.52016860196,
          "castEarningsAmountInWei": "84181520168601950000000",
          "entityId": "airstack",
          "entityType": "CHANNEL",
          "frameDevEarningsAmount": 16410,
          "frameDevEarningsAmountInWei": "16410000000000000000000",
          "otherEarningsAmount": 1749.3765576934104,
          "otherEarningsAmountInWei": "1749376557693410500000",
          "timeframe": "LIFETIME"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Moxie Earning For Farcaster Network

You can fetch the Moxie earnings for the Farcaster network by specifying `entityType` as `NETWORK` and add the channel ID in `entityId`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/P2D5isRmd8" %}
show me the lifetime Moxie earning for Farcaster network
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieEarningStats(
    input: {
      timeframe: LIFETIME,
      blockchain: ALL,
      filter: {
        # Specify entity as NETWORK
<strong>        entityType: {_eq: NETWORK}
</strong>      }
    }
  ) {
    FarcasterMoxieEarningStat {
      allEarningsAmount
      allEarningsAmountInWei
      castEarningsAmount
      castEarningsAmountInWei
      entityId
      entityType
      frameDevEarningsAmount
      frameDevEarningsAmountInWei
      otherEarningsAmount
      otherEarningsAmountInWei
      timeframe
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterMoxieEarningStats": {
      "FarcasterMoxieEarningStat": [
        {
          "allEarningsAmount": 872358.5098993169,
          "allEarningsAmountInWei": "872358509899317000000000",
          "castEarningsAmount": 860028.509899317,
          "castEarningsAmountInWei": "860028509899317000000000",
          "entityId": "FARCASTER",
          "entityType": "NETWORK",
          "frameDevEarningsAmount": 12330,
          "frameDevEarningsAmountInWei": "12330000000000000000000",
          "otherEarningsAmount": 0,
          "otherEarningsAmountInWei": "0",
          "timeframe": "LIFETIME"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Top Moxie Earning Entities Based On Highest Moxie Earnings

You can fetch the top entities w/ highest Moxie earnings by specifying  `order.allEarnings` to `DESC`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/l8lG4nw5T8" %}
Show me top lifetime earners of Moxie of user type
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieEarningStats(
    input: {
      timeframe: LIFETIME,
      blockchain: ALL,
      filter: {
<strong>        entityType: {_eq: USER} # alternatively can be CHANNEL
</strong>      },
      # Order by All earning for sorting by total earnings in 
      # descending order
<strong>      order: {allEarnings: DESC}
</strong>    }
  ) {
    FarcasterMoxieEarningStat {
      allEarningsAmount
      allEarningsAmountInWei
      castEarningsAmount
      castEarningsAmountInWei
      entityId
      entityType
      frameDevEarningsAmount
      frameDevEarningsAmountInWei
      otherEarningsAmount
      otherEarningsAmountInWei
      timeframe
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterMoxieEarningStats": {
      "FarcasterMoxieEarningStat": [
        {
          "allEarningsAmount": 10136643.391825985,
          "allEarningsAmountInWei": "10136643391825986000000000",
          "castEarningsAmount": 51821.7356651616,
          "castEarningsAmountInWei": "51821735665161600000000",
          "entityId": "336022",
          "entityType": "USER",
          "frameDevEarningsAmount": 0,
          "frameDevEarningsAmountInWei": "0",
          "otherEarningsAmount": 10084821.656160824,
          "otherEarningsAmountInWei": "10084821656160823000000000",
          "timeframe": "LIFETIME"
        },
        // Other top Farcaster user with most Moxie earned
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Moxie earnings data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarcasterMoxieEarningStats API Reference](../api-references/api-reference/farcastermoxieearningstats.md)
* [Moxie developer docs](https://developer.moxie.xyz)
