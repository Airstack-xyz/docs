---
description: >-
  Learn how to fetch Farcaster power badge data and its various use cases to
  check if a user is a power user on Farcaster.
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

# 🎖️ Farcaster Power Badge

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Check If Farcaster User(s) Have A Power Badge](farcaster-power-badge.md#check-if-farcaster-user-s-have-a-power-badge)
* [Get All Farcaster Followers That Also Have A Power Badge](farcaster-power-badge.md#get-all-farcaster-followers-that-also-have-a-power-badge)
* [Get All Farcaster Followings That Also Have A Power Badge](farcaster-power-badge.md#get-all-farcaster-followings-that-also-have-a-power-badge)
* [Get All Farcaster Channel Participants That Also Have A Power Badge](farcaster-power-badge.md#get-all-farcaster-channel-participants-that-also-have-a-power-badge)

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

## Check If Farcaster User(s) Have A Power Badge

You can check if Farcaster user(s) have a power badge by using the [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/beRwZJjqTO" %}
Check if Farcaster user(s) have power badge
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        profileName: {
<strong>          _in: ["betashop.eth"] # provide FC user profile name(s)
</strong>        },
        dappName: {_eq: farcaster}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      isFarcasterPowerUser
      profileName
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
<strong>          "isFarcasterPowerUser": true, // this user has power badge
</strong>          "profileName": "betashop.eth"
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get All Farcaster Followers That Also Have A Power Badge

You can fetch all Farcaster followers of a Farcaster user and check if they have a power badge by using the [`SocialFollowers`](../../api-references/api-reference/socialfollowers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/KSLPysB3cb" %}
Show me all the followers of betashop.eth that also have power badge
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: {_eq: "betashop.eth"},
        dappName: {_eq: farcaster}
      },
      blockchain: ALL,
      order: {followerSince: DESC}
    }
  ) {
    Follower {
      followerProfileId
      followerAddress {
        addresses
        socials {
          profileName
          isFarcasterPowerUser
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
    "SocialFollowers": {
      "Follower": [
        {
          "followerProfileId": "411529",
          "followerAddress": {
            "addresses": [
              "0xbc0b2add51010730563159a84d613eddc7d21e4c"
            ],
            "socials": [
              {
                "profileName": "defi-degen",
<strong>                "isFarcasterPowerUser": false // This user have no power badge
</strong>              }
            ]
          }
        },
        // other followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get All Farcaster Followings That Also Have A Power Badge

You can fetch all Farcaster followings of a Farcaster user and check if they have a power badge by using the [`SocialFollowings`](../../api-references/api-reference/socialfollowings-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/GqrYTU8wKy" %}
Show me all the followings of betashop.eth that also have power badge
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: {_eq: "betashop.eth"},
        dappName: {_eq: farcaster}
      },
      blockchain: ALL,
      order: {followingSince: DESC}
    }
  ) {
    Following {
      followingProfileId
      followingAddress {
        addresses
        socials {
          profileName
          isFarcasterPowerUser
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
    "SocialFollowings": {
      "Following": [
        {
          "followingProfileId": "378310",
          "followingAddress": {
            "addresses": [
              "0x765f844900da37dbc6f3c82e0ccdea5e01a303dc",
              "2TkvxGAuEXWFeYB7qP2MY2MSjhmcsnsCotyxu7fQyKH3"
            ],
            "socials": [
              {
                "profileName": "0xbuildr",
<strong>                "isFarcasterPowerUser": false // This user have no power badge
</strong>              }
            ]
          }
        },
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get All Farcaster Channel Participants That Also Have A Power Badge

You can fetch all the channel participants of a Farcaster channel and check if they have a power badge by using the [`FarcasterChannelParticipants`](../../api-references/api-reference/farcasterchannelparticipants-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/JfdKvxqd8w" %}
Show me all participants of /airstack channel that also have power badge
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterChannelParticipants(
    input: {
      filter: {
        channelId: {_eq: "airstack"}
      },
      blockchain: ALL
    }
  ) {
    FarcasterChannelParticipant {
      participantId
      participant {
        profileName
        isFarcasterPowerUser
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterChannelParticipants": {
      "FarcasterChannelParticipant": [
        {
          "participantId": "247143",
          "participant": {
            "profileName": "kylepatrick",
<strong>            "isFarcasterPowerUser": true // This user have power badge
</strong>          }
        },
        {
          "participantId": "5701",
          "participant": {
            "profileName": "chriscocreated",
<strong>            "isFarcasterPowerUser": false // This user have no power badge
</strong>          }
        },
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster power badge data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Socials API Reference](../../api-references/api-reference/socials-api.md)
* [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
* [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
* [FarcasterChannelParticipants API Reference](../../api-references/api-reference/farcasterchannels-api.md)
