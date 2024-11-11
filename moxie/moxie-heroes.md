---
description: >-
  Learn how to fetch all the Farcaster users that are currently designated as
  Moxie Heroes and the variable boost they received on their FarScore.
---

# 🦸 Moxie Heroes

Moxie Heroes are designated each week based on their prior week's quality and diversity of engagement. Every week Moxie Heroes get a variable boost to their FarScore based on their existing score.

Heroes can keep these powers for themselves or transfer to another user. Heroes' superpowers last for a full week. A new set of heroes is launched each week based on previous week diversity and quality of engagement. It will be rotated so no one is hero in consecutive weeks.

## Table Of Contents

* [Get All The Current Moxie Heroes](moxie-heroes.md#get-all-the-current-moxie-heroes)
* [Get The Moxie Hero Multiplier Of A User](moxie-heroes.md#get-the-moxie-hero-multiplier-of-a-user)

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

## Developer Support

If you have any questions or need help regarding fetching FarScore & FarBoosts data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarScores, FarBoost, and Cast Scores](../social-capital-value-and-social-capital-scores.md)
* [Socials API References](../api-references/api-reference/socials-api.md)
* [FarScores API References](../api-references/api-reference/farscores.md)
