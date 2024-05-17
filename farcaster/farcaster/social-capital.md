---
description: >-
  Learn how to fetch Social Capital Scores and Ranks of Farcaster users, and how
  to fetch Social Capital Values of Farcaster casts.
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

# 🫂 Social Capital

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get A Farcaster User's Social Capital Score](social-capital.md#get-a-farcaster-users-social-capital-score)
* [Get A Farcaster User's Social Capital Rank](social-capital.md#get-a-farcaster-users-social-capital-rank)
* [Get All Farcaster Users Sorted By Social Capital Scores](social-capital.md#get-all-farcaster-users-sorted-by-social-capital-scores)
* [Get All Farcaster Users With Social Capital Scores > X](social-capital.md#get-all-farcaster-users-with-social-capital-scores-greater-than-x)
* [Get All Farcaster Users Sorted By Social Capital Rank](social-capital.md#get-all-farcaster-users-sorted-by-social-capital-rank)
* [Get All Farcaster Users With Social Capital Rank < X](social-capital.md#get-all-farcaster-users-with-social-capital-rank-less-than-x)
* [Get A Farcaster Casts's Social Capital Value](social-capital.md#get-a-farcaster-castss-social-capital-value)

## Social Capital Scores

* Social Capital Scores (SCS) are a measure of each Farcaster user's influence in the network.&#x20;
  * This score is derived from a variety of onchain criteria (including Farcaster Hubs). Users with higher SCSs have more positive downstream influence, as their actions and engagements tends to result in higher quality overall engagement.
  * User's rank for their SCS is represented by **Social Capital Rank (SCR)**.
  * Currently, the SCS is powered by a proprietary algorithm from Airstack. This algorithm is kept confidential to prevent manipulation.
  * Over time, we aim to refine the SCS based on its performance and eventually release it as an open-source protocol.

## Social Capital Value

* **Social Capital Value (SCV)** is a metric developed by Airstack to identify high-quality Trending Casts on Farcaster.
* The SCS of a user impacts the SCV of a cast when the user engages with it. The type of engagement also plays a role. The accumulated SCS of all interactions contributes to the cast's overall SCV. Casts are then ranked by their SCV, promoting higher-quality casts.

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
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

## Get A Farcaster User's Social Capital Score

You can use [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch a Farcaster user's social capital score by providing the Farcaster user's fname or fid (check [here](../../api-references/api-reference/airstack-identity-api.md#farcaster-id-and-name) for identity formats) to the `identity` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/VriGp7QDIF" %}
Show me social capital score of FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {
          _eq: farcaster
        },
        identity: { _eq: "fc_fid:602" }
      },
      blockchain: ethereum
    }
  ) {
    Social {
      socialCapital {
        socialCapitalScoreRaw
        socialCapitalScore
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
          "socialCapital": {
            "socialCapitalScoreRaw": "4354877467024.49997743418218819",
            "socialCapitalScore": 43.548774670245
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get A Farcaster User's Social Capital Rank

{% embed url="https://www.youtube.com/embed/PymaxiF9I7k?start=219&end=339" %}
Get Farcaster User's Social Capital Rank
{% endembed %}

You can use [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch a Farcaster user's social capital rank by providing the Farcaster user's fname or fid (check [here](../../api-references/api-reference/airstack-identity-api.md#farcaster-id-and-name) for identity formats) to the `identity` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/3ScuTuWE4B" %}
Show me social capital rank of FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {
          _eq: farcaster
        },
        identity: { _eq: "fc_fid:602" }
      },
      blockchain: ethereum
    }
  ) {
    Social {
      socialCapital {
        socialCapitalRank
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
          "socialCapital": {
<strong>            "socialCapitalRank": 70 // rank 70
</strong>          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get All Farcaster Users Sorted By Social Capital Scores

{% embed url="https://www.youtube.com/embed/PymaxiF9I7k?start=83&end=219" %}
Get Farcaster Users Sorted By Social Capital Scores
{% endembed %}

You can use the [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch all Farcaster users sorted by [social capital scores](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md) by adding `socialCapitalScore` to the `order` field and set it to `DESC` value to sort in descending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/QAr0y5Iowm" %}
Show me all Farcaster users sorted by social capital scores
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {_eq: farcaster}
      },
      blockchain: ethereum,
<strong>      order: {socialCapitalScore: DESC}, # Add this to sort by SCS
</strong>      limit: 200
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      socialCapital {
        socialCapitalScore
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
    "Socials": {
      "Social": [
        {
          "profileName": "dwr.eth",
          "fid": "3",
          "custodyAddress": "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
          "connectedAddresses": [
            {
              "address": "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
              "blockchain": "ethereum"
            },
            {
              "address": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "blockchain": "ethereum"
            }
          ],
          "socialCapital": {
            "socialCapitalScore": 279.70459538175
          }
        },
        // Other highly influential user on Farcaster network
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users With Social Capital Scores > X

You can use the [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch all Farcaster users with [social capital scores](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md) above certain number by using the `socialCapitalScore` input field and provide the **X** value to the `_gt` filter (for other comparators, check out [here](../../api-references/overview/working-with-graphql.md)).

In the example below, **X** is 50:

{% hint style="info" %}
If you would like to also sort the result by social capital score, simply add `socialCapitalScore` to the `order` field and set the value to `DESC`.\
\
To learn more how to do it, click [here](social-capital.md#get-all-farcaster-users-sorted-by-social-capital-scores).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/hR1DZjUvy3" %}
Show me all Farcaster users with social capital scores of at least 50
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {_eq: farcaster},
<strong>        socialCapitalScore: {_gt: 50} # greater than to 50
</strong>      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      socialCapital {
        socialCapitalScore
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
    "Socials": {
      "Social": [
        {
          "profileName": "dwr.eth",
          "fid": "3",
          "custodyAddress": "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
          "connectedAddresses": [
            {
              "address": "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
              "blockchain": "ethereum"
            },
            {
              "address": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "blockchain": "ethereum"
            }
          ],
          "socialCapital": {
            "socialCapitalScore": 279.70459538175
          }
        },
        // Other Farcaster users w/ SCS > 50
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users Sorted By Social Capital Rank

You can use the [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch all Farcaster users sorted by[ social capital rank](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md) by adding `socialCapitalRank` to the `order` field and set it to `ASC` value to sort in ascending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/lnGIDqKyQN" %}
Show me all Farcaster users sorted by social capital rank
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      # Order by social capital rank in ascending order
<strong>      order: {socialCapitalRank: ASC},
</strong>      filter: {
        dappName: {
          _eq: farcaster
        }
      },
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      socialCapital {
        socialCapitalRank
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
    "Socials": {
      "Social": [
        {
          "profileName": "dwr.eth",
          "fid": "3",
          "custodyAddress": "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
          "connectedAddresses": [
            {
              "address": "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
              "blockchain": "ethereum"
            },
            {
              "address": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "blockchain": "ethereum"
            }
          ],
          "socialCapital": {
            "socialCapitalRank": 1
          }
        },
        // Other Farcaster users ranked by social capital rank
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users With Social Capital Rank < X

{% embed url="https://www.youtube.com/embed/PymaxiF9I7k?start=340" %}
Get Farcaster Users With Social Capital Rank < X
{% endembed %}

You can use the [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch all Farcaster users with [social capital rank](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md) below certain value by using the `socialCapitalRank` input field and the `_lte` operator. If you prefer other comparator for your logic, you can use other available comparator that suits you need (check more [here](../../api-references/overview/working-with-graphql.md)).

{% hint style="info" %}
If you would like to also sort the result by social capital rank, simply add `socialCapitalRank` to the `order` field and set the value to `ASC`.\
\
To learn more how to do it, click [here](social-capital.md#get-all-farcaster-users-sorted-by-social-capital-rank).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/gesg8XwOZA" %}
Show all Farcaster users with social captial rank below or equal to 100
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {
          _eq: farcaster
        },
        socialCapitalRank: {
<strong>          _lte: 100 # less than or equal to 100
</strong>        }
      },
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      socialCapital {
        socialCapitalRank
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
    "Socials": {
      "Social": [
        {
          "profileName": "corbin.eth",
          "fid": "358",
          "custodyAddress": "0x3bcadeaef7f8e3bf5ec9ca5212cc9c72556c43ed",
          "connectedAddresses": [
            {
              "address": "0x869ec00fa1dc112917c781942cc01c68521c415e",
              "blockchain": "ethereum"
            }
          ],
          "socialCapital": {
            "socialCapitalRank": 82
          }
        },
        // Other user with social capital rank below or equal to 100
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get A Farcaster Casts's Social Capital Value

You can use `FarcasterCasts` API to fetch a Farcaster cast's [social capital value](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md) by providing the Warpcast URL  to the `url` input:

{% hint style="info" %}
Alternatively, you can also use the `hash` input filter and provide the cast hash if you have the cast hash value.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/4LTaOSI4Mm" %}
Show me social capital value of a cast
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        url: {_eq: "https://warpcast.com/dannylove/0xabc68559"}
      },
      blockchain: ALL
    }
  ) {
    Cast {
      socialCapitalValue {
        rawValue
        formattedValue
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
    "FarcasterCasts": {
      "Cast": [
        {
          "socialCapitalValue": {
            "rawValue": "33984720",
            "formattedValue": 0.0003398472
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

If you have any questions or need help regarding fetching Social Capital data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Social Capital Value & Social Capital Score](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
* [FarcasterCasts API Reference](../../api-references/api-reference/farcastercasts-api.md)
