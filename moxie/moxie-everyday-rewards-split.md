---
description: >-
  Learn how you can fetch a certain FID's Moxie everyday rewards split between
  the user, their fans, the channel fans, and the Farcaster network.
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

# ✂️ Moxie Everyday Rewards Split

By default [Everyday Rewards](https://build.moxie.xyz/the-moxie-protocol/everyday-rewards) earned from Casts and Frames by a member are split as follows:

* 50% to the member who earned the reward
* 20% to their Fans (holders of their Fan Tokens)
* 20% to the Fans of the Channel casted into (holders of their Fan Tokens)
* 10% to Fans of Farcaster overall (holders of the Fan Tokens)

This split can be modified by the address that is the subject of the Member Fan Token.

To learn more about the different split options and how it can impact [Everyday Rewards](https://build.moxie.xyz/the-moxie-protocol/everyday-rewards) distribution on the Moxie protocol, click [here](https://developer.moxie.xyz/learn/technical-deep-dive/rewards-split).

## Table Of Contents

* [Get Everyday Rewards Split Of A User](moxie-everyday-rewards-split.md#get-everyday-rewards-split-of-a-user)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) API key. To access this, you'll need to hold **at least 1 /airstack Channel Fan Token**.
* Basic knowledge of GraphQL

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

## Get Everyday Rewards Split Of A User

You can get the the current percentage of how much the user, their fans, the channel fans (if any), and the Farcaster network FT holders by using the `FarcasterFanTokenAuctions` API and providing the user's FID in the `entityId` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/wQBQOpDcjJ" %}
Show me the everyday rewards split of FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterFanTokenAuctions(
    input: {
      filter: {
<strong>        entityId: {_eq: "602"}, # Specify FID here
</strong>        entityType: {_eq: USER}
      },
      blockchain: ALL
    }
  ) {
    FarcasterFanTokenAuction {
      rewardDistributionPercentage {
        channelFans
        creator
        creatorFans
        network
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterFanTokenAuctions": {
      "FarcasterFanTokenAuction": [
        {
          "rewardDistributionPercentage": {
            "channelFans": 20, // 20% of the rewards goes to the Fans of the Channel casted into
            "creator": 0, // 0% of the rewards goes to the member who earned the reward
            "creatorFans": 70, // 70% of the rewards goes to their Fans (holders of their Fan Tokens)
            "network": 10 // 10% of the rewards goes to Fans of Farcaster overall (holders of the Fan Tokens)
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

If you have any questions or need help regarding fetching Everyday rewards split data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Everyday Rewards](https://build.moxie.xyz/the-moxie-protocol/everyday-rewards)
