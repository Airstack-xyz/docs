---
description: >-
  Learn how to fetch the FarScores, FarBoosts, FarRank of a Farcaster member. In
  addition, you can also fetch a user's TVL and how much it impacts a member's
  FarBoosts.
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

# 🚀 FarScores & FarBoosts

Every Farcaster Member earns a **FarScore** based on their relative influence in the Farcaster network and each FarScore user can be increased by earning **FarBoosts**, which consequently can also increase a Farcaster Member's **FarRank**.

**As of 3 October, 2024**, FarScores do not factor in follows from users who have not been active on Farcaster in the previous 60 days, on a rolling basis.

Every 1 point on **FarBoosts** is earned by contributing 100,000 Moxie into your total locked value (TVL) into the Moxie protocol or providing 100,000 Moxie into the Liquidity Pool (Uniswap v2 & Aerodrome).

Additionally, you can also earn 1.2 point on **FarBoosts** by for every 100,000 Moxie worth of Fan Token is locked into the Moxie Staking contract for at least 3 months.

To learn more about **FarScores & FarBoosts**, click [here](../social-capital-value-and-social-capital-scores.md).

## Table Of Contents

* [Get The FarScores Of A User](farscores-and-farboosts.md#get-the-farscores-of-a-user)
* [Get The FarScores Of A User Without Any FarBoosts](farscores-and-farboosts.md#get-the-farscores-of-a-user-without-any-farboosts)
* [Get The Percentage Of Increase Of User's FarScore From FarBoosts](farscores-and-farboosts.md#get-the-percentage-of-increase-of-users-farscore-from-farboosts)
* [Get The FarRank Of A User](farscores-and-farboosts.md#get-the-farrank-of-a-user)
* [Get The FarBoost Of A User](farscores-and-farboosts.md#get-the-farboost-of-a-user)
* [Get The FarBoost Of A User Gained From TVL](farscores-and-farboosts.md#get-the-farboost-of-a-user-gained-only-from-tvl) (TVL Boost)
* [Get The FarBoost Of A User Gained From Providing Liquidity to DEXs](farscores-and-farboosts.md#get-the-farboost-of-a-user-gained-from-providing-liquidity-to-dexs-also-referred-to-as-lp-boost) (Liquidity Boost)
* [Get The FarBoost Of A User Gained From Locking Fan Tokens](farscores-and-farboosts.md#get-the-farboost-of-a-user-gained-from-locking-fan-tokens-moxie-power-boost) (Power Boost)
* [Get The Moxie Hero Multiplier Of A User](farscores-and-farboosts.md#get-the-moxie-hero-multiplier-of-a-user)
* [Get The TVL Of A User](farscores-and-farboosts.md#get-the-tvl-of-a-user)
* [Get Top Farcaster Users Sorted By Their Total FarScores](farscores-and-farboosts.md#get-top-farcaster-users-sorted-by-their-total-farscores)
* [Get Top Farcaster Users Sorted By Their Organic FarScores](farscores-and-farboosts.md#get-top-farcaster-users-sorted-by-their-organic-farscores)
* [Get Top Farcaster Users Sorted By Their TVL Boost](farscores-and-farboosts.md#get-top-farcaster-users-sorted-by-their-tvl-boost)
* [Get Top Farcaster Users Sorted By Their Liquidity Boost](farscores-and-farboosts.md#get-top-farcaster-users-sorted-by-their-liquidity-boost)
* [Get Top Farcaster Users Sorted By Their Power Boost](farscores-and-farboosts.md#get-top-farcaster-users-sorted-by-their-power-boost)
* [Get All The Current Moxie Heroes](farscores-and-farboosts.md#get-all-the-current-moxie-heroes)

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

## Get The FarScores Of A User

You can get the FarScores of a user by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/cZBfxnGQFp" %}
Show me the FarScore of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery{
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
      realTimeFarScore {
<strong>        farScore # Get the FarScore here
</strong>      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "realTimeFarScore": {
            "farScore": 844.5147695782599
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The FarScores Of A User Without Any FarBoosts

You can get the original FarScores  of a user without any FarBoosts by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input to fetch the `organicFarScore` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OyiEiMSoc4" %}
Show me the organic FarScore of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery{
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
      realTimeFarScore {
        organicScore
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
          "realTimeFarScore": {
            "organicScore": 146.90271263184002
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Percentage Of Increase Of User's FarScore From FarBoosts

You can get the percentage of increase from FarBoosts by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input to fetch the `farScore` and `organicScore` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/SzgsDJQZoW" %}
Show me the total and organic FarScore of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery{
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
      realTimeFarScore {
        farScore
        organicScore
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
          "farcasterScore": {
            "farScore": 844.5147695782599.
            "organicScore": 7.051118295399999
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

To get the percentage of increase, simply divide `farBoost` by `farScore` and multiply it by 100%.

## Get The FarRank Of A User

You can get the FarRank of a user by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/21EkpJ8N4P" %}
Show me the FarRank of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery{
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
      realTimeFarScore {
        farRank
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
          "realTimeFarScore": {
            "farRank": 1
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The FarBoost Of A User

You can get the FarBoost of a user by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input to get all the boost variables:

### Try Demo

{% embed url="https://app.airstack.xyz/query/uwEi4v0hzV" %}
Show me the Farboost of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery{
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
      realTimeFarScore {
        lpBoost
        tvlBoost
        powerBoost
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
          "realTimeFarScore": {
            "lpBoost": 0,
            "tvlBoost": 7.687219015,
            "powerBoost": 0
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

By adding up the LP boost, TVL boost, and power boost, you'll get the total Farboost of a user.

## Get The FarBoost Of A User Gained From Providing Liquidity to DEXs (Also referred to as LP Boost)

You can get the Liquidity Boost, that is the FarBoost of a user gained from providing liquidity to DEXs, by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/N7cmuoNHYz" %}
Show me the Liquidity Boosts of FID 6500
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {_eq: farcaster},
        userId: {_eq: "6500"}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      realTimeFarScore {
<strong>        lpBoost
</strong>      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "realTimeFarScore": {
            "lpBoost": 83.5087240215
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The FarBoost Of A User Gained Only From TVL

You can get the TVL Boost, that is the FarBoost of a user gained only from TVL, by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Bdgw9p2rMX" %}
Show me the TVL Boosts of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery{
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
      realTimeFarScore {
<strong>        tvlBoost
</strong>      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Socials": {
      "Social": [
        {
          "realTimeFarScore": {
            // This value is 10^(-23) to the user's TVL
<strong>            "tvlBoost": 7.051118295399999
</strong>          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get The FarBoost Of A User Gained From Locking Fan Tokens (Moxie Power Boost)

You can get the Moxie Power Boost by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/vFjf2kNmik" %}
Show me the Moxie Power Boost of FID 602
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
        userId: {_eq: "602"}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      realTimeFarScore {
        powerBoost
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
          "realTimeFarScore": {
            "powerBoost": 9.738300070127268
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Moxie Hero Multiplier Of A User

You can get the Moxie Hero Multiplier by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input and first getting the Moxie Hero Boost:

### Try Demo

{% embed url="https://app.airstack.xyz/query/8y1QynQiRM" %}
Show me the Moxie Hero Boost of FID 488178
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
        userId: {_eq: "488178"}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      userId
      profileName
      farcasterScore {
        farBoost
        farRank
        farScore
        farScoreRaw
        heroBoost
        liquidityBoost
        powerBoost
        tvl
        tvlBoost
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Socials": {
      "Social": [
        {
          "userId": "488178",
          "profileName": "nikomfer.eth",
          "farcasterScore": {
            "farBoost": 290.87599810419005,
            "farRank": 3,
            "farScore": 582.6746181312901,
            "farScoreRaw": "58267461813129.003488414568103642",
            // This is the Moxie Hero Boost of the user
<strong>            "heroBoost": 291.337309065645,
</strong>            "liquidityBoost": 0,
            "powerBoost": 288.56944329691504,
            "tvl": "828868042240291844385275",
            "tvlBoost": 2.306554807275
          }
        },
        // Other Farcaster users
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

Once you get the Moxie Hero boost of the user, you can use the formula [here](../social-capital-value-and-social-capital-scores.md#heroes-boost) to get the multiplier.

## Get The TVL Of A User

You can get the TVL of a user in the Moxie protocol by using the [`Socials`](../api-references/api-reference/socials-api.md) API and inputting the user's FID in `userId` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Kers450lad" %}
Show me the TVL of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery{
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
      farcasterScore {
        tvl
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
          "farcasterScore": {
            "tvl": "705111829539999900000000"
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The value returned in `tvl` will be in wei. Therefore, you should divide the number by 10^18 to get the real value.

## Get Top Farcaster Users Sorted By Their Total FarScores

You can get the top Farcaster users sorted by their total Farscores by using the [`FarScores`](../api-references/api-reference/farscores.md) API and configure the API to sort by `farScore` in descending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/r7jxJs1CeD" %}
Show me top Farcaster users sorted by their total FarScores
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarScores(
    input: {
      filter: {},
      limit: 200,
      order: {farScore: DESC}
    }
  ) {
    FarScore {
      farScore
      social {
        profileName
        fid: userId
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
    "FarScores": {
      "FarScore": [
        {
          "farScore": 849.6258547342253,
          "social": {
            "profileName": "bigdegenenergy.eth",
            "fid": "317777"
          }
        },
        // Other Farcaster users
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Top Farcaster Users Sorted By Their Organic FarScores

You can get the top Farcaster users sorted by their organic Farscores by using the [`FarScores`](../api-references/api-reference/farscores.md) API and configure the API to sort by `organicScore` in descending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/i1CLBxR2Ob" %}
Show me top Farcaster users sorted by their organic FarScores
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarScores(
    input: {
      filter: {},
      limit: 200,
      order: {organicScore: DESC}
    }
  ) {
    FarScore {
      organicScore
      social {
        profileName
        fid: userId
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
    "FarScores": {
      "FarScore": [
        {
          "organicScore": 146.90271263184002,
          "social": {
            "profileName": "dwr.eth",
            "fid": "3"
          }
        },
        // Other Farcaster users
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Top Farcaster Users Sorted By Their TVL Boost

You can get the top Farcaster users sorted by their TVL boost by using the [`FarScores`](../api-references/api-reference/farscores.md) API and configure the API to sort by `tvlBoost` in descending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/UvEWaPH4Ic" %}
Show me top Farcaster users sorted by their TVL Boost
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarScores(
    input: {
      filter: {},
      limit: 200,
      order: {tvlBoost: DESC}
    }
  ) {
    FarScore {
      tvlBoost
      social {
        profileName
        fid: userId
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
    "FarScores": {
      "FarScore": [
        {
          "tvlBoost": 61.1144161984,
          "social": {
            "profileName": "betashop.eth",
            "fid": "602"
          }
        },
        // Other Farcaster users
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Top Farcaster Users Sorted By Their Liquidity Boost

You can get the top Farcaster users sorted by their Liquidity Boost by using the [`FarScores`](../api-references/api-reference/farscores.md) API and configure the API to sort by `lpBoost` in descending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ADWxrYZppB" %}
Show me top Farcaster users sorted by their Liquidity Boost
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarScores(
    input: {
      filter: {},
      limit: 200,
      order: {lpBoost: DESC}
    }
  ) {
    FarScore {
      lpBoost
      social {
        profileName
        fid: userId
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
    "FarScores": {
      "FarScore": [
        {
          "lpBoost": 848.5269513826,
          "social": {
            "profileName": "bigdegenenergy.eth",
            "fid": "317777"
          }
        },
        // Other Farcaster users
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Top Farcaster Users Sorted By Their Power Boost

You can get the top Farcaster users sorted by their Power Boost by using the [`FarScores`](../api-references/api-reference/farscores.md) API and configure the API to sort by `powerBoost` in descending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/eD4AiM2OJm" %}
Show me the top Farcaster users sorted by their power boost
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarScores(
    input: {
      filter: {},
      limit: 200,
      order: {powerBoost: DESC}
    }
  ) {
    FarScore {
      powerBoost
      social {
        profileName
        fid: userId
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
    "FarScores": {
      "FarScore": [
        {
          "powerBoost": 61.1144161984,
          "social": {
            "profileName": "farcastor.eth",
            "fid": "308322"
          }
        },
        // Other Farcaster users
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All The Current Moxie Heroes

You can get the Farcaster users that are currently Moxie Heroes by using the [`FarScores`](../api-references/api-reference/farscores.md) API and configure the API by setting the `heroBoost` filter to greater than 0 to only fetch those with Hero boost:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qTeDIhtSrl" %}
Show me all the current Moxie Heroes
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarScores(
    input: {
      filter: {
        # Only get user that have hero boost
<strong>        heroBoost: {_gt: 0}
</strong>      },
      limit: 200
    }
  ) {
    FarScore {
      farScore
      heroBoost
      social {
        profileName
        fid: userId
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
    "FarScores": {
      "FarScore": [
        {
          "farScore": 87.00799974768002,
          "heroBoost": 86.888154018,
          "social": {
            "profileName": "banthafodderdan",
            "fid": "379581"
          }
        },
        {
          "farScore": 94.05552537371149,
          "heroBoost": 70.5416440302836,
          "social": {
            "profileName": "aaronv.eth",
            "fid": "354795"
          }
        },
        // Other Moxie Heroes
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching FarScore & FarBoosts data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarScores, FarBoost, and Cast Scores](../social-capital-value-and-social-capital-scores.md)
* [Socials API References](../api-references/api-reference/socials-api.md)
* [FarScores API References](../api-references/api-reference/farscores.md)
