---
description: >-
  Learn how to enable users to access certain features only if they follow a
  certain user or have common followers with a given user.
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

# 🚪 Token Gating

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 social applications with and integrating on-chain and off-chain data with [Farcaster](https://farcaster.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Gating Only User(s) That Follows A Given User](token-gating.md#gating-only-user-s-that-have-common-followers)
- [Gating Only User(s) That Have Common Followers](token-gating.md#gating-only-user-s-that-have-common-followers)

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

## Gating Only User(s) That Follows A Given User

You can build a token-gating system that gates only users that follow a given user by checking if user A is following a given user B.

This can be done by providing the user B's identity either a 0x address, ENS, cb.id, or Farcaster on the [`Wallet`](../../api-references/api-reference/wallet-api.md) top-level query's `identity` input and the user A's identities in the [`socialFollowing`](../../api-references/api-reference/socialfollowings-api.md).

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

If [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) is following [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth+ADDRESS+ethereum+null%29&inputType=ADDRESS&tokenType=&activeView=&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=) on Farcaster, then it will appear as a response in the `Following` array as shown in the [sample response](token-gating.md#response-1) and thus should **be given feature access**.

Otherwise, [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) will be considered an **not be given any feature access**.

## Gating Only User(s) That Have Common Followers

You can build a token-gating system that gates only users that have common followers with a given user.

This can be done by fetching the intersection of following between two or more users providing either 0x addresses, ENS names, or Farcaster profiles.

For example, check if [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) (user A) has any common following [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS) (user B):

### Try Demo

{% embed url="https://app.airstack.xyz/query/I3UsTeLj1O" %}
Show me common following of both betashop.eth and ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowings( # user A: betashop.eth
<strong>    input: {filter: {identity: {_eq: "betashop.eth"}}, blockchain: ALL, limit: 200}
</strong>  ) {
    Following {
      followingAddress {
        socialFollowings( # user B: ipeciura.eth
<strong>          input: {filter: {identity: {_eq: "ipeciura.eth"}}, limit: 200}
</strong>        ) {
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
</code></pre>

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
<strong>                        "profileName": "nickcherry", // this is common following
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

If [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) has common following [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth+ADDRESS+ethereum+null%29&inputType=ADDRESS&tokenType=&activeView=&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=) on Farcaster, then it will appear as a response in the innermost `Following` array as shown in the [sample response](token-gating.md#response-1) and thus should **be given feature access**.

Otherwise, [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS) will be considered an **not be given any feature access**.

## Developer Support

If you have any questions or need help creating social following token gating, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
- [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
