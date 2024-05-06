---
description: >-
  Learn how to use the Airstack API to fetch Farcaster likes reaction either
  from a certain user or a specific cast hash.
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

# 👍 Farcaster Likes

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Get All Casts Liked By A User](farcaster-likes.md#get-all-casts-liked-by-a-user)
* [Get All Users That Like A Certain Cast](farcaster-likes.md#get-all-users-that-like-a-certain-cast)
* [Get All Users That Likes Any Casts That Contains A Certain Farcaster Frames](farcaster-likes.md#get-all-users-that-likes-any-casts-that-contains-a-certain-farcaster-frames)
* [Check If A User Like A Certain Cast](farcaster-likes.md#check-if-a-user-like-a-certain-cast)

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

## Get All Casts Liked By A User

You can use the [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) APi to get all the casts liked by a user by providing the user's [identity](../../api-references/api-reference/airstack-identity-api.md) to `reactedBy` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/NbGtNTKIIt" %}
Show me all likes from FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReactions(
    input: {
      filter: {
        criteria: liked,
        reactedBy: {_eq: "fc_fid:602"}
      },
      blockchain: ALL,
      limit: 200
    }
  ) {
    Reaction {
      cast {
        castedAtTimestamp
        embeds
        url
        text
        numberOfRecasts
        numberOfLikes
        channel {
          channelId
        }
        mentions {
          fid
          position
        }
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
    "FarcasterReactions": {
      "Reaction": [
        {
          "cast": {
            "castedAtTimestamp": "2024-05-05T04:28:38Z",
            "embeds": [
              {
                "castId": {
                  "fid": 7960,
                  "hash": "0xf5dd28030a12288d0bf7b9230357269c8228a380"
                }
              }
            ],
            "url": "https://warpcast.com/marissaposner/0xf811805d",
            "text": "Proud of my cofounder @sheldon for helping drive the narrative on how layer 3s are a new way to drive value to ecosystems 🥰⬆️\n\nI’m very bullish on L3s for custom use cases (like gaming) and community-based projects. Do you think most L3s will merge together or will there be a rise of separate chains?",
            "numberOfRecasts": 0,
            "numberOfLikes": 17,
            "channel": {
              "channelId": "farcon"
            },
            "mentions": [
              {
                "fid": "7960",
                "position": 22
              }
            ]
          }
        },
        // Other likes from FID 602
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Users That Like A Certain Cast

You can use the [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) APi to get all the users that liked a cast by providing the cast hash to `castHash` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/XvdcngN5P8" %}
Show me all Farcaster user that like cast hash 0xfc328b9ba0a18fa271c508f9b91b0b66f23062fe
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReactions(
    input: {
      filter: {
        criteria: liked,
        castHash: {_eq: "0xfc328b9ba0a18fa271c508f9b91b0b66f23062fe"}
      },
      blockchain: ALL,
      limit: 200
    }
  ) {
    Reaction {
      reactedBy {
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
    "FarcasterReactions": {
      "Reaction": [
        {
          "reactedBy": {
            "profileName": "phil",
            "fid": "129"
          }
        },
        // Other user like the cast
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Users That Likes Any Casts That Contains A Certain Farcaster Frames

You can use [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) API to fetch all users that likes all casts that contains a certain Frames by providing the Frames URL to the `frameUrl` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4Wld2IPY8F" %}
Show me all users that likes any casts with Frames URL https://gallery.manifold.xyz/venicebreeze
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReactions(
    input: {
      filter: {
        criteria: liked,
        frameUrl: {_eq: "https://gallery.manifold.xyz/venicebreeze"}
      },
      blockchain: ALL,
      limit: 200
    }
  ) {
    Reaction {
      reactedBy {
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
    "FarcasterReactions": {
      "Reaction": [
        {
          "reactedBy": {
            "profileName": "matthewbian",
            "fid": "461708"
          }
        },
        // Other users that likes
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If A User Like A Certain Cast

You can use the [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) APi to check if a user like a certain cast by providing the cast hash to `castHash` and the user's identity to the `reactedBy` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/vPWSYUgIC1" %}
Check if FID 602 like cast hash 0xfc328b9ba0a18fa271c508f9b91b0b66f23062fe
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReactions(
    input: {
      filter: {
        criteria: liked,
        castHash: {_eq: "0xfc328b9ba0a18fa271c508f9b91b0b66f23062fe"},
        reactedBy: {_eq: "fc_fid:602"}
      },
      blockchain: ALL
    }
  ) {
    Reaction {
      castHash
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterReactions": {
      "Reaction": [
        {
          // if not null, then user liked the cast
<strong>          "castHash": "0xfc328b9ba0a18fa271c508f9b91b0b66f23062fe"
</strong>        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster likes data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarcasterReactions API Reference](../../api-references/api-reference/farcasterreactions-api.md)
* [Farcaster Frames Guides](farcaster-frames.md)
