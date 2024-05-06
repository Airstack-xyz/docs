---
description: >-
  Learn how to use the Airstack API to fetch Farcaster replies either from a
  certain user or a specific cast hash.
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

# 🔂 Farcaster Replies

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Get All Replies By A Farcaster User](farcaster-replies.md#get-all-replies-by-a-farcaster-user)
* [Get All Replies Of A Certain Cast By Cast Hash](farcaster-replies.md#get-all-replies-of-a-certain-cast-by-cast-hash)
* [Get All Replies From A Cast Casted By A Farcaster User](farcaster-replies.md#get-all-replies-from-a-cast-casted-by-a-farcaster-user)
* [Get All Replies From All Casts That Contains Certain Farcaster Frames](farcaster-replies.md#get-all-replies-from-all-casts-that-contains-certain-farcaster-frames)
* [Check If A User Replied A Certain Cast](farcaster-replies.md#check-if-a-user-replied-a-certain-cast)
* [Check If User A Replied Any Cast By User B](farcaster-replies.md#check-if-user-a-replied-any-cast-by-user-b)

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

## Get All Replies By A Farcaster User

You can use [`FarcasterReplies`](../../api-references/api-reference/farcasterreplies-api.md) API to fetch all the replies casted by a given Farcaster user by specifying the [user's identity ](../../api-references/api-reference/airstack-identity-api.md)in `repliedBy` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ALDMFod7gl" %}
Show me all the replies casted by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterReplies(
    input: {
      filter: {
<strong>        repliedBy: {_eq: "fc_fid:602"} # specify FID 602
</strong>      },
      blockchain: ALL,
      limit: 200
    }
  ) {
    Reply {
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
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterReplies": {
      "Reply": [
        {
          "castedAtTimestamp": "2024-05-06T16:30:01Z",
          "embeds": [],
          "url": "https://warpcast.com/betashop.eth/0xde71bb21",
          "text": "more thinking on this \n\nPreferred widgets/Actions at user and channel level \n\nallow more shopify-type experiences to emerge— always on my page \n\nAllows meme coins and nft projects to always have perm buy and mint actions \n\nAllows users to easily play favorite games together",
          "numberOfRecasts": 0,
          "numberOfLikes": 0,
          "channel": null,
          "mentions": []
        },
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Replies Of A Certain Cast By Cast Hash

You can use [`FarcasterReplies`](../../api-references/api-reference/farcasterreplies-api.md)  API to fetch all replies of a given cast by specifying the cast hash in `parentHash` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/GCDX7OzxhN" %}
Show me all the replies for cast with hash 0x4c0f2172836b14a57d35f5d096ced49049b1d92d
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReplies(
    input: {
      filter: {
        parentHash: {_eq: "0x4c0f2172836b14a57d35f5d096ced49049b1d92d"}
      },
      blockchain: ALL,
      limit: 200
    }
  ) {
    Reply {
      fid
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
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterReplies": {
      "Reply": [
        {
          "fid": "274034",
          "castedAtTimestamp": "2024-05-06T14:25:37Z",
          "embeds": [],
          "url": "https://warpcast.com/spd/0x3d913531",
          "text": "Minted",
          "numberOfRecasts": 0,
          "numberOfLikes": 0,
          "channel": {
            "channelId": "farcards"
          },
          "mentions": []
        },
        // Other replies for cast hash 0x4c0f2172836b14a57d35f5d096ced49049b1d92d
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Replies From A Cast Casted By A Farcaster User

You can use [`FarcasterReplies`](../../api-references/api-reference/farcasterreplies-api.md) API to fetch all the replies from any casts casted by a given Farcaster user by specifying the [user's identity ](../../api-references/api-reference/airstack-identity-api.md)in `parentCastedBy` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/xBFdMBH49c" %}
Show me all the replies for all casts casted by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReplies(
    input: {
      filter: {
        parentCastedBy: {_eq: "fc_fid:602"}
      },
      blockchain: ALL,
      limit: 200
    }
  ) {
    Reply {
      fid
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
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterReplies": {
      "Reply": [
        {
          "fid": "8439",
          "castedAtTimestamp": "2024-05-06T12:58:56Z",
          "embeds": [],
          "url": "https://warpcast.com/sreenath/0x9f281b82",
          "text": "💯",
          "numberOfRecasts": 0,
          "numberOfLikes": 0,
          "channel": null,
          "mentions": []
        },
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
        criteria: replied,
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
{{
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
            "numberOfRecasts": 5,
            "numberOfLikes": 17,
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

## Check If A User Replied A Certain Cast

You can use the [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) API to check if a user replied to a certain cast by providing the user's identity in `reactedBy` and the cast hash in `castHash` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/aC01eSxJir" %}
Check if FID 602 replied on cast hash 0xfb41e8ada70ee2da9a21023ec77469a03c4bcea9
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterReactions(
    input: {
      filter: {
        criteria: replied,
        reactedBy: {_eq: "fc_fid:602"},
        castHash: {_eq: "0xfb41e8ada70ee2da9a21023ec77469a03c4bcea9"}
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
          // if not null, then user replied to this cast
<strong>          "castHash": "0xfb41e8ada70ee2da9a21023ec77469a03c4bcea9"
</strong>        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If User A Replied Any Cast By User B

You can use [`FarcasterReplies`](../../api-references/api-reference/farcasterreplies-api.md) API to check if user A replied to any cast by user B:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Ca0LVe42H9" %}
Check if FID 2602 have ever replied to any cast by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterReplies(
    input: {
      filter: {
<strong>        repliedBy: {_eq: "fc_fid:2602"}, # user A
</strong><strong>        parentCastedBy: {_eq: "fc_fid:602"} # user B
</strong>      },
      blockchain: ALL,
      limit: 1
    }
  ) {
    Reply {
      castedAtTimestamp
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterReplies": {
      "Reply": [
        {
          // if not null, then user A replied to a cast by user B
<strong>          "castedAtTimestamp": "2024-05-05T20:48:08Z"
</strong>        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster replies data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarcasterReplies API Reference](../../api-references/api-reference/farcasterreplies-api.md)
* [FarcasterReactions API Reference](../../api-references/api-reference/farcasterreactions-api.md)
* [Farcaster Frames Guides](farcaster-frames.md)
