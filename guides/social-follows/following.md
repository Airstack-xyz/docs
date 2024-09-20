---
description: >-
  Learn how to fetch Web3 Social Following data and its various use cases and
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

# 💐 Following

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 social applications with and integrating on-chain and off-chain data with [Farcaster](https://farcaster.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get All Following of 0x Address(es)](following.md#get-all-following-of-0x-address-es)
- [Get All Following of ENS Domain(s)](following.md#get-all-following-of-ens-domain-s)
- [Get All Following of Farcaster User(s)](following.md#get-all-following-of-farcaster-user-s)
- [Check If User A Is Following User B](following.md#check-if-user-a-is-following-user-b)
- [Get The Most Recent Following of User(s)](following.md#get-the-most-recent-following-of-user-s)
- [Get The Earliest Following of User(s)](following.md#get-the-earliest-following-of-user-s)
- [Get Following of User(s) that has ENS Domain](following.md#get-following-of-user-s-that-has-ens-domain)
- [Get Users that have a certain amount of Following](following.md#get-users-that-have-a-certain-amount-of-following)

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

## Get All Following of 0x Address(es)

You can get the list of following data of user(s) by inputting an array of 0x address(es):

### Try Demo

{% embed url="https://app.airstack.xyz/query/2E5hM0rXyC" %}
Show me all following of 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: { _in: ["0xeaf55242a90bb3289dB8184772b0B98562053559"] }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
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
      followerAddress {
        addresses
      }
      followerProfileId
      followerTokenId
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
            "addresses": ["0xce8e9f29525e566db4d98fb22e156607b9b8109a"],
            "domains": [],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "nitesh",
                "profileTokenId": "240",
                "profileTokenIdHex": "0x0f0",
                "userId": "240",
                "userAssociatedAddresses": [
                  "0xce8e9f29525e566db4d98fb22e156607b9b8109a"
                ]
              }
            ]
          },
          "followingProfileId": "240",
          "followerAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ]
          },
          "followerProfileId": "602",
          "followerTokenId": ""
        }
        // more following of 0xeaf55242a90bb3289dB8184772b0B98562053559
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Following of ENS Domain(s)

You can get the list of following data of user(s) by inputting an array of ENS domain(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/KddWojr1WW" %}
Show me all following of vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: { identity: { _in: ["vitalik.eth"] } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
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
      followerAddress {
        domains {
          name
        }
      }
      followerProfileId
      followerTokenId
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
              "0xf6fd7deec77d7b1061435585df1d7fdfd4682577",
              "0x18b7511938fbe2ee08adf3d4a24edb00a5c9b783",
              "0x925afeb19355e289ed1346ede709633ca8788b25",
              "0x1040259f6fe4f1c6ed1044b9df6a8a4ebd2d3511"
            ],
            "domains": [
              {
                "name": "cryptovenetian.eth"
              },
              {
                "name": "phil.brightmoments.eth"
              },
              {
                "name": "mohun.eth"
              },
              {
                "name": "purple.philm.eth"
              },
              {
                "name": "surrogatemint.eth"
              },
              {
                "name": "cryptonewyorkers.eth"
              },
              {
                "name": "lorensbags.eth"
              },
              {
                "name": "pay.philmohun.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "phil",
                "profileTokenId": "129",
                "profileTokenIdHex": "0x081",
                "userId": "129",
                "userAssociatedAddresses": [
                  "0xf6fd7deec77d7b1061435585df1d7fdfd4682577",
                  "0x18b7511938fbe2ee08adf3d4a24edb00a5c9b783",
                  "0x925afeb19355e289ed1346ede709633ca8788b25",
                  "0x1040259f6fe4f1c6ed1044b9df6a8a4ebd2d3511"
                ]
              }
            ]
          },
          "followingProfileId": "129",
          "followerAddress": {
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
          "followerProfileId": "5650",
          "followerTokenId": ""
        }
        // more followers of vitalik.eth
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Following of Farcaster User(s)

You can get the list of following data of user(s) by inputting an array of either Farcaster name or [Farcaster ID](#user-content-fn-2)[^2]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/vYCQUv6j6W" %}
Show me all following of Farcaster user vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: { identity: { _in: ["fc_fname:vitalik.eth"] } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
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
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerTokenId
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
              "0xf6fd7deec77d7b1061435585df1d7fdfd4682577",
              "0x18b7511938fbe2ee08adf3d4a24edb00a5c9b783",
              "0x925afeb19355e289ed1346ede709633ca8788b25",
              "0x1040259f6fe4f1c6ed1044b9df6a8a4ebd2d3511"
            ],
            "domains": [
              {
                "name": "cryptovenetian.eth"
              },
              {
                "name": "phil.brightmoments.eth"
              },
              {
                "name": "mohun.eth"
              },
              {
                "name": "purple.philm.eth"
              },
              {
                "name": "surrogatemint.eth"
              },
              {
                "name": "cryptonewyorkers.eth"
              },
              {
                "name": "lorensbags.eth"
              },
              {
                "name": "pay.philmohun.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "phil",
                "profileTokenId": "129",
                "profileTokenIdHex": "0x081",
                "userId": "129",
                "userAssociatedAddresses": [
                  "0xf6fd7deec77d7b1061435585df1d7fdfd4682577",
                  "0x18b7511938fbe2ee08adf3d4a24edb00a5c9b783",
                  "0x925afeb19355e289ed1346ede709633ca8788b25",
                  "0x1040259f6fe4f1c6ed1044b9df6a8a4ebd2d3511"
                ]
              }
            ]
          },
          "followingProfileId": "129",
          "followerAddress": {
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
          "followerProfileId": "5650",
          "followerTokenId": ""
        }
        // more followers of Farcaster user vitalik.eth
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Check If User A Is Following User B

This can be done by providing the user B's identiy either a 0x address, ENS, cb.id, or Farcaster on the `Wallet` top-level query's `identity` input and the user A's identities in the [`socialFollowings`](../../api-references/api-reference/socialfollowings-api.md).

For example, check if [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) (user A) is following [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS) (user B):

### Try Demo

{% embed url="https://app.airstack.xyz/query/gxn0jQ0ADA" %}
Show me if betashop.eth is following ipeciura.eth
{% endembed %}

### Code

{% hint style="info" %}
If you need to check multiple users A simultaneously, then simply provide more identities into the `identity` input in the [`socialFollowings`](../../api-references/api-reference/socialfollowings-api.md) nested API.
{% endhint %}

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query isFollowing { # Top-level is User B's Identity (ipeciura.eth)
<strong>  Wallet(input: {identity: "ipeciura.eth", blockchain: ethereum}) {
</strong>    socialFollowings( # Here is User A's Identity (betashop.eth)
<strong>      input: {filter: {identity: {_in: ["betashop.eth"]}}}
</strong>    ) {
      Following {
        dappName
        dappSlug
        followingProfileId
        followerProfileId
        followerAddress {
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
      "socialFollowings": {
        "Following": [
          {
<strong>            "dappName": "farcaster", // following on Farcaster
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
<strong>                  "profileName": "betashop.eth" // betashop.eth is following ipeciura.eth
</strong>                },
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

If [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) is following [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth+ADDRESS+ethereum+null%29&inputType=ADDRESS&tokenType=&activeView=&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=) on Farcaster, then it will appear as a response in the `Following` array as shown in the [sample response](following.md#response-1) and thus should be classified as a **known sender**.

Otherwise, [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) will be considered an **unknown sender** and should be classified as one in the UI.

## Get The Most Recent Following of User(s)

You can get the list of most recent following of user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/wA67oO3Vdn" %}
Show me the most recent following of an array of users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
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
      order: { followingSince: DESC }
    }
  ) {
    Following {
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
              "0xba88168abd7e9d53a03be6ec63f6ed30d466c451",
              "0xbbd8f1ca6688e06bf19ad10067a14bbef9881753"
            ],
            "domains": [
              {
                "name": "hirsh.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@hirsh",
                "profileTokenId": "15280",
                "profileTokenIdHex": "0x03bb0",
                "userId": "0xba88168abd7e9d53a03be6ec63f6ed30d466c451",
                "userAssociatedAddresses": [
                  "0xba88168abd7e9d53a03be6ec63f6ed30d466c451"
                ]
              },
              {
                "dappName": "farcaster",
                "profileName": "hirsh",
                "profileTokenId": "989",
                "profileTokenIdHex": "0x03dd",
                "userId": "989",
                "userAssociatedAddresses": [
                  "0xba88168abd7e9d53a03be6ec63f6ed30d466c451",
                  "0xbbd8f1ca6688e06bf19ad10067a14bbef9881753"
                ]
              }
            ]
          },
          "followingProfileId": "989",
          "followerAddress": {
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
              }
            ]
          },
          "followerProfileId": "602",
          "followerTokenId": ""
        }
        // more following
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Earliest Following of User(s)

You can get the list of the earliest following of user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/Y1pnPpb4WD" %}
Show me the earliest following of an array of users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
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
      order: { followingSince: ASC }
    }
  ) {
    Following {
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
              "0xe978bdbc3347feb6ed10c9845bbdc298604f80ca",
              "0x71503b1309d6797bc4c4fbfde973adc2c191834b",
              "0xe0f35f554937d21ddb9e0f83b1436053e33634b9"
            ],
            "domains": [],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "ace",
                "profileTokenId": "539",
                "profileTokenIdHex": "0x021b",
                "userId": "539",
                "userAssociatedAddresses": [
                  "0xe978bdbc3347feb6ed10c9845bbdc298604f80ca",
                  "0x71503b1309d6797bc4c4fbfde973adc2c191834b",
                  "0xe0f35f554937d21ddb9e0f83b1436053e33634b9"
                ]
              }
            ]
          },
          "followingProfileId": "539",
          "followerAddress": {
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
              }
            ]
          },
          "followerProfileId": "602",
          "followerTokenId": ""
        }
        // more following
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Following of User(s) that has ENS Domain

You can get the list of all following of user(s) and check if they have any ENS domain:

### Try Demo

{% embed url="https://app.airstack.xyz/query/m1EwXDeSZs" %}
Show me followings of an array of users that also have ENS domain
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_in: ["lens/@stani", "lens_id:0x024", "fc_fname:dwr.eth", "fc_fid:3", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
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
      followingProfileId
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
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
              "0x223f2db258234f7fa164a9e4c0929318feb3b550"
            ],
            "domains": [
              {
<strong>                "name": "victorma.eth", // Indicate that following have ENS domain
</strong>                "isPrimary": true
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vm",
                "profileTokenId": "325",
                "profileTokenIdHex": "0x0145",
                "userId": "325",
                "userAssociatedAddresses": [
                  "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
                  "0x223f2db258234f7fa164a9e4c0929318feb3b550"
                ]
              }
            ]
          },
          "followingProfileId": "325",
          "followerAddress": {
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
          "followerProfileId": "3",
          "followerTokenId": ""
        },
        // more following
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

You can use the `followingAddress.domains` that will return an array to see if there's any domain owned by the following.

If the length of the `followingAddress.domains` array is non-zero, then the following has at least one ENS domain. Otherwise, it does not have any ENS domain.

## Get Users that have a certain amount of Following

You can get the list of all users that have certain amount of following, e.g. at least 1000 following:

### Try Demo

{% embed url="https://app.airstack.xyz/query/H7jmYgfbAi" %}
Show me all users that have at least 1000 following
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: { followingCount: { _gte: 1000 } }
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
          "dappName": "farcaster",
          "profileName": "jayme",
          "profileTokenId": "373",
          "profileTokenIdHex": "0x0175",
          "userId": "373",
          "userAssociatedAddresses": [
            "0x9eab9d856a3a667dc4cd10001d59c679c64756e7",
            "0x1ca66c990e86b750ea6b2180d17fff89273a5c0d"
          ],
          "followingCount": 1445
        },
        {
          "dappName": "farcaster",
          "profileName": "aydasayer",
          "profileTokenId": "14993",
          "profileTokenIdHex": "0x03a91",
          "userId": "14993",
          "userAssociatedAddresses": [
            "0x022fe87e83f3d6662862ed51ef74de33ed16ff25",
            "0xf009162e2495dd1dd54b5470e03004010f931a9f"
          ],
          "followingCount": 1245
        },
        {
          "dappName": "farcaster",
          "profileName": "reiri",
          "profileTokenId": "17626",
          "profileTokenIdHex": "0x044da",
          "userId": "17626",
          "userAssociatedAddresses": [
            "0x5f948ad313f280eb71d038f72d932f3b335458de",
            "0x688d0cef9d2ad572e2b889ea595a042a1821e5ff"
          ],
          "followingCount": 1054
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster followings data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)

[^1]: `profileTokenIdHex`
[^2]: e.g. `fc_fid:5650`
