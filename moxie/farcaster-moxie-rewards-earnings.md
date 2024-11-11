---
description: >-
  Learn how to fetch Farcaster Moxie Rewards earned by Farcaster users,
  channels, and the Farcaster network. In addition, learn how to sort by the
  earnings to get the top earners of Moxie.
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

# 💜 Farcaster Moxie Rewards Earnings

The Moxie Protocol allows Farcaster Members to earn [Everyday Rewards](https://build.moxie.xyz/the-moxie-protocol/everyday-rewards) based on interactions with their casts. Developers also receive rewards for each unique FID that engages with their frames.

Rewards are distributed in MOXIE tokens based on the FarScore of the person engaging, with the following rates:

* 0.5x [FarScore](farscores-and-farboosts.md) per like
* 1x [FarScore](farscores-and-farboosts.md) per reply (up to 5 replies per cast)
* 2x [FarScore](farscores-and-farboosts.md) per recast or quote cast

As of October 21, 2024, every Farcaster user has a daily **smart allowance** of MOXIE, which is determined by their engagement from the previous week. These allowances reset every Monday at 11 AM ET and will be communicated to the Farcaster users each Monday at the same time.

Additionally, each week a new set of **Moxie Heroes** is introduced, chosen based on diversity and quality of engagement. Heroes can either keep their superpowers or transfer them to another user. This week, Moxie Heroes receive a variable boost to their FarScore multiplying the MOXIE they distribute through each interaction during the week. Additional superpowers will be revealed in the upcoming weeks.

Learn more about Moxie Everyday Rewards, Daily Smart Allowances, and Moxie Heroes in the official Moxie whitepaper [here](https://build.moxie.xyz/the-moxie-protocol/everyday-rewards).

## Table Of Contents

* [Get The Moxie Smart Allowance For Certain User](farcaster-moxie-rewards-earnings.md#get-the-moxie-smart-allowance-for-certain-user)
* [Get Moxie Earned For Certain User](farcaster-moxie-rewards-earnings.md#get-moxie-earning-for-certain-user)
* [Get Moxie Earned For Certain Channel](farcaster-moxie-rewards-earnings.md#get-moxie-earning-for-certain-channel)
* [Get Moxie Earned For Farcaster Network](farcaster-moxie-rewards-earnings.md#get-moxie-earning-for-farcaster-network)
* [Get The Split Details Of Moxie Earned By Certain User](farcaster-moxie-rewards-earnings.md#get-the-split-details-of-moxie-earned-by-certain-user)
* Leaderboard: [Top Moxie Entities Based On Highest Moxie Earnings](farcaster-moxie-rewards-earnings.md#top-moxie-earning-entities-based-on-highest-moxie-earnings)

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

## Get The Moxie Smart Allowance For Certain User

You can fetch the Moxie smart allowance for a certain user by using the `Socials` API and specifying the FID of the user in `userId`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/TOeucjk01A" %}
Show me the Moxie Smart Allowance of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {_eq: farcaster},
        userId: {_eq: "3"}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      userId
      profileName
      totalSpendAllowance {
        frameInteractions
        likes
        recasts
        replies
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
    "Socials": {
      "Social": [
        {
          "userId": "3",
          "profileName": "dwr.eth",
          "totalSpendAllowance": {
            "frameInteractions": 75,
            "likes": 750,
            "recasts": 200,
            "replies": 300
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Moxie Earning For Certain User

You can fetch the Moxie earnings for a certain user before split by specifying `entityType` as `USER` and add the FID of the user in `entityId`:

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

## Get The Split Details Of Moxie Earned By Certain User

You can use the [`FarcasterMoxieEarningStats`](../api-references/api-reference/farcastermoxieearningstats.md) to fetch the split details of the total Moxie earned by a certain user entity on Moxie protocol that shows how much is distributed to the caster, the caster's fans,  channel fans, and the Farcaster network token holders.

To get the split details, simply provide the FID of the user in `entityId`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/y6kILCUt41" %}
Show me how the total Moxie earned by FID 3 is split between the caster, his/her fans, channel fans, and the Farcaster network
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieEarningStats(
    input: {
      filter: {
        entityType: {_eq: USER},
<strong>        entityId: {_eq: "3"} # specify the user's FID here
</strong>      },
      timeframe: TODAY,
      blockchain: ALL
    }
  ) {
    FarcasterMoxieEarningStat {
      splitDetails {
        castEarningsAmount
        frameDevEarningsAmount
        otherEarningsAmount
        entityType
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterMoxieEarningStats": {
      "FarcasterMoxieEarningStat": [
        {
          "splitDetails": [
            {
              "castEarningsAmount": 2420.289836734585,
              "frameDevEarningsAmount": 0,
              "otherEarningsAmount": 0,
              // Earnings split for the caster's fans
<strong>              "entityType": "CREATOR_FANS"
</strong>            },
            {
              "castEarningsAmount": 1080.3288640185472,
              "frameDevEarningsAmount": 0,
              "otherEarningsAmount": 0,
              // Earnings split for channel fans
<strong>              "entityType": "CHANNEL_FANS"
</strong>            },
            {
              "castEarningsAmount": 7390.685564552501,
              "frameDevEarningsAmount": 0,
              "otherEarningsAmount": 0,
              // Earnings split for the caster him/herself
<strong>              "entityType": "CREATOR"
</strong>            },
            {
              "castEarningsAmount": 1210.1449183672926,
              "frameDevEarningsAmount": 0,
              "otherEarningsAmount": 0,
              // Earnings split for the Farcaster network token holder
<strong>              "entityType": "NETWORK"
</strong>            }
          ]
        }
      ]
    }
  }
}
</code></pre>
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
