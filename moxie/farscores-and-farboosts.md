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
* [Get The TVL Of A User](farscores-and-farboosts.md#get-the-tvl-of-a-user)

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

You can get the FarScores of a user by using the `Socials` API and inputting the user's FID in `userId` input:

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
      farcasterScore {
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
          "farcasterScore": {
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

You can get the original FarScores  of a user without any FarBoosts by using the `Socials` API and inputting the user's FID in `userId` input to fetch the `farScore` and `farBoost` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OyiEiMSoc4" %}
Show me the FarScore & Farboosts of FID 3
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
        farScore
        farBoost
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
            "farBoost": 7.051118295399999
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

To get the original FarScore without any FarBoosts, simply substract `farScore` to `farBoost`.

## Get The Percentage Of Increase Of User's FarScore From FarBoosts

You can get the percentage of increase from FarBoosts by using the `Socials` API and inputting the user's FID in `userId` input to fetch the `farScore` and `farBoost` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OyiEiMSoc4" %}
Show me the FarScore & Farboosts of FID 3
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
        farScore
        farBoost
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
            "farBoost": 7.051118295399999
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

You can get the FarRank of a user by using the `Socials` API and inputting the user's FID in `userId` input:

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
      farcasterScore {
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
          "farcasterScore": {
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

You can get the FarBoost of a user by using the `Socials` API and inputting the user's FID in `userId` input:

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
      farcasterScore {
        farBoost
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
            "farBoost": 7.051118295399999
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The FarBoost Of A User Gained From Providing Liquidity to DEXs (Also referred to as LP Boost)

You can get the Liquidity Boost, that is the FarBoost of a user gained from providing liquidity to DEXs, by using the `Socials` API and inputting the user's FID in `userId` input:

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
      farcasterScore {
<strong>        liquidityBoost
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
          "farcasterScore": {
            "liquidityBoost": 83.5087240215
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

You can get the TVL Boost, that is the FarBoost of a user gained only from TVL, by using the `Socials` API and inputting the user's FID in `userId` input:

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
      farcasterScore {
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
          "farcasterScore": {
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

You can get the Moxie Power Boost by using the `Socials` API and inputting the user's FID in `userId` input:

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
      farcasterScore {
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
          "farcasterScore": {
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

## Get The TVL Of A User

You can get the TVL of a user in the Moxie protocol by using the `Socials` API and inputting the user's FID in `userId` input:

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

## Developer Support

If you have any questions or need help regarding fetching FarScore & FarBoosts data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarScores, FarBoost, and Cast Scores](../social-capital-value-and-social-capital-scores.md)
* [Socials API References](../api-references/api-reference/socials-api.md)
