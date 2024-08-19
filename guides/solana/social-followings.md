---
description: >-
  Learn how fetch the social followings of a solana address across Farcaster and
  Lens.
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

# 💐 Social Followings

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get All Followings of Solana Address](social-followings.md#get-all-followings-of-solana-address)
* [Check If Solana Address A Is Following Solana Address B](social-followings.md#check-if-solana-address-a-is-following-solana-address-b)

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

## Get All Followings of Solana Address

You can get the list of Farcaster and Lens followings of a given solana address by using the [`SocialFollowings`](../../api-references/api-reference/socialfollowings-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qXk8Jj8OQh" %}
Show me all the social followings of GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV across Farcaster and Lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: { _eq: "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV" }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        socials {
          dappName
          profileName
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
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "phabc"
              }
            ]
          }
        },
        {
          "followingAddress": {
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "guapolocal.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@guapolocal"
              }
            ]
          }
        },
        {
          "followingAddress": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@0xamir"
              },
              {
                "dappName": "farcaster",
                "profileName": "ir"
              }
            ]
          }
        },
        {
          "followingAddress": {
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "crick"
              }
            ]
          }
        }
        // more followings on Farcaster and Lens
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If Solana Address A Is Following Solana Address B

You can check if solana address A is following solana address B by using the [`Wallet`](../../api-references/api-reference/wallet-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/SXig7s5K6B" %}
Show if solana address A is following solana address B
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query isFollowing {
  Wallet(
    input: {
      # Top-level is Solana address B
<strong>      identity: "HyrNmmmce9W3rDdTQcZHyYvhuxPN6AaY3mVJcS9f4AZw",
</strong>      blockchain: ethereum
    }
  ) {
    socialFollowings( 
      input: {
        filter: {
          identity: {
            # Here is Solana address A
<strong>            _eq: "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV"
</strong>          }
        }
      }
    ) {
      Following {
        dappName
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Wallet": {
      "socialFollowings": {
        "Following": [
          {
            // Solana address A is following solana address B on Farcaster
<strong>            "dappName": "farcaster"
</strong>          }
        ]
      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching social followings of a solana address, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Social Follows Guides](../social-follows/)
* [Resolving Solana Addresses](resolve-identities.md)
* [Social Followers Of Solana Addresses](social-followers.md)
* [Token Balances Of Solana Addresses](token-balances.md)
* [Check XMTP For Solana Addresses](broken-reference)
* [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
