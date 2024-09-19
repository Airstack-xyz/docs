---
description: >-
  Learn how to fetch Web3 Social Followers data and its various use cases and
  combinations.
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

# 🫂 Followers

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 social applications with and integrating on-chain and off-chain data with [Lens](https://lens.xyz) and [Farcaster](https://farcaster.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get All Followers of 0x Address(es)](followers.md#get-all-followers-of-0x-address-es)
- [Get All Followers of ENS Domain(s)](followers.md#get-all-followers-of-ens-domain-s)
- [Get All Followers of Farcaster User(s)](followers.md#get-all-followers-of-farcaster-user-s)
- [Check If User A Is Followed By User B](followers.md#check-if-user-a-is-followed-by-user-b)
- [Get The Most Recent Followers of User(s)](followers.md#get-the-most-recent-followers-of-user-s)
- [Get The Earliest Followers of User(s)](followers.md#get-the-earliest-followers-of-user-s)
- [Get All Followers of User(s) that has ENS Domain](followers.md#get-all-followers-of-user-s-that-has-ens-domain)
- [Get Users that have a certain amount of Followers](followers.md#get-users-that-have-a-certain-amount-of-followers)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL

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

## Get All Followers of 0x Address(es)

You can get the list of followers data of user(s) by inputting an array of [0x address(es)](#user-content-fn-1)[^1]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/TT504ypwC4" %}
Show me all followers of 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: { _in: ["0xeaf55242a90bb3289dB8184772b0B98562053559"] }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
      }
      followingProfileId
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
            "addresses": ["0x3a134eb077cda063dec6f507e969556ddcb4e1f9"],
            "domains": [],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "florist",
                "profileTokenId": "13915",
                "profileTokenIdHex": "0x0365b",
                "userId": "13915",
                "userAssociatedAddresses": [
                  "0x3a134eb077cda063dec6f507e969556ddcb4e1f9"
                ]
              }
            ]
          },
          "followerProfileId": "13915",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ]
          },
          "followingProfileId": "602"
        }
        // more followers of 0xeaf55242a90bb3289dB8184772b0B98562053559
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Followers of ENS Domain(s)

You can get the list of all follower of ENS domains(s) by providing ENS domain name:

### Try Demo

{% embed url="https://app.airstack.xyz/query/61Rka41Q8E" %}
Show me all followers of vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: { identity: { _in: ["vitalik.eth"] } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        domains {
          name
        }
      }
      followingProfileId
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
            "addresses": [
              "0x3e21146008f07e27745918b71357f1b3abc30ca8",
              "0x99d80e209b4904aa2ef25a6014e1a7cf3ed6d5ee"
            ],
            "domains": [],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "scnullois",
                "profileTokenId": "6119",
                "profileTokenIdHex": "0x017e7",
                "userId": "6119",
                "userAssociatedAddresses": [
                  "0x3e21146008f07e27745918b71357f1b3abc30ca8",
                  "0x99d80e209b4904aa2ef25a6014e1a7cf3ed6d5ee"
                ]
              }
            ]
          },
          "followerProfileId": "6119",
          "followerTokenId": "",
          "followingAddress": {
            "domains": [
              {
                "name": "quantumexchange.eth"
              },
              {
                "name": "7860000.eth"
              },
              {
                "name": "offchainexample.eth"
              },
              {
                "name": "brianshaw.eth"
              },
              {
                "name": "vbuterin.stateofus.eth"
              },
              {
                "name": "quantumsmartcontracts.eth"
              },
              {
                "name": "Vitalik.eth"
              },
              {
                "name": "openegp.eth"
              },
              {
                "name": "vitalik.cannafam.eth"
              },
              {
                "name": "VITALIK.eth"
              },
              {
                "name": "quantumdapps.eth"
              },
              {
                "name": "vitalik.soy.eth"
              },
              {
                "name": "satoshinart.eth"
              },
              {
                "name": "vitalik.eth"
              },
              {
                "name": "eth.soy.eth"
              },
              {
                "name": "vitalik.thepeople.eth"
              },
              {
                "name": "vitalik.dev.eth"
              },
              {
                "name": "vitalik.takoyaki.eth"
              }
            ]
          },
          "followingProfileId": "5650"
        }
        // more followers of vitalik.eth
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Followers of Farcaster User(s)

You can get the list of followers of Farcaster user(s) by either providing either [Farcaster name](#user-content-fn-3)[^3] or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/gU6seFnay6" %}
Show me all followers of Farcaster user vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: { identity: { _in: ["fc_fname:vitalik.eth"] } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
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
            "addresses": [
              "0x3e21146008f07e27745918b71357f1b3abc30ca8",
              "0x99d80e209b4904aa2ef25a6014e1a7cf3ed6d5ee"
            ],
            "domains": [],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "scnullois",
                "profileTokenId": "6119",
                "profileTokenIdHex": "0x017e7",
                "userId": "6119",
                "userAssociatedAddresses": [
                  "0x3e21146008f07e27745918b71357f1b3abc30ca8",
                  "0x99d80e209b4904aa2ef25a6014e1a7cf3ed6d5ee"
                ]
              }
            ]
          },
          "followerProfileId": "6119",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "socials": [
              {
                "profileName": "vitalik.eth",
                "profileTokenId": "5650",
                "profileTokenIdHex": "0x01612"
              }
            ]
          },
          "followingProfileId": "5650"
        }
        // more followers of Farcaster user vitalik.eth
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Check If User A Is Followed By User B

This can be done by providing the user B's identity either a 0x address, ENS, cb.id, Lens, or Farcaster on the `Wallet` top-level query's `identity` input and the user A's identities in the [`socialFollowers`](../../api-references/api-reference/socialfollowers-api.md).

For example, check if [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) (user A) is followed by [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS) (user B):

### Try Demo

{% embed url="https://app.airstack.xyz/query/2S7JXZj5cj" %}
Show me if betashop.eth is being followed by ipeciura.eth
{% endembed %}

### Code

{% hint style="info" %}
If you need to check multiple users A simultaneously, then simply provide more identities into the `identity` input in the [`socialFollowers`](../../api-references/api-reference/socialfollowers-api.md) nested API.
{% endhint %}

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query isFollowing { # Top-level is User B's Identity (ipeciura.eth)
<strong>  Wallet(input: {identity: "ipeciura.eth", blockchain: ethereum}) {
</strong>    socialFollowers( # Here is User A's Identity (betashop.eth)
<strong>      input: {filter: {identity: {_in: ["betashop.eth"]}}}
</strong>    ) {
      Follower {
        dappName
        dappSlug
        followingProfileId
        followerProfileId
        followingAddress {
          addresses
          socials {
            dappName
            profileName
          }
          domains {
            name
          }
        }
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
        "Followers": [
          {
<strong>            "dappName": "farcaster", // follower on Farcaster
</strong>            "dappSlug": "farcaster_optimism",
            "followingProfileId": "2602",
            "followerProfileId": "602",
            "followerAddress": {
              "addresses": [
                "0x66bd69c7064d35d146ca78e6b186e57679fba249",
                "0xeaf55242a90bb3289db8184772b0b98562053559"
              ],
              "socials": [
                {
                  "dappName": "farcaster",
<strong>                  "profileName": "betashop.eth" // betashop.eth is being followed by ipeciura.eth
</strong>                },
                {
                  "dappName": "lens",
                  "profileName": "lens/@betashop9"
                }
              ],
              "domains": [
                {
                  "name": "jasongoldberg.eth"
                },
                {
                  "name": "betashop.eth"
                }
              ]
            }
          }
        ]
      }
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

If [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) is followed by [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth+ADDRESS+ethereum+null%29&inputType=ADDRESS&tokenType=&activeView=&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=) on either Lens or Farcaster, then it will appear as a response in the `Follower` array as shown in the [sample response](followers.md#response-4) and thus should be classified as a **known sender**.

Otherwise, [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) will be considered an **unknown sender** and should be classified as one in the UI.

## Get The Most Recent Followers of User(s)

You can get the list of most recent followers of users(s) by providing either 0x addresses, ENS names, Lens profiles, or Farcasters:

### Try Demo

{% embed url="https://app.airstack.xyz/query/pTiFfL5oq3" %}
Show me the most recent followers of an array of users
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
          _in: [
            "lens/@stani"
            "lens_id:0x024"
            "fc_fname:dwr.eth"
            "fc_fid:3"
            "vitalik.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
      }
      blockchain: ALL
      limit: 200
      order: { followerSince: DESC }
    }
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
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
            "addresses": ["0x0d2ad52c9ce562bffecf2d4381e23e97f5ff5267"],
            "domains": [],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "alcueca",
                "profileTokenId": "19805",
                "profileTokenIdHex": "0x04d5d",
                "userId": "19805",
                "userAssociatedAddresses": [
                  "0x0d2ad52c9ce562bffecf2d4381e23e97f5ff5267"
                ]
              }
            ]
          },
          "followerProfileId": "19805",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0x74232bf61e994655592747e20bdf6fa9b9476f79",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
              },
              {
                "name": "noun124.eth"
              },
              {
                "name": "dwr.mirror.xyz"
              },
              {
                "name": "danromero.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@danromero",
                "profileTokenId": "46189",
                "profileTokenIdHex": "0x0b46d",
                "userId": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                "userAssociatedAddresses": [
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              },
              {
                "dappName": "farcaster",
                "profileName": "dwr.eth",
                "profileTokenId": "3",
                "profileTokenIdHex": "0x03",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "followingProfileId": "3"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Earliest Followers of User(s)

You can get the list of the earliest followers of user(s) by providing either 0x addresses, ENS names, Lens profiles, or Farcasters:

### Try Demo

{% embed url="https://app.airstack.xyz/query/eSaapEosfa" %}
Show me the earliest followers of an array of users
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
          _in: [
            "lens/@stani"
            "lens_id:0x024"
            "fc_fname:dwr.eth"
            "fc_fid:3"
            "vitalik.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
      }
      blockchain: ALL
      limit: 200
      order: { followerSince: ASC }
    }
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
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
            "addresses": [
              "0x8005a9309966eb88f5f63291e0fe56cc7fbc09b1",
              "0x416e0c10a3760a35f1891455f30e415e96fbd779"
            ],
            "domains": [
              {
                "name": "maxbranzburg.eth"
              },
              {
                "name": "chriswolf.eth"
              },
              {
                "name": "branzburg.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "max",
                "profileTokenId": "23",
                "profileTokenIdHex": "0x017",
                "userId": "23",
                "userAssociatedAddresses": [
                  "0x8005a9309966eb88f5f63291e0fe56cc7fbc09b1",
                  "0x416e0c10a3760a35f1891455f30e415e96fbd779"
                ]
              }
            ]
          },
          "followerProfileId": "23",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "0x74232bf61e994655592747e20bdf6fa9b9476f79"
            ],
            "domains": [
              {
                "name": "dwr.eth"
              },
              {
                "name": "noun124.eth"
              },
              {
                "name": "dwr.mirror.xyz"
              },
              {
                "name": "danromero.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@danromero",
                "profileTokenId": "46189",
                "profileTokenIdHex": "0x0b46d",
                "userId": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                "userAssociatedAddresses": [
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              },
              {
                "dappName": "farcaster",
                "profileName": "dwr.eth",
                "profileTokenId": "3",
                "profileTokenIdHex": "0x03",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79"
                ]
              }
            ]
          },
          "followingProfileId": "3"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Followers of User(s) that has ENS Domain

You can get the list of all followers of user(s) and check if they have any ENS domain:

### Try Demo

{% embed url="https://app.airstack.xyz/query/tqP6R0fzfr" %}
Show me all followers of users that have ENS domain
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["lens/@stani", "lens_id:0x024", "fc_fname:dwr.eth", "fc_fid:3", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
<strong>        domains { # Show all domains owned by followers
</strong>          name
          isPrimary
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0xac7a709f09eac9769c567a24a858fe65b44850af",
              "0x3a497a2a8b35b934bf271938502ca24e444e689e"
            ],
            "domains": [
              {
                "name": "pfizergang.eth",
                "isPrimary": true
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@pfizergang",
                "profileTokenId": "66242",
                "profileTokenIdHex": "0x0102c2",
                "userId": "0x3a497a2a8b35b934bf271938502ca24e444e689e",
                "userAssociatedAddresses": [
                  "0x3a497a2a8b35b934bf271938502ca24e444e689e"
                ]
              },
              {
                "dappName": "farcaster",
                "profileName": "pfizergang",
                "profileTokenId": "16250",
                "profileTokenIdHex": "0x03f7a",
                "userId": "16250",
                "userAssociatedAddresses": [
                  "0xac7a709f09eac9769c567a24a858fe65b44850af",
                  "0x3a497a2a8b35b934bf271938502ca24e444e689e"
                ]
              }
            ]
          },
          "followerProfileId": "16250",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ],
            "domains": [
              {
                "name": "jasongoldberg.eth"
              },
              {
                "name": "betashop.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "betashop.eth",
                "profileTokenId": "602",
                "profileTokenIdHex": "0x025a",
                "userId": "602",
                "userAssociatedAddresses": [
                  "0x66bd69c7064d35d146ca78e6b186e57679fba249",
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              },
              {
                "dappName": "lens",
                "profileName": "lens/@betashop9",
                "profileTokenId": "94472",
                "profileTokenIdHex": "0x017108",
                "userId": "0xeaf55242a90bb3289db8184772b0b98562053559",
                "userAssociatedAddresses": [
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              }
            ]
          },
          "followingProfileId": "602"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

You can use the `followerAddress.domains` that will return an array to see if there's any domain owned by the follower.

If the length of the `followerAddress.domains` array is non-zero, then the follower has at least one ENS domain. Otherwise, it does not have any ENS domain.

## Get Users that have a certain amount of Followers

You can get the list of all users of that have a certain amount of followers, e.g. at least 1000 followers:

### Try Demo

{% embed url="https://app.airstack.xyz/query/tZyC2q2Be6" %}
Show me all users that have at least 1000 followers
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: { followerCount: { _gte: 1000 } }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      dappName
      profileName
      profileTokenId
      profileTokenIdHex
      userId
      userAssociatedAddresses
      followerCount
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
          "dappName": "farcaster",
          "profileName": "pfista",
          "profileTokenId": "451",
          "profileTokenIdHex": "0x01c3",
          "userId": "451",
          "userAssociatedAddresses": [
            "0xd2344f892241e3a83c7eca8e586093714be8ca7f",
            "0x8a95d5f9ac0eebd5880199ea8fcd0fae845bbbf2",
            "0x92382f5c6ad39829a07537d18807462960025725"
          ],
          "followerCount": 7622
        },
        {
          "dappName": "farcaster",
          "profileName": "jayme",
          "profileTokenId": "373",
          "profileTokenIdHex": "0x0175",
          "userId": "373",
          "userAssociatedAddresses": [
            "0x1ca66c990e86b750ea6b2180d17fff89273a5c0d",
            "0x9eab9d856a3a667dc4cd10001d59c679c64756e7"
          ],
          "followerCount": 9556
        },
        {
          "dappName": "farcaster",
          "profileName": "macbudkowski",
          "profileTokenId": "1048",
          "profileTokenIdHex": "0x0418",
          "userId": "1048",
          "userAssociatedAddresses": [
            "0x66cc8f831f2a05fde7573f18ba6abfef8ad59d34",
            "0x3e6c23cdaa52b1b6621dbb30c367d16ace21f760"
          ],
          "followerCount": 5768
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching followers data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
