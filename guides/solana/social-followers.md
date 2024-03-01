---
description: >-
  Learn how fetch the social followers of a solana address across Farcaster and
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

# 🫂 Social Followers

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get All Followers of Solana Address](social-followers.md#get-all-followers-of-solana-address)
* [Check If Solana Address A Is Followed By Solana Address B](social-followers.md#check-if-solana-address-a-is-followed-by-solana-address-b)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
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

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get All Followers of Solana Address

You can get the list of Farcaster and Lens followers of a given solana address by using the [`SocialFollowers`](../../api-references/api-reference/socialfollowers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/E2h5mTEe8r" %}
Show me all the social followers of GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV across Farcaster and Lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: {
          _eq: "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV"
        }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
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
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "ghostcode"
              }
            ]
          }
        },
        {
          "followerAddress": {
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "jackyang36"
              }
            ]
          }
        },
        {
          "followerAddress": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@saxenasaheb"
              },
              {
                "dappName": "farcaster",
                "profileName": "saxenasaheb.eth"
              }
            ]
          }
        },
        // more followers on Farcaster and Lens
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If Solana Address A Is Followed By Solana Address B

You can check if solana address A is followed by solana address B by using the [`Wallet`](../../api-references/api-reference/wallet-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/KP6G2gx9zl" %}
Show me if solana address A is followed by solana address B
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query isFollowing {
  Wallet(
    input: {
      # Top-level is Solana address B
<strong>      identity: "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV",
</strong>      blockchain: ethereum
    }
  ) {
    socialFollowers( 
      input: {
        filter: {
          identity: {
            # Here is Solana address A
<strong>            _eq: "HyrNmmmce9W3rDdTQcZHyYvhuxPN6AaY3mVJcS9f4AZw"
</strong>          }
        }
      }
    ) {
      Follower {
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
      "socialFollowers": {
        "Follower": [
          {
            // Solana address A is followed by Solana address B on Farcaster
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

If you have any questions or need help regarding fetching social followers of a solana address, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Social Follows Guides](../social-follows/)
* [Resolving Solana Addresses](resolve-identities.md)
* [Token Balances Of Solana Addresses](token-balances.md)
* [Social Followings Of Solana Addresses](social-followings.md)
* [Check XMTP For Solana Addresses](../xmtp/check-multiple-users.md)
* [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
