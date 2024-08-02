---
description: >-
  Learn how to fetch the Moxie earning details of casts and replies and how it
  is split among the casters and fan token holders.
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

# ✂️ Farcaster Cast Moxie Earning Details

## Introduction

The Moxie Protocol enables Farcaster Members to earn Everyday Rewards based on who engages with their casts. These rewards are automatically split as follows:

* 50% to the member who casted
* 20% to their Fans (holders of their [Fan Tokens](https://build.moxie.xyz/the-moxie-protocol/moxie-protocol/fan-tokens))
* 20% to the Fans of the Channel casted into (holders of their [Fan Tokens](https://build.moxie.xyz/the-moxie-protocol/moxie-protocol/fan-tokens))
* 10% to Fans of Farcaster overall (holders of the [Fan Tokens](https://build.moxie.xyz/the-moxie-protocol/moxie-protocol/fan-tokens))

[Learn more about Moxie Everyday Rewards](https://build.moxie.xyz/the-moxie-protocol/everyday-rewards)

## Table Of Contents

In this tutorial, you will learn various use cases to get Moxie earning details of a cast and how they are split among members and Fan Token holders:

* [Get User's Casts With Their Moxie Earning Details](farcaster-cast-moxie-earning-details.md#get-users-casts-with-their-moxie-earning-details)
* [Get User's Replies With Their Moxie Earning Details](farcaster-cast-moxie-earning-details.md#get-users-replies-with-their-moxie-earning-details)
* [Get All Cast In A Channel And Their Moxie Earning Details](farcaster-cast-moxie-earning-details.md#get-all-cast-in-a-channel-and-their-moxie-earning-details)
* [Get Certain Cast Moxie Earning Details](farcaster-cast-moxie-earning-details.md#get-certain-cast-moxie-earning-details)
* [Get Certain Reply Moxie Earning Details](farcaster-cast-moxie-earning-details.md#get-certain-reply-moxie-earning-details)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL

### Get Started

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

## Get User's Casts With Their Moxie Earning Details

You can use the [`FarcasterCasts`](../api-references/api-reference/farcastercasts-api.md) API to fetch all user's casts and get their Moxie earning details by providing the FID of the user to the `castedBy` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/VRqqqYHXWn" %}
Show me the Moxie earning details of casts casted by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        castedBy: {
<strong>          _eq: "fc_fid:602" # Specify user's FID here
</strong>        }
      },
      blockchain: ALL
    }
  ) {
    Cast {
      hash
      # Get the moxie earning details here
<strong>      moxieEarningsSplit {
</strong>        earnerType
        earningsAmount
        earningsAmountInWei
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Responses" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterCasts": {
      "Cast": [
        {
          "hash": "0x34e6bbb99b9166bc2098e109a328bdfd1dfaa207",
          "moxieEarningsSplit": [
            {
              // Earnings for the caster
<strong>              "earnerType": "CREATOR",
</strong>              "earningsAmount": 0.052771409349,
              "earningsAmountInWei": "52771409349000000"
            },
            {
              // Earnings for the channel fan token holders
<strong>              "earnerType": "CHANNEL_FANS",
</strong>              "earningsAmount": 0.00338907949,
              "earningsAmountInWei": "3389079490000000"
            },
            {
              // Earnings for the Farcaster network token holders
<strong>              "earnerType": "NETWORK",
</strong>              "earningsAmount": 0.008022926977,
              "earningsAmountInWei": "8022926977000000"
            },
            {
              // Earnings for the fan token holders
<strong>              "earnerType": "CREATOR_FANS",
</strong>              "earningsAmount": 0.016045853954,
              "earningsAmountInWei": "16045853954000000"
            }
          ]
        },
        // Other casts
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If [`moxieEarningSplit`](../api-references/objects/moxieearningssplit.md) is returned as `null`, then it implies that the cast has not earned any Moxie.

## Get User's Replies With Their Moxie Earning Details

You can use the [`FarcasterReplies`](../api-references/api-reference/farcasterreplies-api.md) API to fetch all user's replies and get their Moxie earning details by providing the FID of the user to the `repliedBy` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/mgRZOD49Uj" %}
Show me all the replies casted by FID 602 and their Moxie earning details
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterReplies(
    input: {
      filter: {
<strong>        repliedBy: {_eq: "fc_fid:602"} # Specify user's FID here
</strong>      },
      blockchain: ALL
    }
  ) {
    Reply {
      hash
      # Get the moxie earning details here
<strong>      moxieEarningsSplit {
</strong>        earnerType
        earningsAmount
        earningsAmountInWei
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Responses" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterReplies": {
      "Reply": [
        {
          "hash": "0x8db996f6515509d32fc1524740b8010839ca754b",
          "moxieEarningsSplit": [
            {
              // Earnings for the Farcaster network fan token holder
<strong>              "earnerType": "NETWORK",
</strong>              "earningsAmount": 0.040443829698,
              "earningsAmountInWei": "40443829698000000"
            },
            {
              // Earnings for the caster
<strong>              "earnerType": "CREATOR",
</strong>              "earningsAmount": 0.283106807886,
              "earningsAmountInWei": "283106807886000000"
            },
            {
              // Earnings for fan token holders
<strong>              "earnerType": "CREATOR_FANS",
</strong>              "earningsAmount": 0.080887659396,
              "earningsAmountInWei": "80887659396000000"
            }
          ]
        },
        // Other replies
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If [`moxieEarningSplit`](../api-references/objects/moxieearningssplit.md) is returned as `null`, then it implies that the cast has not earned any Moxie.

## Get All Cast In A Channel And Their Moxie Earning Details

You can use the [`FarcasterCasts`](../api-references/api-reference/farcastercasts-api.md) API to fetch all the casts casted in a certain channel and get their Moxie earning details by providing the channel URL to the `rootParentUrl` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/NZolBm0J3h" %}
Show me the Moxie earning details of all casts in /airstack channel
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        rootParentUrl: {
          # Specify channel URL here
<strong>          _eq: "https://warpcast.com/~/channel/airstack"
</strong>        }
      },
      blockchain: ALL
    }
  ) {
    Cast {
      hash
      # Get the moxie earning details here
<strong>      moxieEarningsSplit {
</strong>        earnerType
        earningsAmount
        earningsAmountInWei
      }
      rootParentUrl
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterCasts": {
      "Cast": [
        {
          "hash": "0x5cfb536eceeebb9fa54b8390ff4fc1e5eba582cc",
          "moxieEarningsSplit": [
            {
              "earnerType": "CREATOR",
              "earningsAmount": 0.056955485088,
              "earningsAmountInWei": "56955485088000000"
            },
            {
              "earnerType": "NETWORK",
              "earningsAmount": 0.006328387232,
              "earningsAmountInWei": "6328387232000000"
            }
          ],
          "rootParentUrl": "https://warpcast.com/~/channel/airstack"
        },
        // Other casts in /airstack channel
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

If [`moxieEarningSplit`](../api-references/objects/moxieearningssplit.md) is returned as `null`, then it implies that the cast has not earned any Moxie.

## Get Certain Cast Moxie Earning Details

You can use the [`FarcasterCasts`](../api-references/api-reference/farcastercasts-api.md) API to fetch a certain cast and get its Moxie earning details by providing the cast hash to the `hash` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/7UgaIjJLmA" %}
Show me the Moxie earning details of cast hash 0x34e6bbb99b9166bc2098e109a328bdfd1dfaa207
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        hash: {
          # specify the cast hash here
<strong>          _eq: "0x34e6bbb99b9166bc2098e109a328bdfd1dfaa207"
</strong>        }
      },
      blockchain: ALL
    }
  ) {
    Cast {
      hash
      # Get the moxie earning details here
<strong>      moxieEarningsSplit {
</strong>        earnerType
        earningsAmount
        earningsAmountInWei
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Responses" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterCasts": {
      "Cast": [
        {
          "hash": "0x34e6bbb99b9166bc2098e109a328bdfd1dfaa207",
          "moxieEarningsSplit": [
            {
              // Earnings for the caster
<strong>              "earnerType": "CREATOR",
</strong>              "earningsAmount": 0.052771409349,
              "earningsAmountInWei": "52771409349000000"
            },
            {
              // Earnings for the channel fan token holders
<strong>              "earnerType": "CHANNEL_FANS",
</strong>              "earningsAmount": 0.00338907949,
              "earningsAmountInWei": "3389079490000000"
            },
            {
              // Earnings for the Farcaster network token holders
<strong>              "earnerType": "NETWORK",
</strong>              "earningsAmount": 0.008022926977,
              "earningsAmountInWei": "8022926977000000"
            },
            {
              // Earnings for the fan token holders
<strong>              "earnerType": "CREATOR_FANS",
</strong>              "earningsAmount": 0.016045853954,
              "earningsAmountInWei": "16045853954000000"
            }
          ]
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If [`moxieEarningSplit`](../api-references/objects/moxieearningssplit.md) is returned as `null`, then it implies that the cast has not earned any Moxie.

## Get Certain Reply Moxie Earning Details

You can use the [`FarcasterReplies`](../api-references/api-reference/farcasterreplies-api.md) API to fetch a certain reply and get its Moxie earning details by providing the cast hash to the `hash` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/1LATUwtERa" %}
Show the Moxie earning details of reply cast hash 0x8db996f6515509d32fc1524740b8010839ca754b
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterReplies(
    input: {
      filter: {
        hash: {
          # specify the reply cast hash here
<strong>          _eq: "0x8db996f6515509d32fc1524740b8010839ca754b"
</strong>        }
      },
      blockchain: ALL
    }
  ) {
    Reply {
      hash
      # Get the moxie earning details here
<strong>      moxieEarningsSplit {
</strong>        earnerType
        earningsAmount
        earningsAmountInWei
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Responses" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterReplies": {
      "Reply": [
        {
          "hash": "0x8db996f6515509d32fc1524740b8010839ca754b",
          "moxieEarningsSplit": [
            {
              // Earnings for the Farcaster network fan token holder
<strong>              "earnerType": "NETWORK",
</strong>              "earningsAmount": 0.040443829698,
              "earningsAmountInWei": "40443829698000000"
            },
            {
              // Earnings for the caster
<strong>              "earnerType": "CREATOR",
</strong>              "earningsAmount": 0.283106807886,
              "earningsAmountInWei": "283106807886000000"
            },
            {
              // Earnings for fan token holders
<strong>              "earnerType": "CREATOR_FANS",
</strong>              "earningsAmount": 0.080887659396,
              "earningsAmountInWei": "80887659396000000"
            }
          ]
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If [`moxieEarningSplit`](../api-references/objects/moxieearningssplit.md) is returned as `null`, then it implies that the cast has not earned any Moxie.

## Developer Support

If you have any questions or need help regarding fetching Moxie earnings details data on Farcaster casts and replies, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarcasterCasts API Reference](../api-references/api-reference/farcastercasts-api.md)
* [FarcasterReplies API Reference](../api-references/api-reference/farcasterreplies-api.md)
* [Moxie developer docs](https://developer.moxie.xyz)
