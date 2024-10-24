---
description: >-
  Airstack Activate Kit enables you to execute a series of API calls to activate
  Farcaster users after they connect with Farcaster Auth Kit.
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

# 💡 Activate Kit for Farcaster Auth Kit

[Farcaster Auth Kit](https://docs.farcaster.xyz/auth-kit/introduction) enables your users to login to your app with their Farcaster custody address. Airstack Activate Kit is the next immediate step.

With Airstack Activate you can immediately fetch all of the user's:

- Registration details from Optimism and Hubs
- Profile details including bio, profile image
- Connected addresses
- Following
- Followers

You can also use Airstack regex search to enable auto-complete search for Farcaster users in your app.

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

- [Get Farcaster User Details](activate-kit-for-farcaster-auth-kit.md#get-started)
- [Get Farcaster User's Connected Addresses](activate-kit-for-farcaster-auth-kit.md#get-farcaster-users-connected-addresses)
- [Get Farcaster User's Followers and Following Counts](activate-kit-for-farcaster-auth-kit.md#get-farcaster-users-followers-and-following-counts)
- [Get Farcaster User's Followers](activate-kit-for-farcaster-auth-kit.md#get-farcaster-users-followers)
- [Get Farcaster User's Followings](activate-kit-for-farcaster-auth-kit.md#get-farcaster-users-followings)
- [Get All Farcaster Users Whose Names Start With Certain Terms (auto-complete)](activate-kit-for-farcaster-auth-kit.md#get-all-farcaster-users-whose-names-start-with-certain-terms-auto-complete)
- [Get All Farcaster Users Whose Names Contain Certain Terms (auto-complete)](activate-kit-for-farcaster-auth-kit.md#get-all-farcaster-users-whose-names-contain-certain-terms-auto-complete)

### Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL

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

## Get Farcaster User Details

You can fetch the Farcaster user details of a 0x address by using the [`Socials`](../../api-references/api-reference/socials-api.md) API and provide the 0x address to the `identity` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qnp0VuwM2K" %}
Show me Farcaster user details of Farcaster profile owned by 0x182327170fC284cAaA5b1bC3e3878233f529D741
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        identity: { _eq: "0x182327170fC284cAaA5b1bC3e3878233f529D741" }
        dappName: { _eq: farcaster }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      profileName
      fnames
      userId
      profileImage
      profileImageContentValue {
        image {
          medium
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
    "Socials": {
      "Social": [
        {
          "profileName": "v",
          "fnames": ["v", "varunsrin.eth"],
          "userId": "2",
          "profileImage": "https://i.seadn.io/gae/sYAr036bd0bRpj7OX6B-F-MqLGznVkK3--DSneL_BT5GX4NZJ3Zu91PgjpD9-xuVJtHq0qirJfPZeMKrahz8Us2Tj_X8qdNPYC-imqs?w=500&auto=format",
          "profileImageContentValue": {
            "image": {
              "medium": "https://assets.airstack.xyz/image/social/XCPJH5EP49qftYc7+wAFfv5jzo3ddBWc9FMEERWezG8=/medium.png"
            }
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster User's Connected Addresses

You can fetch the Farcaster profile's connected addresses of a 0x address by using the [`Socials`](../../api-references/api-reference/socials-api.md) API and provide the 0x address to the `identity` filter input and all the connected addresses will be returned in the `userAssociatedAddresses` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qnp0VuwM2K" %}
Show me all the connected addresses of Farcaster profile owned by 0x182327170fC284cAaA5b1bC3e3878233f529D741
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        identity: { _eq: "0x182327170fC284cAaA5b1bC3e3878233f529D741" }
        dappName: { _eq: farcaster }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      userAssociatedAddresses
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
          "userAssociatedAddresses": [
            "0x4114e33eb831858649ea3702e1c9a2db3f626446",
            "0x91031dcfdea024b4d51e775486111d2b2a715871",
            "0x182327170fc284caaa5b1bc3e3878233f529d741"
          ]
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster User's Followers and Following Counts

You can fetch the Farcaster profile's connected addresses of a 0x address by using the [`Socials`](../../api-references/api-reference/socials-api.md) API and provide the 0x address to the `identity` filter input.

The number of people following and people followed by the given Farcaster profile will be returned in the `followerCount` and `followingCount`, respectively:

### Try Demo

{% embed url="https://app.airstack.xyz/query/A12oKUzoWM" %}
Show me the number of followers and followings of 0x182327170fC284cAaA5b1bC3e3878233f529D741 on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        identity: { _eq: "0x182327170fC284cAaA5b1bC3e3878233f529D741" }
        dappName: { _eq: farcaster }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      followerCount
      followingCount
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
          "followerCount": 37149,
          "followingCount": 1000
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster User's Followers

You can fetch all the users following 0x address on Farcaster by using the [`SocialFollowers`](../../api-references/api-reference/socialfollowers-api.md) API and provide the 0x address to the `identity` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/phCfEbdh7x" %}
Show me Farcaster followers of 0x182327170fC284cAaA5b1bC3e3878233f529D741
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: { _eq: "0x182327170fC284cAaA5b1bC3e3878233f529D741" }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
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
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0xf86365d15b374a28c453396af7e10af24fae1b04"],
            "socials": [
              {
                "profileName": "aresangel",
                "userId": "214577"
              }
            ]
          }
        },
        {
          "followerAddress": {
            "addresses": ["0x9ad6bf4108b79057c0ccc70521c86feca540f24f"],
            "socials": [
              {
                "profileName": "tortula",
                "userId": "193061"
              }
            ]
          }
        }
        // More users following 0x182327170fC284cAaA5b1bC3e3878233f529D741 on Farcaster
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster User's Followings

You can fetch all the users being followed by 0x address on Farcaster by using the [`SocialFollowings`](../../api-references/api-reference/socialfollowings-api.md) API and provide the 0x address to the `identity` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/LI6HVgZGLT" %}
Show me Farcaster following of 0x182327170fC284cAaA5b1bC3e3878233f529D741
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: { _eq: "0x182327170fC284cAaA5b1bC3e3878233f529D741" }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
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
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x7244b3a5726f5f11d6fb9414ea98d8ce9f10c88a",
              "0x6ca011aae6d551a3efa533c40c24a810321c0384"
            ],
            "socials": [
              {
                "profileName": "adeets-22",
                "userId": "323"
              }
            ]
          }
        },
        {
          "followingAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ],
            "socials": [
              {
                "profileName": "betashop.eth",
                "userId": "602"
              }
            ]
          }
        }
        // More users followed on Farcaster by 0x182327170fC284cAaA5b1bC3e3878233f529D741
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Farcaster Users Whose Names Start With Certain Terms (auto-complete)

You can fetch all Farcaster users that starts with given words by providing the regex pattern `"^<given-words>"` to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/mXuUlbGPnI" %}
show me all Farcaster users starting with "a"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search all Farcaster users
        # starting with "a"
<strong>        profileName: {_regex: "^a"},
</strong>        dappName: {_eq: farcaster}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
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
          "dappName": "farcaster",
          "profileName": "atty"
        },
        {
          "dappName": "farcaster",
          "profileName": "anita-mpf"
        },
        {
          "dappName": "farcaster",
          "profileName": "amarraghu"
        }
        // Other Farcaster users starting with "a"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Farcaster Users Whose Names Contain Certain Terms (auto-complete)

You can fetch all Farcaster users that contains given words by providing `"<given-words>"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/yuolzNgcAR" %}
show me all Farcaster users containing with "abc"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search all Farcaster users
        # containing "abc"
<strong>        profileName: {_regex: "abc"},
</strong>        dappName: {_eq: farcaster}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
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
          "dappName": "farcaster",
          "profileName": "abcabc"
        },
        {
          "dappName": "farcaster",
          "profileName": "krabchinski"
        },
        {
          "dappName": "farcaster",
          "profileName": "861213abcc"
        }
        // Other Farcaster users containing with "abc"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding the activate kit for [Farcaster Auth Kit](https://docs.farcaster.xyz/auth-kit/introduction), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Search Farcaster Users](search-farcaster-users.md)
- [Farcaster Followers](farcaster-followers.md)
- [Farcaster Following](farcaster-following.md)
- [Socials API Reference](../../api-references/api-reference/socials-api.md)
- [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
- [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
