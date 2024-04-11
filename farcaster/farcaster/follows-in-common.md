---
description: >-
  Learn how to fetch Farcaster users who are following or being followed by two
  Farcaster users.
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

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Farcaster Users Who Are Following Two Given Farcaster Users](follows-in-common.md#farcaster-users-who-are-following-two-given-farcaster-users)
* [Farcaster Users Who Are Being Followed By Two Given Farcaster Users](follows-in-common.md#farcaster-users-who-are-being-followed-by-two-given-farcaster-users)
* [Farcaster Users Who Are Following User X That Are Also Being Followed By User Y](follows-in-common.md#farcaster-users-who-are-following-user-x-that-are-also-being-followed-by-user-y)
* [Farcaster Users Who Are Being Followed By User X That Are Also Following By User Y](follows-in-common.md#farcaster-users-who-are-being-followed-by-user-x-that-are-also-following-by-user-y)
* [Mutual Follows of A Farcaster User](follows-in-common.md#mutual-farcaster-follows-of-a-farcaster-user)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL
* Finished [Farcaster Followers](farcaster-followers.md) and [Farcaster Following](farcaster-following.md)

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

### Farcaster Users Who Are Following Two Given Farcaster Users

You can get the list of Farcaster users which are following two given Farcaster users by inputting [0x address](#user-content-fn-1)[^1], ENS domain, [Farcaster Name](#user-content-fn-2)[^2], or Farcaster ID.

For example, get all Farcaster users which are following both Farcaster users `fc_fname:betashop.eth` and `fc_fname:ipeciura`:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/LggnwkvJ3K" %}
Show me common followers of both fc\_fname betashop.eth and fc\_fname ipeciura
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: { _eq: "fc_fname:betashop.eth" }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        socialFollowers(
          input: {
            filter: {
              identity: { _eq: "fc_fname:ipeciura" }
              dappName: { _eq: farcaster }
            }
            limit: 200
          }
        ) {
          Follower {
            followerAddress {
              socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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

### Farcaster Users Who Are Being Followed By Two Given Farcaster Users

You can get the list of all Farcaster users being followed by two given Farcaster users by inputting [0x address](#user-content-fn-3)[^3], ENS domain, [Farcaster Name](#user-content-fn-4)[^4], or [Farcaster ID](#user-content-fn-5)[^5].

For example, get all Farcaster users which are being followed by both Farcaster users `fc_fname:betashop.eth` and `fc_fname:ipeciura`:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/suXp8pV9TR" %}
Show me common following of both fc\_fname betashop.eth and fc\_fname ipeciura
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: { _eq: "fc_fname:betashop.eth" }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        socialFollowings(
          input: {
            filter: {
              identity: { _eq: "fc_fname:ipeciura" }
              dappName: { _eq: farcaster }
            }
            limit: 200
          }
        ) {
          Following {
            followingAddress {
              socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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

### Farcaster Users Who Are Following User X That Are Also Being Followed By User Y

You can get the list of Farcaster users that is following Farcaster user X who are also being followed by another Farcaster user Y by inputting [0x address](#user-content-fn-6)[^6], ENS domain, [Farcaster Name](#user-content-fn-7)[^7], or [Farcaster ID](#user-content-fn-8)[^8].

For example, get all Farcaster users that is following Farcaster user `fc_fname:betashop.eth` who are also being followed by `fc_fname:ipeciura`:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/ktFRZuXzFm" %}
Show me followers of fc\_fname betashop.eth that is also followed by fc\_fname ipeciura
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: { _eq: "fc_fname:betashop.eth" }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        socialFollowings(
          input: {
            filter: {
              identity: { _eq: "fc_fname:ipeciura" }
              dappName: { _eq: farcaster }
            }
            limit: 200
          }
        ) {
          Following {
            followingAddress {
              socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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

### Farcaster Users Who Are Being Followed By User X That Are Also Following By User Y

You can get a list of Farcaster users who are being followed by Farcaster user X and also are following another Farcaster user Y by inputting [0x address](#user-content-fn-9)[^9], ENS domain, [Farcaster Name](#user-content-fn-10)[^10], or [Farcaster ID](#user-content-fn-11)[^11].

For example, get all Farcaster users who are being followed by `fc_fname:betashop.eth` and also are following `fc_fname:ipeciura`:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/2O8pYeWLBt" %}
Show me following of fc\_fname betashop.eth that is also followers of fc\_fname ipeciura
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: { _eq: "fc_fname:betashop.eth" }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        socialFollowers(
          input: {
            filter: {
              identity: { _eq: "fc_fname:ipeciura" }
              dappName: { _eq: farcaster }
            }
            limit: 200
          }
        ) {
          Follower {
            followerAddress {
              socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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
                        "fnames": ["vishwa"],
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

### Mutual Farcaster Follows of A Farcaster User

You can fetch all Farcaster users that are following a given Farcaster user X and check if Farcaster user X is mutually following back.

For example, get all Farcaster users that are following `fc_fname:betashop.eth` and check if `fc_fname:betashop.eth` is mutually following back:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/j8dMg9XSnv" %}
Show me mutual Farcaster follows of fc\_fname betashop.eth
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: { _eq: "fc_fname:betashop.eth" }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        socialFollowings(
          input: {
            filter: {
              identity: { _eq: "fc_fname:betashop.eth" }
              dappName: { _eq: farcaster }
            }
            limit: 200
          }
        ) {
          Following {
            followingAddress {
              socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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

### Developer Support

If you have any questions or need help regarding fetching Farcaster followers and followings in common, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [SocialFollowers API](../../api-references/api-reference/socialfollowers-api.md)
* [SocialFollowings API](../../api-references/api-reference/socialfollowings-api.md)

[^1]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^2]: e.g. `fc_fname:dwr.eth`

[^3]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^4]: e.g. `fc_fname:dwr.eth`

[^5]: e.g. `fc_fid:5650`

[^6]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^7]: e.g. `fc_fname:dwr.eth`

[^8]: e.g. `fc_fid:5650`

[^9]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^10]: e.g. `fc_fname:dwr.eth`

[^11]: e.g. `fc_fid:5650`
