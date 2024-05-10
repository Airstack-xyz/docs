---
description: >-
  Learn how to use Airstack to show Trending Casts on Farcaster based on various
  engagement criteria.
---

# 💜 All Farcaster Users

## Criteria

Airstack provides multiple ways to fetch Trending Casts

<table><thead><tr><th width="184">Name</th><th width="268">Value</th><th>Description</th></tr></thead><tbody><tr><td><code>criteria</code></td><td><p><code>social_capital_value |</code></p><p><code>likes</code> | <br><code>recasts</code> | <br><code>replies</code> | <code>likes_recasts_replies</code> |<br></p></td><td>This will calculate and sort the Farcaster casts based on the chosen criteria:<br>- <code>social_capital_value</code>: the <a href="social-capital-value-and-social-capital-scores.md">social capital value</a> associated with the cast<br>- <code>likes</code>: number of likes on the cast<br>- <code>recasts</code>: number of recasts on the cast<br>- <code>replies</code>: number of replies on the cast<br>- <code>likes_recasts_replies</code>: number of total likes, recasts, and replies  on the cast combined</td></tr><tr><td><code>timeFrame</code></td><td><code>one_hour</code> | <br><code>two_hours</code> | <br><code>four_hours</code> | <br><code>eight_hours</code> | <code>twelve_hours</code> | <br><code>one_day</code> | <br><code>two_days</code> | <br><code>seven_days</code></td><td>Only fetch trending casts that have the most engagement within the chosen time frame, e.g. <code>one_hour</code> will fetch trending Farcaster casts with the most engagement for the last 1 hour.</td></tr></tbody></table>

## Social Capital Value

A new metric, **Social Capital Value (SCV),** has been introduced to identify high-quality trending broadcasts. **SCV** incorporates the authority of users, measured by another metric, **Social Capital Scores (SCS)**, who engage with the broadcast based on on-chain data.

For additional information on **SCV** and **SCS**, please refer to the following section [here](social-capital-value-and-social-capital-scores.md).

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

## Fetch Trending Casts

Once you have the `criteria` and `timeFrame` parameters prepared, simply use the query below and add the parameters as variables:&#x20;

### Try Demo

{% embed url="https://app.airstack.xyz/query/31Y5aCzGg4" %}
Show all trending Farcaster casts in the last 1 hour sorted by social capital value
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery(
  $criteria: TrendingCastsCriteria!,
  $timeFrame: TrendingCastTimeFrame!
) {
  TrendingCasts(
    input: {
      blockchain: ALL,
      criteria: $criteria,
      timeFrame: $timeFrame,
    }
  ) {
    TrendingCast {
      criteria
      criteriaCount
      hash
      id
      socialCapitalValueFormatted
      socialCapitalValueRaw
      timeFrom
      timeTo
      cast {
        text
        mentions {
          fid
          position
        }
        embeds
        url
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
  "criteria": "social_capital_value"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TrendingCasts": {
      "TrendingCast": [
        {
          "criteria": "social_capital_value",
          "criteriaCount": 10476.43430537895,
          "hash": "0x1f48c83b51e395c433f984ab707bfc84cece7cb5",
          "id": "0x1f48c83b51e395c433f984ab707bfc84cece7cb5",
          "socialCapitalValueFormatted": 10476.43430537895,
          "socialCapitalValueRaw": "1047643430537860",
          "timeFrom": "2024-04-24T17:30:00Z",
          "timeTo": "2024-04-25T10:00:00Z",
          "cast": {
            "text": "FARCON REQUEST\n\nif you are building FC-native or -integrated project, please reply to this cast with:\n\n- the URL to your project (could be warpcast profile, but pls have logo on whatever URL you share)\n- the usernames of the people on your team\n\nplease do ASAP as we're cooking up something fun",
            "mentions": [],
            "embeds": [],
            "url": "https://warpcast.com/ted/0x1f48c83b"
          }
        },
        // Other trending Farcaster casts
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

🎉 :partying\_face: Congratulations you've just fetched all the trending Farcaster casts and integrate it into your application!

## **D**eveloper Support

If you have any questions or need help regarding integrating or building trending casts into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TrendingCasts API Reference](../../api-references/api-reference/trendingcasts-api.md)
