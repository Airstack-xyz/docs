---
description: >-
  Learn how to fetch Farcaster Frames data using the Airstack API and its
  various use cases and combinations.
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

# 🖼️ Farcaster Frames

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Get All The Frames Casted Within Certain Period Of Time](farcaster-frames.md#get-all-the-frames-casted-within-certain-period-of-time)
* [Get All The Frames Casted By Certain User](farcaster-frames.md#get-all-the-frames-casted-by-certain-user)
* [Get All the Casts That Contains Certain Frame](farcaster-frames.md#get-all-the-casts-that-contains-certain-frame)
* [Get Details Of A Certain Farcaster Frames](farcaster-frames.md#get-details-of-a-certain-farcaster-frames)
* [Get All Replies From All Casts That Contains Certain Farcaster Frames](farcaster-frames.md#get-all-replies-from-all-casts-that-contains-certain-farcaster-frames)
* [Get All Users That Recasts Any Casts That Contains A Certain Farcaster Frames](farcaster-frames.md#get-all-users-that-recasts-any-casts-that-contains-a-certain-farcaster-frames)
* [Get All Users That Likes Any Casts That Contains A Certain Farcaster Frames](farcaster-frames.md#get-all-users-that-likes-any-casts-that-contains-a-certain-farcaster-frames)

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

## Get All The Frames Casted Within Certain Period Of Time

You can fetch all the frames casted within certain period of time by using the [`FarcasterCasts`](../../api-references/api-reference/farcastercasts-api.md) API and specify the time period in the `castedAtTimestamp` filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/nJprqTA3Tb" %}
Show me all Farcaster Frames casted since Jan 1, 2024
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        castedAtTimestamp: {_gte: "2024-01-01T00:00:00Z"},
        hasFrames: {_eq: true}
      },
      blockchain: ALL
    }
  ) {
    Cast {
      castedBy {
        profileName
        fid: userId
      }
      frame {
        buttons {
          action
          id
          index
          target
        }
        castedAtTimestamp
        frameUrl
        imageAspectRatio
        imageUrl
        inputText
        postUrl
        state
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
          "castedBy": {
            "profileName": "kregitmhun",
            "fid": "254808"
          },
          "frame": {
            "buttons": [
              {
                "action": "",
                "id": "0x85bffc4de32c70688855f31a868181613fe4182890c4d35224a0afa311cfc6df-1",
                "index": 1,
                "target": ""
              }
            ],
            "castedAtTimestamp": "2024-02-27T19:07:24Z",
            "frameUrl": "https://api.anky.lat/farcaster-frames/newen-tldr",
            "imageAspectRatio": "",
            "imageUrl": "https://jpfraneto.github.io/images/newen2.png",
            "inputText": "",
            "postUrl": "http://api.anky.lat/farcaster-frames/newen-tldr?page=1",
            "state": ""
          }
        },
        // Other Frames casted since Jan 1, 2024
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All The Frames Casted By Certain User

You can fetch all the frames casted by a certain Farcaster user by using the [`FarcasterCasts`](../../api-references/api-reference/farcastercasts-api.md) API and specify the caster's fname or fid in the `castedBy` filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/gAThlrVUVT" %}
Show me all Farcaster Frames casted by Farcaster user betashop.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        castedBy: {_eq: "fc_fname:betashop.eth"},
        hasFrames: {_eq: true}
      },
      blockchain: ALL
    }
  ) {
    Cast {
      castedBy {
        profileName
        fid: userId
      }
      frame {
        buttons {
          action
          id
          index
          target
        }
        castedAtTimestamp
        frameUrl
        imageAspectRatio
        imageUrl
        inputText
        postUrl
        state
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
          "castedBy": {
            "profileName": "betashop.eth",
            "fid": "602"
          },
          "frame": {
            "buttons": [
              {
                "action": "link",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-2",
                "index": 2,
                "target": "https://warpcast.com/~/compose?embeds%5B%5D=https%3A%2F%2Fframes.airstack.xyz%2Ftt&text=hihi%2C+see+what+tokens+are+trending+right+now+on+Farcaster"
              },
              {
                "action": "post",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-4",
                "index": 4,
                "target": ""
              },
              {
                "action": "link",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-3",
                "index": 3,
                "target": "https://www.airstack.xyz/leaderboard"
              },
              {
                "action": "post",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-1",
                "index": 1,
                "target": "https://frames.airstack.xyz/tt/frame?a=1&b=4&c=1&t=4&r=2&f=1&info=1"
              }
            ],
            "castedAtTimestamp": "2024-03-30T14:21:30Z",
            "frameUrl": "https://frames.airstack.xyz/tt",
            "imageAspectRatio": "1.91:1",
            "imageUrl": "https://frames.airstack.xyz/tt/image?a=1&b=4&c=1&t=4&r=2",
            "inputText": "",
            "postUrl": "https://frames.airstack.xyz/tt/frame?a=1&b=4&c=1&t=4&r=2",
            "state": ""
          }
        },
        // Other Frames casted by betashop.eth
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All the Casts That Contains Certain Frame

You can fetch all the casts that contains a given Frame by using the [`FarcasterCasts`](../../api-references/api-reference/farcastercasts-api.md) API and specify the Farcaster Frame's URL in the `frameUrl` filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/WH5eGWXYOn" %}
Show me all the casts that contains Frame URL https://frames.airstack.xyz/tt
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        frameUrl: {_eq: "https://frames.airstack.xyz/tt"}
      },
      blockchain: ALL
    }
  ) {
    Cast {
      castedBy {
        profileName
        fid: userId
      }
      text
      embeds
      mentions {
        fid
        position
      }
      numberOfLikes
      numberOfRecasts
      numberOfReplies
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
          "castedBy": {
            "profileName": "echovenus22",
            "fid": "260925"
          },
          "text": "hihi, see what tokens are trending right now on Farcaster 👑\n\nLet'sgooo joined to get points ✨",
          "embeds": [
            {
              "url": "https://frames.airstack.xyz/tt"
            }
          ],
          "mentions": [],
          "numberOfLikes": 0,
          "numberOfRecasts": 0,
          "numberOfReplies": 0
        },
        // Other casts by other users
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Details Of A Certain Farcaster Frames

You can fetch all the details of a given Frame by using the [`FarcasterCasts`](../../api-references/api-reference/farcastercasts-api.md) API and specify the Farcaster Frame's URL in the `frameUrl` filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/cMnRO6vQ0g" %}
Show me Frame details of Frame URL https://frames.airstack.xyz/tt
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterCasts(
    input: {
      filter: {
        frameUrl: {_eq: "https://frames.airstack.xyz/tt"}
      },
      blockchain: ALL,
      limit: 1
    }
  ) {
    Cast {
      frame {
        buttons {
          action
          id
          index
          target
        }
        frameUrl
        imageAspectRatio
        imageUrl
        inputText
        postUrl
        state
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
          "frame": {
            "buttons": [
              {
                "action": "link",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-2",
                "index": 2,
                "target": "https://warpcast.com/~/compose?embeds%5B%5D=https%3A%2F%2Fframes.airstack.xyz%2Ftt&text=hihi%2C+see+what+tokens+are+trending+right+now+on+Farcaster"
              },
              {
                "action": "post",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-4",
                "index": 4,
                "target": ""
              },
              {
                "action": "post",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-1",
                "index": 1,
                "target": "https://frames.airstack.xyz/tt/frame?a=1&b=4&c=1&t=4&r=2&f=1&info=1"
              },
              {
                "action": "link",
                "id": "0x7ace9add807d8f0b393e50a20ac64d620d2faa0d766b06187a0f036978cb5dd5-3",
                "index": 3,
                "target": "https://www.airstack.xyz/leaderboard"
              }
            ],
            "frameUrl": "https://frames.airstack.xyz/tt",
            "imageAspectRatio": "1.91:1",
            "imageUrl": "https://frames.airstack.xyz/tt/image?a=1&b=4&c=1&t=4&r=2",
            "inputText": "",
            "postUrl": "https://frames.airstack.xyz/tt/frame?a=1&b=4&c=1&t=4&r=2",
            "state": ""
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Replies From All Casts That Contains Certain Farcaster Frames

You can use [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) API to fetch all casts that contains certain Frames by specifying the Frames URL to the `frameUrl` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qkoyo0IqHU" %}
Show me all replies from all casts that contains Frames https://gallery.manifold.xyz/venicebreeze
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReactions(
    input: {
      filter: {
        criteria: recasted,
        frameUrl: {_eq: "https://gallery.manifold.xyz/venicebreeze"}
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
            "castedAtTimestamp": "2024-05-06T17:32:10Z",
            "embeds": [
              {
                "url": "https://cdn.botfrens.com/contents/2731379/web_md.jpg"
              },
              {
                "url": "https://gallery.manifold.xyz/venicebreeze"
              }
            ],
            "url": "https://warpcast.com/carlos28355/0xf10f3c56",
            "text": "☀️ Venice Breeze 📻\r\n1/1\r\nMinted & listed with Manifold  ",
            "numberOfRecasts": 4,
            "numberOfLikes": 16,
            "channel": {
              "channelId": "cryptoart"
            },
            "mentions": []
          }
        },
        // other replies
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Users That Recasts Any Casts That Contains A Certain Farcaster Frames

You can use [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) API to fetch all users that recasts all casts that contains a certain Frames by providing the Frames URL to the `frameUrl` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/LfdnhIG0Tj" %}
Show me all users that recasts any casts with Frames URL https://gallery.manifold.xyz/venicebreeze
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReactions(
    input: {
      filter: {
        criteria: recasted,
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
        // Other users that recasts
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

## Developer Support

If you have any questions or need help regarding fetching Farcaster Frames data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Farcaster Cast Guides](farcaster-casts.md)
* [FarcasterCasts API Reference](../../api-references/api-reference/farcastercasts-api.md)
