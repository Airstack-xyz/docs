---
description: >-
  Learn how to fetch web3 social followers and followings in common between
  multiple users.
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

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 social applications with and integrating on-chain and off-chain data with [Lens](https://lens.xyz) and [Farcaster](https://farcaster.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Common Followers of Multiple User(s)](follows-in-common.md#common-followers-of-multiple-user-s)
- [Common Following of Multiple User(s)](follows-in-common.md#common-following-of-multiple-user-s)
- [Followers of User X That Also Following User Y](follows-in-common.md#followers-of-user-x-that-also-following-user-y)
- [Following of User X That Also Follows User Y](follows-in-common.md#following-of-user-x-that-also-follows-user-y)
- [Mutual Follows of A User](follows-in-common.md#mutual-follows-of-a-user)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL
- Finished [Followers](followers.md) and [Following](following.md)

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

## Common Followers of Multiple User(s)

You can get the list of common followers of multiple users by providing either 0x addresses, ENS names, Lens profiles, or Farcasters:

### Try Demo

{% embed url="https://app.airstack.xyz/query/hZX7uivC4S" %}
Show me common followers of both betashop.eth and ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: { identity: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        socialFollowers(
          input: { filter: { identity: { _eq: "ipeciura.eth" } }, limit: 200 }
        ) {
          Follower {
            followerAddress {
              socials {
                fnames
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
                  "followerAddress": {
                    "socials": [
                      // follow both betashop.eth and ipeciura.eth
                      {
                        "fnames": [
                          "prab.eth",
                          "prabh.eth",
                          "prab"
                        ],
                        "profileName": "prabh.eth",
                        "profileTokenId": "11946",
                        "profileTokenIdHex": "0x02eaa",
                        "userId": "11946",
                        "userAssociatedAddresses": [
                          "0x8c34ae28c1bb84785e4c059c301842b7be295a01",
                          "0x3570958b8dcbc4f663f508efcedb454ee9af9516",
                          "0x44f9047ec33dd3682df5e9178c492272a5f7afd0",
                          "0x71167f1794c90510671f3d122207696709ef4417"
                        ]
                      },
                      {
                        "fnames": null,
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
<strong>              "Follower": [] // Follow betashop.eth, but doesn't follow ipeciura.eth
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

## Common Following of Multiple User(s)

You can get the list of common following of multiple users by providing either 0x addresses, ENS names, Lens profiles, or Farcasters:

### Try Demo

{% embed url="https://app.airstack.xyz/query/I3UsTeLj1O" %}
Show me common following of both betashop.eth and ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: { identity: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        socialFollowings(
          input: { filter: { identity: { _eq: "ipeciura.eth" } }, limit: 200 }
        ) {
          Following {
            followingAddress {
              socials {
                fnames
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

## Followers of User X That Also Following User Y

You can get the list of followers of user X, e.g. `betashop.eth`, that also is followed by user Y, e.g. `ipeciura.eth`, by providing either 0x addresses, ENS names, Lens profiles, or Farcasters:

### Try Demo

{% embed url="https://app.airstack.xyz/query/0cSVfWNW5u" %}
Show me followers of betashop.eth that is also followed by ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: { identity: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        socialFollowings(
          input: { filter: { identity: { _eq: "ipeciura.eth" } }, limit: 200 }
        ) {
          Following {
            followingAddress {
              socials {
                fnames
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
            "socialFollowings": {
              "Following": [
                {
                  "followingAddress": {
                    "socials": [
                      // Follower of betashop.eth and is followed by ipeciura.eth
                      {
                        "fnames": [
<strong>                          "rish"
</strong>                        ],
                        "profileName": "rish",
                        "profileTokenId": "194",
                        "profileTokenIdHex": "0x0c2",
                        "userId": "194",
                        "userAssociatedAddresses": [
                          "0xb43a7cc909d842721c288ff90b03e511a78a4a8d",
                          "0xe9e261852ea62150eee685807df8fe3f211310a0",
                          "0x5a927ac639636e534b678e81768ca19e2c6280b7"
                        ]
                      },
                      {
                        "fnames": null,
                        "profileName": "lens/@rishm",
                        "profileTokenId": "117299",
                        "profileTokenIdHex": "0x01ca33",
                        "userId": "0x5a927ac639636e534b678e81768ca19e2c6280b7",
                        "userAssociatedAddresses": [
                          "0x5a927ac639636e534b678e81768ca19e2c6280b7"
                        ]
                      },
                      {
                        "fnames": null,
                        "profileName": "lens/@rishavmukherji",
                        "profileTokenId": "106198",
                        "profileTokenIdHex": "0x019ed6",
                        "userId": "0x5a927ac639636e534b678e81768ca19e2c6280b7",
                        "userAssociatedAddresses": [
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
<strong>              "Following": [] // follower of betashop, but isn't followed by ipeciura.eth
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

## Following of User X That Also Follows User Y

You can get the list of following of user X, e.g. `betashop.eth`, that also follows user Y, e.g. `ipeciura.eth`, by providing either 0x addresses, ENS names, Lens profiles, or Farcasters:

### Try Demo

{% embed url="https://app.airstack.xyz/query/pQ9Q4gfxL5" %}
Show me following of betashop.eth that also follows ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: { identity: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        socialFollowers(
          input: { filter: { identity: { _eq: "ipeciura.eth" } }, limit: 200 }
        ) {
          Follower {
            followerAddress {
              socials {
                fnames
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
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "socialFollowers": {
              "Follower": [
                {
                  "followerAddress": {
                    "socials": [
                      // is followed by betashop.eth and follows ipeciura.eth
                      {
                        "fnames": null,
<strong>                        "profileName": "lens/@vishwa",
</strong>                        "profileTokenId": "100439",
                        "profileTokenIdHex": "0x018857",
                        "userId": "0x090f9b693b6b6d8213bc463235bddc65553c078f",
                        "userAssociatedAddresses": [
                          "0x090f9b693b6b6d8213bc463235bddc65553c078f"
                        ]
                      },
                      {
                        "fnames": [
                          "vishwa"
                        ],
                        "profileName": "vishwa",
                        "profileTokenId": "7701",
                        "profileTokenIdHex": "0x01e15",
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
<strong>              "Follower": [] // is followed by betashop.eth, but does not follow ipeciura.eth
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

## Mutual Follows of A User

You can get the mutual follows of a user using the same query as Followers of User X That Also Following User Y, where in this case X is equals to Y, e.g. `betashop.eth`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/VQ7gmx56H4" %}
Show me mutual follows of betashop.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: { identity: { _eq: "betashop.eth" } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        socialFollowings(
          input: { filter: { identity: { _eq: "betashop.eth" } }, limit: 200 }
        ) {
          Following {
            followingAddress {
              socials {
                fnames
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

If you have any questions or need help regarding fetching follows in common data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
- [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
- [Follows In Common For Farcaster Devs](../farcaster/follows-in-common.md)
