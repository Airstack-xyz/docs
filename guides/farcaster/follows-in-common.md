---
description: >-
  Learn how to fetch Farcaster followers and followings in common between
  multiple Farcaster users.
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

# 👭 Follows In Common

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and integrating on-chain and off-chain data with Farcaster.

In this tutorial, you will learn how to fetch Farcaster followers and following in common and its variation.

In this guide you will learn how to use Airstack to:

* [Common Farcaster Followers of Multiple Farcaster Users](follows-in-common.md#common-farcaster-followers-of-multiple-farcaster-users)
* [Common Farcaster Following of Multiple Farcaster Users](follows-in-common.md#common-farcaster-following-of-multiple-farcaster-users)
* [Farcaster Followers of Farcaster User X That Is Also Farcaster Following of Farcaster User Y](follows-in-common.md#farcaster-followers-of-farcaster-user-x-that-is-also-farcaster-following-of-farcaster-user-y)
* [Farcaster Following of Farcaster User X That Is Also Farcaster Followers of Farcaster User Y](follows-in-common.md#farcaster-following-of-farcaster-user-x-that-is-also-farcaster-follower-of-farcaster-user-y)
* [Mutual Follows of A Farcaster User](follows-in-common.md#mutual-farcaster-follows-of-a-farcaster-user)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Finished [Farcaster Followers](farcaster-followers.md) and [Farcaster Following](farcaster-following.md)

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
#### React

```sh
npm install @airstack/airstack-react
```

#### Node

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
#### React

```sh
yarn add @airstack/airstack-react
```

#### Node

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
#### React

```sh
pnpm install @airstack/airstack-react
```

#### Node

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack asyncio
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

## Common Farcaster Followers of Multiple Farcaster Users

You can get the list of common Farcaster followers of multiple Farcaster user by inputting [0x address](#user-content-fn-1)[^1], [ENS domain](#user-content-fn-2)[^2], [Farcaster Name](#user-content-fn-3)[^3], or [Farcaster ID](#user-content-fn-4)[^4]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/LggnwkvJ3K" %}
Show me common followers of both fc\_fname betashop.eth and fc\_fname ipeciura
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_eq: "fc_fname:betashop.eth"}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        socialFollowers(
          input: {filter: {identity: {_eq: "fc_fname:ipeciura"}, dappName: {_eq: farcaster}}, limit: 200}
        ) {
          Follower {
            followerAddress {
              socials(input: {filter: {dappName: {_eq: farcaster}}}) {
                fnames
                profileName
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
                  "followerAddress": {
                    "socials": [
                      {
                        "fnames": [
                          "prab.eth",
                          "prabh.eth",
                          "prab"
                        ],
<strong>                        "profileName": "prabh.eth", // Follow both betashop.eth and ipeciura
</strong>                        "userId": "11946",
                        "userAssociatedAddresses": [
                          "0x8c34ae28c1bb84785e4c059c301842b7be295a01",
                          "0x3570958b8dcbc4f663f508efcedb454ee9af9516",
                          "0x44f9047ec33dd3682df5e9178c492272a5f7afd0",
                          "0x71167f1794c90510671f3d122207696709ef4417"
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
<strong>              "Follower": [] // Follow betashop.eth, but doesn't follow ipeciura
</strong>            }
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Common Farcaster Following of Multiple Farcaster Users

You can get the list of common Farcaster following of multiple Farcaster user by inputting [0x address](#user-content-fn-5)[^5], [ENS domain](#user-content-fn-6)[^6], [Farcaster Name](#user-content-fn-7)[^7], or [Farcaster ID](#user-content-fn-8)[^8]:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/suXp8pV9TR" %}
Show me common following of both fc\_fname betashop.eth and fc\_fname ipeciura
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_eq: "fc_fname:betashop.eth"}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        socialFollowings(
          input: {filter: {identity: {_eq: "fc_fname:ipeciura"}, dappName: {_eq: farcaster}}, limit: 200}
        ) {
          Following {
            followingAddress {
              socials(input: {filter: {dappName: {_eq: farcaster}}}) {
                fnames
                profileName
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
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "socialFollowings": {
              "Following": [
                {
                  "followingAddress": {
                    "socials": [
                      {
                        "fnames": [
                          "nickcherry" 
                        ],
<strong>                        "profileName": "nickcherry", // is followed by betashop.eth and ipeciura
</strong>                        "userId": "145",
                        "userAssociatedAddresses": [
                          "0x1692101d7b84bf8ed8d828e44e55a8ca9a242bc4",
                          "0x3a8a1f045cd4f7246c6b3a78861269cc6065433a"
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
          "followingAddress": {
            "socialFollowings": {
<strong>              "Following": [] // Followed by betashop.eth, but isn't followed by ipeciura
</strong>            }
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Farcaster Followers of Farcaster User X That Is Also Farcaster Following of Farcaster User Y

You can get the list of Farcaster followers of Farcaster user X, e.g. `fc_fname:betashop.eth`, that also is followed by another Farcaster user Y, e.g. `fc_fname:ipeciura`, by inputting [0x address](#user-content-fn-9)[^9], [ENS domain](#user-content-fn-10)[^10], [Farcaster Name](#user-content-fn-11)[^11], or [Farcaster ID](#user-content-fn-12)[^12]:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/ktFRZuXzFm" %}
Show me followers of fc\_fname betashop.eth that is also followed by fc\_fname ipeciura
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_eq: "fc_fname:betashop.eth"}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        socialFollowings(
          input: {filter: {identity: {_eq: "fc_fname:ipeciura"}, dappName: {_eq: farcaster}}, limit: 200}
        ) {
          Following {
            followingAddress {
              socials(input: {filter: {dappName: {_eq: farcaster}}}) {
                fnames
                profileName
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
            "socialFollowings": {
              "Following": [
                {
                  "followingAddress": {
                    "socials": [
                      {
                        "fnames": [
                          "rish"
                        ],
<strong>                        "profileName": "rish", // Follow betashop.eth and is followed by ipeciura
</strong>                        "userId": "194",
                        "userAssociatedAddresses": [
                          "0xb43a7cc909d842721c288ff90b03e511a78a4a8d",
                          "0xe9e261852ea62150eee685807df8fe3f211310a0",
                          "0x5a927ac639636e534b678e81768ca19e2c6280b7"
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
            "socialFollowings": {
<strong>              "Following": [] // follows betashop.eth, but isn't followed by ipeciura
</strong>            }
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Farcaster Following of Farcaster User X That Is Also Farcaster Follower of Farcaster User Y

You can get the list of Farcaster following of Farcaster user X, e.g. `fc_fname:betashop.eth`, that also follows another Farcaster user Y, e.g. `fc_fname:ipeciura`, by inputting [0x address](#user-content-fn-13)[^13], [ENS domain](#user-content-fn-14)[^14], [Farcaster Name](#user-content-fn-15)[^15], or [Farcaster ID](#user-content-fn-16)[^16]:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/2O8pYeWLBt" %}
Show me following of fc\_fname betashop.eth that is also followers of fc\_fname ipeciura
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_eq: "fc_fname:betashop.eth"}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        socialFollowers(
          input: {filter: {identity: {_eq: "fc_fname:ipeciura"}, dappName: {_eq: farcaster}}, limit: 200}
        ) {
          Follower {
            followerAddress {
              socials(input: {filter: {dappName: {_eq: farcaster}}}) {
                fnames
                profileName
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
```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "socialFollowers": {
              "Follower": [
                {
                  "followerAddress": {
                    "socials": [
                      {
                        "fnames": [
                          "vishwa"
                        ],
                        "profileName": "vishwa", // is followed by betashop.eth and follows by ipeciura
                        "userId": "7701",
                        "userAssociatedAddresses": [
                          "0xe124b0590daa563746d86e3a810545c0e798136a",
                          "0x090f9b693b6b6d8213bc463235bddc65553c078f"
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
          "followingAddress": {
            "socialFollowers": {
              "Follower": [] // is followed by betashop.eth, but doesn't follow ipeciura
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

## Mutual Farcaster Follows of A Farcaster User

You can get the mutual Farcaster follows of a Farcaster user using the same query as Farcaster Followers of Farcaster User X That Is Also Farcaster Following of Farcaster User Y, where in this case X is equals to Y, e.g. `fc_fname:betashop.eth`:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/j8dMg9XSnv" %}
Show me mutual Farcaster follows of fc\_fname betashop.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_eq: "fc_fname:betashop.eth"}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        socialFollowings(
          input: {filter: {identity: {_eq: "fc_fname:betashop.eth"}, dappName: {_eq: farcaster}}, limit: 200}
        ) {
          Following {
            followingAddress {
              socials(input: {filter: {dappName: {_eq: farcaster}}}) {
                fnames
                profileName
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
            "socialFollowings": {
              "Following": [
                {
                  "followingAddress": {
                    "socials": [
                      {
                        "fnames": [
                          "asiablockchain.eth",
                          "hosein778"
                        ],
<strong>                        "profileName": "asiablockchain.eth", // mutually follows betashop.eth
</strong>                        "profileTokenId": "13752",
                        "profileTokenIdHex": "0x035b8",
                        "userId": "13752",
                        "userAssociatedAddresses": [
                          "0x5732411028f058a1c43e20c8e22a7a8cffbc04df",
                          "0xd78485a59e9763869bf1ec62c4520695bc826edc"
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
            "socialFollowings": {
<strong>              "Following": [] // follow betashop.eth, but is not followed back by betashop.eth
</strong>            }
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster followers and followings in common, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [SocialFollowers API](../../api-references/api-reference/socialfollowers-api.md)
* [SocialFollowings API](../../api-references/api-reference/socialfollowings-api.md)

[^1]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^2]: e.g. `varunsrin.eth`

[^3]: e.g. `fc_fname:dwr.eth`

[^4]: e.g. `fc_fid:602`

[^5]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^6]: e.g. `varunsrin.eth`

[^7]: e.g. `fc_fname:dwr.eth`

[^8]: e.g. `fc_fid:602`

[^9]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^10]: e.g. `varunsrin.eth`

[^11]: e.g. `fc_fname:dwr.eth`

[^12]: e.g. `fc_fid:602`

[^13]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^14]: e.g. `varunsrin.eth`

[^15]: e.g. `fc_fname:dwr.eth`

[^16]: e.g. `fc_fid:602`
