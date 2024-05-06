---
description: >-
  Learn how to use the Airstack API to fetch Farcaster recasts and quoted
  recasts either from a certain user or a specific cast hash.
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

# 🌀 Farcaster Recasts

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Get All Recasts By A Farcaster User](farcaster-recasts.md#get-all-recasts-by-a-farcaster-user)
* [Get All Quoted Recasts By A Farcaster User](farcaster-recasts.md#get-all-quoted-recasts-by-a-farcaster-user)
* [Get All Users That Recasts A Certain Cast](farcaster-recasts.md#get-all-users-that-recasts-a-certain-cast)
* [Get All Users That Recasts Any Casts That Contains A Certain Farcaster Frames](farcaster-recasts.md#get-all-users-that-recasts-any-casts-that-contains-a-certain-farcaster-frames)
* [Get All Quoted Recasts From A Cast Casted By A Farcaster User](farcaster-recasts.md#get-all-quoted-recasts-from-a-cast-casted-by-a-farcaster-user)
* [Check If A User Recasted A Certain Cast By Cast Hash](farcaster-recasts.md#check-if-a-user-recasted-a-certain-cast-by-cast-hash)
* [Check If User A Quote Recasted A Cast By User B](farcaster-recasts.md#check-if-user-a-quote-recasted-a-cast-by-user-b)

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

## Get All Recasts By A Farcaster User

You can use [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) API to fetch all recasts by a Farcaster user by specifying the user's [identity](../../api-references/api-reference/airstack-identity-api.md) in `reactedBy` input filter:

{% hint style="info" %}
If you are looking for quoted recasts, check [this query](farcaster-recasts.md#get-all-quoted-recasts-by-a-farcaster-user) instead.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/zmE0fdoa2A" %}
Show me all recasts by FID 602
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
            "castedAtTimestamp": "2024-05-06T00:03:43Z",
            "embeds": [
              {
                "url": "https://farcaster.network"
              }
            ],
            "url": "https://warpcast.com/greg/0xe98b22bf",
            "text": "https://farcaster.network is back!\n\n@zachterrell and I originally built it in 2022, but it's slowly crumbled over time as Farcaster grew from a few hundred users to tens of thousands\n\nDuring Farcon, we finally took the time to rebuild it from the ground up and are excited to be working on it again",
            "numberOfRecasts": 46,
            "numberOfLikes": 254,
            "channel": {
              "channelId": "farhack"
            },
            "mentions": [
              {
                "fid": "457",
                "position": 36
              }
            ]
          }
        },
        // other recasts by FID 602
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Quoted Recasts By A Farcaster User

You can use [`FarcasterQuotedRecasts`](../../api-references/api-reference/farcasterquotedrecasts-api.md) API to fetch all quoted recasts by a Farcaster user by specifying the user's [identity](../../api-references/api-reference/airstack-identity-api.md) in `recastedBy` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/kMOw3IbrIp" %}
Show me all quoted recasts by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterQuotedRecasts(
    input: {
      filter: {
        recastedBy: {_eq: "fc_fid:602"}
      },
      blockchain: ALL,
      limit: 200
    }
  ) {
    QuotedRecast {
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
    "FarcasterQuotedRecasts": {
      "QuotedRecast": [
        {
          "castedAtTimestamp": "2024-05-06T15:40:23Z",
          "embeds": [
            {
              "castId": {
                "fid": 602,
                "hash": "0x08796828d8bdda14fd1acbed1472c0feab1452e7"
              }
            }
          ],
          "url": "https://warpcast.com/betashop.eth/0x6da72057",
          "text": "would also be cool to have some Actions at the User level. \n\nFor example: \n- Top casts\n- Top followers (most engaged)\n- check NOTA earnings\n- check Degen\n- NFTs in common\netc\n\n@dwr.eth @v @horsefacts.eth",
          "numberOfRecasts": 2,
          "numberOfLikes": 41,
          "channel": null,
          "mentions": [
            {
              "fid": "3",
              "position": 176
            },
            {
              "fid": "2",
              "position": 177
            },
            {
              "fid": "3621",
              "position": 178
            }
          ]
        },
        // Other quote recasts by FID 602
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Users That Recasts A Certain Cast

You can use [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) API to get all the users that recasted a specific cast hash by providing the cast hash into `castHash` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/498pvpweu7" %}
Show me all users that recasts cast hash 0x4d5f904518bb9e8368eb560d1b93c762f7267cb4
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
        castHash: {_eq: "0x4d5f904518bb9e8368eb560d1b93c762f7267cb4"}
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
            "profileName": "betashop.eth",
            "fid": "602"
          }
        }
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

## Get All Quoted Recasts From A Cast Casted By A Farcaster User

You can use [`FarcasterQuotedRecasts`](../../api-references/api-reference/farcasterquotedrecasts-api.md) API to get all quoted recasts of any casts by a specified Farcaster user by providing the Farcaster user's identity to the `parentCastedBy` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4KgQG6X2B8" %}
Show me all quoted recasts from any casts by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterQuotedRecasts(
    input: {
      filter: {
        parentCastedBy: {_eq: "fc_fid:602"}
      },
      blockchain: ALL
    }
  ) {
    QuotedRecast {
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
    "FarcasterQuotedRecasts": {
      "QuotedRecast": [
        {
          "castedAtTimestamp": "2024-05-06T16:49:14Z",
          "embeds": [
            {
              "castId": {
                "fid": 602,
                "hash": "0xd65fda9c0db0d4cd82bbb10de8de6b43a9a0c9c5"
              }
            }
          ],
          "url": "https://warpcast.com/leovido.eth/0x4d5f9045",
          "text": "I use @airstack frames analytics dashboard practically EVERYDAY\n\nThe insights help me work on key areas of the app and track actual usage and engagement.\n\nA MUST, a super PRO tool, for frames and cast action devs 🚀",
          "numberOfRecasts": 1,
          "numberOfLikes": 3,
          "channel": {
            "channelId": "frames-devs"
          },
          "mentions": [
            {
              "fid": "20909",
              "position": 6
            }
          ]
        },
        // Other quoted recasts
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If A User Recasted A Certain Cast By Cast Hash

You can use [`FarcasterReactions`](../../api-references/api-reference/farcasterreactions-api.md) API to check if a user recasted a certain cast by providing the cast hash into `castHash` and the user's identity to the `reactedBy` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/eMEN6UZP7J" %}
Check If FID 602 recasted cast hash 0x4d5f904518bb9e8368eb560d1b93c762f7267cb4
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
        castHash: {_eq: "0x4d5f904518bb9e8368eb560d1b93c762f7267cb4"},
        reactedBy: {_eq: "fc_fid:602"}
      },
      blockchain: ALL
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
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterReactions": {
      "Reaction": [
        // if not null, then user recasted the cast
<strong>        {
</strong>          "cast": {
            "castedAtTimestamp": "2024-05-06T16:49:14Z",
            "embeds": [
              {
                "castId": {
                  "fid": 602,
                  "hash": "0xd65fda9c0db0d4cd82bbb10de8de6b43a9a0c9c5"
                }
              }
            ],
            "url": "https://warpcast.com/leovido.eth/0x4d5f9045",
            "text": "I use @airstack frames analytics dashboard practically EVERYDAY\n\nThe insights help me work on key areas of the app and track actual usage and engagement.\n\nA MUST, a super PRO tool, for frames and cast action devs 🚀",
            "numberOfRecasts": 1,
            "numberOfLikes": 3,
            "channel": {
              "channelId": "frames-devs"
            },
            "mentions": [
              {
                "fid": "20909",
                "position": 6
              }
            ]
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If User A Quote Recasted A Cast By User B

You can [`FarcasterQuotedRecasts`](../../api-references/api-reference/farcasterquotedrecasts-api.md) API to check if user A quote recasted any cast by user B:

### Try Demo

{% embed url="https://app.airstack.xyz/query/viiYQqeHVp" %}
Check If FID 2602 ever quoted recasts any of FID 602's cast
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterQuotedRecasts(
    input: {
      filter: {
<strong>        parentCastedBy: {_eq: "fc_fid:602"}, # user B
</strong><strong>        recastedBy: {_eq: "fc_fid:2602"} # user A
</strong>      },
      blockchain: ALL,
      limit: 1
    }
  ) {
    QuotedRecast {
      castedAtTimestamp
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterQuotedRecasts": {
      "QuotedRecast": [
        {
          // if not null, then user A quoted recasts FID 602's cast
<strong>          "castedAtTimestamp": "2024-05-06T15:24:18Z"
</strong>        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster recasts data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarcasterQuotedRecasts API](../../api-references/api-reference/farcasterquotedrecasts-api.md)
* [FarcasterReactions API](../../api-references/api-reference/farcasterreactions-api.md)
* [Farcaster Frames Guides](farcaster-frames.md)
