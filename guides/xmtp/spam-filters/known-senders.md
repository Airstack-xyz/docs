---
description: >-
  Learn how you can categorize a message sender as a known or unknown one to
  avoid unknown and unwanted senders spamming your user's XMTP inbox.
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

# 📔 Known Senders

## 📔 Known Senders

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [XMTP](https://xmtp.org) applications and integrating on-chain and off-chain data with [XMTP](https://xmtp.org).

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to:

* [Check If User A Is Following User B](known-senders.md#check-if-user-a-is-following-user-b)
* [Check If User A Have Any Token Transfers History With User B](known-senders.md#check-if-user-a-have-any-token-transfers-history-with-user-b)

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

While choosing a specific criteria will significantly decrease the number of spam appearing on your user's XMTP inbox, It is best practice that you **combine** the multiple criterion given here to build your **known sender** inbox.

This is done to provide **multiple layers of filtration** that will make it nearly impossible for spammers to have their messages slide into your users' XMTP inbox.

### Check If User A Is Following User B

This can be done by providing the user B's identiy either a 0x address, ENS, cb.id, Lens, or Farcaster on the `Wallet` top-level query's `identity` input and the user A's identities in the [`socialFollowings`](../../../api-references/api-reference/socialfollowings-api.md).

For example, check if [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS) (user A) is following [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29\&inputType=ADDRESS) (user B):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/gxn0jQ0ADA" %}
Show me if betashop.eth is following ipeciura.eth
{% endembed %}

#### Code

{% hint style="info" %}
If you need to check multiple users A simultaneously, then simply provide more identities into the `identity` input in the [`socialFollowings`](../../../api-references/api-reference/socialfollowings-api.md) nested API.
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

If [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS) is following [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth+ADDRESS+ethereum+null%29\&inputType=ADDRESS\&tokenType=\&activeView=\&activeTokenInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=) on either Lens or Farcaster, then it will appear as a response in the `Following` array as shown in the [sample response](known-senders.md#response-1) and thus should be classified as a **known sender**.

Otherwise, [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS) will be considered an **unknown sender** and should be classified as one in the UI.

### Check If User A Have Any Token Transfers History With User B

This can be done by providing the user A's identity either a 0x address, ENS, cb.id, Lens, or Farcaster on the `from` input and the user B's on the `to` input.

For example, check if [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS) (user A) have transfered any tokens [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29\&inputType=ADDRESS) (user B):

Try Demo

{% embed url="https://app.airstack.xyz/query/fBJOJH9bZA" %}
Show token transfers history from betashop.eth to ipeciura.eth
{% endembed %}

#### Code

{% hint style="info" %}
If you need to check multiple users A simultaneously, then simply provide more identities into the `from` input in the both the `ethereum` and `polygon` query.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers {
  ethereum: TokenTransfers(
    input: {
      filter: { from: { _in: ["betashop.eth"] }, to: { _eq: "ipeciura.eth" } }
      blockchain: ethereum
    }
  ) {
    TokenTransfer {
      from {
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
      to {
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
      transactionHash
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  polygon: TokenTransfers(
    input: {
      filter: { from: { _in: ["betashop.eth"] }, to: { _eq: "ipeciura.eth" } }
      blockchain: polygon
    }
  ) {
    TokenTransfer {
      from {
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
      to {
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
      transactionHash
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "ethereum": {
<strong>      "TokenTransfer": [ // If the array is not empty, this indicates there are token transferred previously
</strong>        {
          "from": {
            "addresses": [
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
          "to": {
            "addresses": [
              "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
            ],
            "domains": [
              {
                "name": "ipeciura.eth"
              },
              {
                "name": "shnoodles.eth"
              },
              {
                "name": "🅱🏪👨‍💻.eth"
              },
              {
                "name": "getjam.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@shnoodles",
                "profileTokenId": "27561",
                "profileTokenIdHex": "0x6ba9",
                "userId": "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab",
                "userAssociatedAddresses": [
                  "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
                ]
              },
              {
                "dappName": "farcaster",
                "profileName": "ipeciura",
                "profileTokenId": "2602",
                "profileTokenIdHex": "0x0a2a",
                "userId": "2602",
                "userAssociatedAddresses": [
                  "0x40ceb58e8f17ae4fa6124684aaad22a39c33fb8c",
                  "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
                ]
              }
            ]
          },
          "transactionHash": "0x4f98621583fa88beae2ef2d8c3cfd13541b202d806e5da76ea98008bbb48d119"
        },
        // other token transfers
      ],
      "pageInfo": {
        "nextCursor": "",
        "prevCursor": ""
      }
    },
    "polygon": {
<strong>      "TokenTransfer": null, // Indicate no token transfers on Polygon
</strong>      "pageInfo": {
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS) have transferred any tokens to [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth+ADDRESS+ethereum+null%29\&inputType=ADDRESS\&tokenType=\&activeView=\&activeTokenInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=) on either Ethereum or Polygon, then either the `ethereum.TokenTransfer` or the `polygon.TokenTransfer` array has non-zero length. Thus, [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS) should be classified as a **known sender**.

Otherwise, `betashop.eth` will be considered an **unknown sender** and should be classified as one in the UI.

### Developer Support

If you have any questions or need help regarding creating a known sender inbox for your XMTP messaging app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Wallet API Reference](../../../api-references/api-reference/wallet-api/)
* [TokenTransfers API Reference](../../../api-references/api-reference/tokentransfers-api/)
* [Proof of Personhood](proof-of-personhood.md)
* [High Probability of Connection](high-probability-of-connection.md)
