---
description: >-
  Learn how to use Airstack to determine a user's high probability connection
  with another user to classify spammers on your XMTP messaging app.
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

# 🕸 High Probability of Connection

## 🕸 High Probability of Connection

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [XMTP](https://xmtp.org) applications and integrating on-chain and off-chain data with [XMTP](https://xmtp.org).

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to check high probability of connection by:

* [Common POAP Events Attended](high-probability-of-connection.md#common-poap-events-attended)
* [Common Followers on Lens or Farcaster](high-probability-of-connection.md#common-followers-on-lens-or-farcaster)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [XMTP](https://xmtp.org)

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

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

### Best Practice

While choosing a specific criteria will significantly decrease the number of spam appearing on your user's XMTP inbox, It is best practice that you **combine** the multiple criterion given here to build your **spam filter**.

This is done to provide **multiple layers of filtration** that will make it nearly impossible for spammers to have their messages slide into your users' XMTP inbox.

### Common POAP Events Attended

You can establish a high probability of connection if both users have attended the same POAP event(s) previously:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user A attended the same PAOP events as user B is **NOT** a foolproof method to remove spammers from your user's inbox.

Thus, be sure to combine multiple criterion to provide multiple layer of filtration to prove if that the user is has a genuinely high probability of connection.
{% endhint %}

#### Try Demo

{% embed url="https://app.airstack.xyz/query/GiJgN06hW9" %}
show me all common poaps hold by both betashop.eth and ipeciura.eth
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query CommonPoaps {
  Poaps(
    input: {
      filter: { owner: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 50
    }
  ) {
    Poap {
      poapEvent {
        poaps(input: { filter: { owner: { _eq: "ipeciura.eth" } } }) {
          eventId
          mintHash
          mintOrder
          poapEvent {
            eventName
            eventURL
            contentValue {
              image {
                extraSmall
                small
                original
                medium
                large
              }
            }
            isVirtualEvent
            city
          }
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
    "Poaps": {
      "Poap": [
        {
          "poapEvent": {
            "poaps": [ 
              {
<strong>                "eventId": "141662", // This POAP hold by both betashop.eth and ipeciura.eth
</strong>                "mintHash": "0x959c6119c98af3d2e4b0616d8cdb10f2d161fc917613c0ca7f41234ed6892f19",
                "mintOrder": 117,
                "poapEvent": {
                  "eventName": "You've met Lucas in Summer 23",
                  "eventURL": "https://poap.studio",
                  "contentValue": {
                    "image": {
                      "extraSmall": "https://assets.airstack.xyz/image/poap/gqIGlLldTGyeR+h4MXK90Q==/extra_small.gif",
                      "small": "https://assets.airstack.xyz/image/poap/gqIGlLldTGyeR+h4MXK90Q==/small.gif",
                      "original": "https://assets.airstack.xyz/image/poap/gqIGlLldTGyeR+h4MXK90Q==/original_image.gif",
                      "medium": "https://assets.airstack.xyz/image/poap/gqIGlLldTGyeR+h4MXK90Q==/medium.gif",
                      "large": "https://assets.airstack.xyz/image/poap/gqIGlLldTGyeR+h4MXK90Q==/large.gif"
                    }
                  },
                  "isVirtualEvent": false,
                  "city": "Summer 23"
                }
              }
            ]
          }
        },
        {
          "poapEvent": {
<strong>            "poaps": [] // The POAP is hold by betashop.eth, but not ipeciura.eth
</strong>          }
        },
        // more POAP events
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

### Common Followers on Lens or Farcaster

You can establish a high probability of connection if both users have common followers on either Lens or Farcaster:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user A has any common followers with user B is **NOT** a foolproof method to remove spammers from your user's inbox.

Thus, be sure to combine multiple criterion to provide multiple layer of filtration to prove if that the user is has a genuinely high probability of connection.
{% endhint %}

#### Try Demo

{% embed url="https://app.airstack.xyz/query/Ngh4xIAcYL" %}
show me all common followers of betashop.eth and ipeciura.eth
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query CommonFollowers {
  SocialFollowers(
    input: {
      filter: { identity: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 50
    }
  ) {
    Follower {
      followerAddress {
        socialFollowers(
          input: { filter: { identity: { _eq: "ipeciura.eth" } } }
        ) {
          Follower {
            followerAddress {
              addresses
              domains {
                name
              }
              socials {
                profileName
                profileTokenId
                profileTokenIdHex
                userId
                userAssociatedAddresses
              }
            }
          }
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
          "followerAddress": {
            "socialFollowers": {
              "Follower": [
                {
<strong>                  "followerAddress": { // followers of both betashop.eth and ipeciura.eth
</strong>                    "addresses": [
                      "0x44f9047ec33dd3682df5e9178c492272a5f7afd0",
                      "0x71167f1794c90510671f3d122207696709ef4417",
                      "0x8c34ae28c1bb84785e4c059c301842b7be295a01",
                      "0x3570958b8dcbc4f663f508efcedb454ee9af9516"
                    ],
                    "domains": [
                      {
                        "name": "panjab.eth"
                      },
                      {
                        "name": "prabh.eth"
                      },
                      {
                        "name": "kapurthala.eth"
                      },
                      {
                        "name": "gurdaspur.eth"
                      },
                      {
                        "name": "solostakes.eth"
                      },
                      {
                        "name": "antilibrary.eth"
                      },
                      {
                        "name": "ਣਣਣ.eth"
                      },
                      {
                        "name": "ethereumfellowship.eth"
                      },
                      {
                        "name": "jynmrh.eth"
                      },
                      {
                        "name": "jugaad.eth"
                      }
                    ],
                    "socials": [
                      {
                        "profileName": "prabh.eth",
                        "profileTokenId": "11946",
                        "profileTokenIdHex": "0x2eaa",
                        "userId": "11946",
                        "userAssociatedAddresses": [
                          "0x44f9047ec33dd3682df5e9178c492272a5f7afd0",
                          "0x71167f1794c90510671f3d122207696709ef4417",
                          "0x8c34ae28c1bb84785e4c059c301842b7be295a01",
                          "0x3570958b8dcbc4f663f508efcedb454ee9af9516"
                        ]
                      },
                      {
                        "profileName": "lens/@retroflex",
                        "profileTokenId": "106741",
                        "profileTokenIdHex": "0x01a0f5",
                        "userId": "0x44f9047ec33dd3682df5e9178c492272a5f7afd0",
                        "userAssociatedAddresses": [
                          "0x44f9047ec33dd3682df5e9178c492272a5f7afd0"
                        ]
                      }
                    ]
                  }
                }
              ]
            }
          }
        },
        {
          "followerAddress": {
            "socialFollowers": {
<strong>              "Follower": null // followers of betashop.eth, but not followed by ipeciura.eth
</strong>            }
          }
        },
        // more followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding determine a user's high probability connection with another user to classify spammers on your XMTP messaging app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Poaps API Reference](../../../api-references/api-reference/poaps-api/)
* [SocialFollowers API Reference](../../../api-references/api-reference/socialfollowers-api.md)
* [Known Senders](known-senders.md)
* [Proof of Personhood](proof-of-personhood.md)
