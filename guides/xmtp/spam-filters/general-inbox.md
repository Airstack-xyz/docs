---
description: >-
  Learn how to use Airstack to build General inbox and determine all the users
  that have a high probability connection with the given user. If they don't
  qualify, they should be in request inbox
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

# 🕸 General Inbox

The General inbox should contain all the users that a given user is likely to know or we have strong signals that they are a good actor.&#x20;

Some criteria that can be checked for a user to be included in the general inbox are:

- \>X match score on **Onchain Graph, which includes**:
  - Sender has non-virtual POAPs in common with the user
  - Sender has minted >X NFTs in common with the user
  - Sender has several NFTs in common with the user
- People the User follows are following the Sender on Farcaster&#x20;
- People the User follows are following the Sender on Lens
- People the User follows have sent the Sender tokens&#x20;
- Senders have ENS and other factors (e.g. ENS + attended non-virtual POAPs)

If the sender doesn't meet the criteria, they should be removed from the main inbox and then placed into the [**Request Inbox**](request-inbox.md).

Additionally, you can use [XMTP Consent](https://xmtp.org/docs/build/user-consent) to check if a sender has given consent (has an `Allowed` consent value) to the sender before including them in the primary inbox. By default, you should place all senders that have been given consent directly into the **Primary Inbox**.

As demonstrated below, your app's interface can also feature user-friendly options for users to apply consent or deny consent. Applying consent should move the user to the Primary Inbox.

<div>

<figure><img src="../../../.gitbook/assets/Move to Primary 1-min.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Move to Primary 2-min.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Move to Primary 4-min.png" alt=""><figcaption></figcaption></figure>

</div>

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to build an XMTP general inbox by:

- [Check If Senders' Onchain Graph Score Above X](general-inbox.md#check-if-senders-onchain-graph-score-above-x)
  - [Common POAP Events Attended](general-inbox.md#common-poap-events-attended)
  - [Common NFT Collections Minted](general-inbox.md#common-nft-collections-minted)
  - [Common NFT Collections Hold](general-inbox.md#common-nft-collections-hold)
- [Check If The Senders Followed By Those Followed by The User on Lens](general-inbox.md#check-if-the-senders-followed-by-those-followed-by-the-user-on-lens)
- [Check If The Senders Followed By Those Followed by The User on Farcaster](general-inbox.md#check-if-the-senders-followed-by-those-followed-by-the-user-on-farcaster)
- [Check If Any of The User's Followers on Lens or Farcaster Sent Any Token To The Sender](general-inbox.md#check-if-any-of-the-users-followers-on-lens-or-farcaster-sent-any-token-to-the-sender)
- [Check If XMTP Users Have Any ENS and Attended Any Non-Virtual POAPs](general-inbox.md#check-if-xmtp-users-have-any-ens-and-attended-any-non-virtual-poaps)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL
- Basic knowledge of [XMTP](https://xmtp.org)

## Get Started

### **JavaScript/TypeScript/Python**

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

### **Other Programming Languages**

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Best Practice

While choosing a specific criteria will significantly decrease the number of spam appearing on your user's XMTP inbox, It is best practice that you **combine** the multiple criterion given here to build your **General Inbox**.

This is done to provide **multiple layers of filtration** that will make it nearly impossible for spammers to have their messages slide into your users' XMTP inbox.

## Check If Senders' Onchain Graph Score Above X

{% hint style="info" %}
To use this criteria, you will **NEED TO** have a dedicated backend to store user's onchain graph data from Airstack API.
{% endhint %}

To see if a user's [onchain graph](../../onchain-graph.md) score is higher than a specific threshold (for example, value X), your application must already have an [onchain graph](../../onchain-graph.md) feature. If it doesn't, follow this tutorial to add one:

{% content-ref url="../../onchain-graph.md" %}
[onchain-graph.md](../../onchain-graph.md)
{% endcontent-ref %}

After implementing the [onchain graph](../../onchain-graph.md), you'll be able to examine the onchain connections between **the user** and **the senders**, assigning scores to each.

Besides the [onchain graph](../../onchain-graph.md) score, you can also include other conditions to evaluate these criteria:

### Common POAP Events Attended

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

You can check if the senders have any common POAP events attended with the user by providing an array of senders' 0x addresses to the `$senders` variable and the user to the `$mainUser` variable using the [`Poaps`](../../../api-references/api-reference/poaps-api.md) API:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/Y5Y1VIAueL" %}
show me the common POAPs attended by the user and the senders
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query CommonPOAPs($mainUser: Identity!, $senders: [Identity!]) {
  Poaps(
    input: { filter: { owner: { _eq: $mainUser } }, blockchain: ALL, limit: 50 }
  ) {
    Poap {
      eventId
      poapEvent {
        poaps(input: { filter: { owner: { _in: $senders } } }) {
          owner {
            addresses
          }
        }
      }
    }
  }
}
```

{% endtab %}

{% tab title="Variables" %}

```json
{
  "mainUser": "0xeaf55242a90bb3289dB8184772b0B98562053559",
  "senders": [
    "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB",
    "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2",
    "0x0964256674E42d61f0fF84097E28F65311786ccb"
  ]
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "eventId": "148642",
          "poapEvent": {
            "poaps": [
              {
                "owner": {
                  "addresses": [
                    // This sender attended the same event as main user
<strong>                    "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
</strong>                  ]
                }
              }
            ]
          }
        },
        {
          "eventId": "149066",
          "poapEvent": {
<strong>            "poaps": null // This POAP has no common attendees
</strong>          }
        },
        // Other POAPs
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

### Common NFT Collections Minted

You can check if the senders have any common NFT collections minted with the user by taking 2 steps.&#x20;

First, fetch all the NFT addresses minted by the user by providing the user to the `$mainUser` variable using the [`TokenTransfers`](../../../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
For this check, you will need a backend to store all the NFT that the user minted

You can update the data on your end periodically to get the data most up to date.
{% endhint %}

#### Try Demo

{% embed url="https://app.airstack.xyz/query/aH3JdzEORM" %}
Show me all NFTs minted by user
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery($mainUser: Identity!) {
  Ethereum: TokenTransfers(
    input: {
      filter: {
        to: { _eq: $mainUser }
        operator: { _eq: $mainUser }
        from: { _eq: "0x0000000000000000000000000000000000000000" }
        tokenType: { _in: [ERC721, ERC1155] }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenTransfer {
      tokenAddress
    }
  }
  Polygon: TokenTransfers(
    input: {
      filter: {
        to: { _eq: $mainUser }
        operator: { _eq: $mainUser }
        from: { _eq: "0x0000000000000000000000000000000000000000" }
        tokenType: { _in: [ERC721, ERC1155] }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenTransfer {
      tokenAddress
    }
  }
  Base: TokenTransfers(
    input: {
      filter: {
        to: { _eq: $mainUser }
        operator: { _eq: $mainUser }
        from: { _eq: "0x0000000000000000000000000000000000000000" }
        tokenType: { _in: [ERC721, ERC1155] }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenTransfer {
      tokenAddress
    }
  }
  Zora: TokenTransfers(
    input: {
      filter: {
        to: { _eq: $mainUser }
        operator: { _eq: $mainUser }
        from: { _eq: "0x0000000000000000000000000000000000000000" }
        tokenType: { _in: [ERC721, ERC1155] }
      }
      blockchain: zora
      limit: 50
    }
  ) {
    TokenTransfer {
      tokenAddress
    }
  }
}
```

{% endtab %}

{% tab title="Variables" %}

```json
{
  "mainUser": "0xeaf55242a90bb3289dB8184772b0B98562053559"
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "tokenAddress": "0xabefbc9fd2f806065b4f3c237d4b59d9a97bcac7"
        },
        {
          "tokenAddress": "0x0f92612c5f539c89dc379e8aa42e3ba862a34b7e"
        },
        {
          "tokenAddress": "0x9d90669665607f08005cae4a7098143f554c59ef"
        }
        // Other Ethereum NFTs minted by user
      ]
    },
    "Polygon": {},
    "Base": {},
    "Zora": {}
  }
}
```

{% endtab %}
{% endtabs %}

Once you have the list of NFT addresses from each chain, you can them compile them into an array and provide them as a variable input to the next query along with an array of senders' 0x addresses to the `$senders` variable to the [`TokenTransfers`](../../../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

#### Try Demo

{% embed url="https://app.airstack.xyz/query/7dmCbRKGEZ" %}
Show me all common NFTs minted by senders across Ethereum, Polygon, and Base
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery(
  $senders: [Identity!],
  $ethereumNfts: [Address!],
  $polygonNfts: [Address!],
  $baseNfts: [Address!]
) {
  # Fetch Ethereum NFT mints
<strong>  Ethereum: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        to: {_in: $senders},
        operator: {_in: $senders},
        tokenAddress: {_in: $ethereumNfts}
      },
      blockchain: ethereum
    }
  ) {
    TokenTransfer {
      to {
        addresses
      }
    }
  }
  # Fetch Polygon NFT mints
<strong>  Polygon: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        to: {_in: $senders},
        operator: {_in: $senders},
        tokenAddress: {_in: $polygonNfts}
      },
      blockchain: polygon
    }
  ) {
    TokenTransfer {
      to {
        addresses
      }
    }
  }
  # Fetch Base NFT mints
<strong>  Base: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        to: {_in: $senders},
        operator: {_in: $senders},
        tokenAddress: {_in: $baseNfts}
      },
      blockchain: base
    }
  ) {
    TokenTransfer {
      to {
        addresses
      }
    }
  }
  # No Zora as no Zora NFT is minted by user
}
</code></pre>

{% endtab %}

{% tab title="Variables" %}

<pre class="language-json"><code class="lang-json">{
  // All Ethereum NFTs minted
<strong>  "ethereumNfts": [
</strong>    "0xabefbc9fd2f806065b4f3c237d4b59d9a97bcac7",
    "0x0f92612c5f539c89dc379e8aa42e3ba862a34b7e",
    "0x9d90669665607f08005cae4a7098143f554c59ef",
    // Other Ethereum NFTs minted
  ],
  // All Polygon NFTs minted
<strong>  "polygonNfts": [
</strong>    "0x6c6bac0c1adef88f6c35f59181a49bdc78e002f8",
    "0xaef66b056e5daaafc7565700d1f4543b58184b57",
    "0x3d77bbbe0c438db1d3ede544825b7559dddcdb6f",
    // Other Polygon NFTs minted
  ],
  // All Base NFTs minted
<strong>  "baseNfts": [
</strong>    "0x7d5861cfe1c74aaa0999b7e2651bf2ebd2a62d89",
    "0x2a2ca163b4f6806c12f7aad8263ad551ef8ec255",
    "0xba5e05cb26b78eda3a2f8e3b3814726305dcac83".
    // Other Base NFTs minted
  ],
  // No Zora NFTs as the user has no NFT minted in Zora
  "senders": [
    "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB",
    "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2",
    "0x0964256674E42d61f0fF84097E28F65311786ccb"
  ]
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "to": {
            "addresses": [
              // This sender has the same NFT collection minted as
              // the user on Ethereum
<strong>              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
</strong>            ]
          }
        },
        // Other NFTs minted
      ]
    },
    "Polygon": {
<strong>      "TokenTransfer": null // The senders did not mint anything on Polygon
</strong>    },
    "Base": {
      "TokenTransfer": [
        {
          "to": {
            "addresses": [
              // This sender has the same NFT collection minted as
              // the user on Base
<strong>              "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>            ]
          }
        },
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

### Common NFT Collections Hold

You can check if the senders have any common NFT collections hold with the user by providing an array of senders' 0x addresses to the `$senders` variable and the user to the `$mainUser` variable using the [`TokenBalances`](../../../api-references/api-reference/tokenbalances-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

#### Try Demo

{% embed url="https://app.airstack.xyz/query/ksjURw05GQ" %}
Show me all common NFTs hold between user and sender
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery($mainUser: Identity!, $senders: [Identity!]) {
  Ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _eq: $mainUser }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      token {
        tokenBalances(input: { filter: { owner: { _in: $senders } } }) {
          tokenAddress
          owner {
            addresses
          }
        }
      }
    }
  }
  Polygon: TokenBalances(
    input: {
      filter: {
        owner: { _eq: $mainUser }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      token {
        tokenBalances(input: { filter: { owner: { _in: $senders } } }) {
          tokenAddress
          owner {
            addresses
          }
        }
      }
    }
  }
  Base: TokenBalances(
    input: {
      filter: {
        owner: { _eq: $mainUser }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      token {
        tokenBalances(input: { filter: { owner: { _in: $senders } } }) {
          tokenAddress
          owner {
            addresses
          }
        }
      }
    }
  }
  Zora: TokenBalances(
    input: {
      filter: {
        owner: { _eq: $mainUser }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: zora
      limit: 50
    }
  ) {
    TokenBalance {
      token {
        tokenBalances(input: { filter: { owner: { _in: $senders } } }) {
          tokenAddress
          owner {
            addresses
          }
        }
      }
    }
  }
}
```

{% endtab %}

{% tab title="Variables" %}

```json
{
  "mainUser": "0xeaf55242a90bb3289dB8184772b0B98562053559",
  "senders": [
    "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB",
    "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2",
    "0x0964256674E42d61f0fF84097E28F65311786ccb"
  ]
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
      "TokenBalance": [
        {
          "token": {
            "tokenBalances": [
              {
                "tokenAddress": "0x95cb845b525f3a2126546e39d84169f1eca8c77f",
                "owner": {
                  "addresses": [
                    // This sender hold a common NFT with the user
<strong>                    "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
</strong>                  ]
                }
              }
            ]
          }
        },
        {
          "token": {
<strong>            "tokenBalances": [] // This NFT is hold by main user, but not the senders
</strong>          }
        },
        // Other NFTs on Ethereum
      ]
    },
    // Common NFTs on Polygon
<strong>    "Polygon": {},
</strong>    // Common NFTs on Base
<strong>    "Base": {},
</strong>    // Common NFTs on Zora
<strong>    "Zora": {}
</strong>  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Check If The Senders Followed By Those Followed by The User on Lens

{% hint style="info" %}
For this check, you will need a backend to store the data fetched:

- the user's followings on Lens
- the addresses that is being followed by the user's followings on Lens

You can update the data on your end periodically to get the data most up to date.
{% endhint %}

You can check if the senders are is being followed by one of the Lens user followed by the given user on Lens by fetching data from Airstack twice.

First, fetch all the Lens followings of a user by providing the main user to the `$mainUser` variable using the [`SocialFollowers`](../../../api-references/api-reference/socialfollowers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/hcPIPm0XSE" %}
Get all Lens followings of a given user
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery($mainUser: Identity!) {
  # Get all the user's followers
<strong>  SocialFollowings(
</strong>    input: {
      filter: {
        identity: {_eq: $mainUser},
        # Only get followers on Lens
<strong>        dappName: {_eq: lens}
</strong>      },
      blockchain: ALL
    }
  ) {
    Following {
      followingAddress {
        addresses
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Variables" %}

```json
{
  "mainUser": "0xeaf55242a90bb3289dB8184772b0B98562053559"
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
            "addresses": ["0xe5b38d69b10b0a5d990c000fb5bdfce04e6a4071"]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0x714b831eb02fe854283219b2b9f1c6951f46dcb9"]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0xb366d5b211f90625ac9ce9af3325e9e6fa627777"]
          }
        }
        // Other followings on Farcaster
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

Then, compile the list of users followings and provide it as a variable input to `$userFollowings` by using the [`SocialFollowings`](../../../api-references/api-reference/socialfollowings-api.md) API:

{% hint style="info" %}
This query accepts a maximum of 200 wallet inputs to the `$userFollowings` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/hM5ujvfOrR" %}
Show me all addresses that is being followed by the user's followings on Lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery($userFollowers: [Identity!]) {
  # Get all the Lens followings of the user's followings
<strong>  SocialFollowings(
</strong>    input: {
      filter: {
        identity: {_in: $userFollowings},
        # Only check on Lens
        dappName: {_eq: lens}
      },
      blockchain: ALL
    }
  ) {
    Following {
      followingAddress {
        addresses
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Variables" %}

```json
{
  "userFollowings": [
    "0xe5b38d69b10b0a5d990c000fb5bdfce04e6a4071",
    "0x714b831eb02fe854283219b2b9f1c6951f46dcb9",
    "0xb366d5b211f90625ac9ce9af3325e9e6fa627777"
  ]
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
            "addresses": ["0x2ee5baf1fbaf679b5611fa2b93ae4c752ff62693"]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0x960fd829fbec155f616102f6e654bfab9b9b3ae4"]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0xeaf55242a90bb3289db8184772b0b98562053559"]
          }
        }
        // Other addresses being followed by user's followings on Lens
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

Once you have all these data stored in your backend, you can use it to check whether the sender is one of the addresses that is being followed by the user's followings on Lens.

## Check If The Senders Followed By Those Followed by The User on Farcaster

{% hint style="info" %}
For this check, you will need a backend to store the data fetched:

- the user's followings on Farcaster
- the addresses that is being followed by the user's followings on Farcaster

You can update the data on your end periodically to get the data most up to date.
{% endhint %}

You can check if the senders are is being followed by one of the user's followings on Farcaster by fetching data from Airstack twice.

First, fetch all the Farcaster followings of a user by providing the main user to the `$mainUser` variable using the [`SocialFollowers`](../../../api-references/api-reference/socialfollowers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/I05ZH0bp0A" %}
Get all Farcaster followings of a given user
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery($mainUser: Identity!) {
  # Get all the user's followings
<strong>  SocialFollowings(
</strong>    input: {
      filter: {
        identity: {_eq: $mainUser},
        # Only get followings on Farcaster
<strong>        dappName: {_eq: farcaster}
</strong>      },
      blockchain: ALL
    }
  ) {
    Following {
      followingAddress {
        addresses
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Variables" %}

```json
{
  "mainUser": "0xeaf55242a90bb3289dB8184772b0B98562053559"
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
            "addresses": ["0x2fd0e9793f691b097e782c871ac300cf35d0c315"]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0xff77c6d3392202c84a347d17f8499c580df8ec78"]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0x52d543a222d3a499a0e88d05b3d9739b9936f66c"]
          }
        }
        // Other followings on Farcaster
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

Then, compile the list of users followings and provide it as a variable input to `$userFollowings` by using the [`SocialFollowings`](../../../api-references/api-reference/socialfollowings-api.md) API:

{% hint style="info" %}
This query accepts a maximum of 200 wallet inputs to the `$userFollowings` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/YT60m6Qywk" %}
Show me all addresses that is being followed by the user's followings on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery($userFollowings: [Identity!]) {
  # Get all the Farcaster followings of the user's followings
<strong>  SocialFollowings(
</strong>    input: {
      filter: {
        identity: {_in: $userFollowings},
        # Only check on Farcaster
        dappName: {_eq: farcaster}
      },
      blockchain: ALL
    }
  ) {
    Following {
      followingAddress {
        addresses
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Variables" %}

```json
{
  "userFollowings": [
    "0x2fd0e9793f691b097e782c871ac300cf35d0c315",
    "0xff77c6d3392202c84a347d17f8499c580df8ec78",
    "0x52d543a222d3a499a0e88d05b3d9739b9936f66c"
  ]
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
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ]
          }
        },
        {
          "followingAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ]
          }
        }
        // Other addresses being followed by user's followers on Farcaster
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

Once you have all these data stored in your backend, you can use it to check whether the sender is one of the addresses that is being followed by the user's followings on Farcaster.

## Check If Any of The User's Followers on Lens or Farcaster Sent Any Token To The Sender

You can check if the senders are is being followed by one of the user's followers on Farcaster by fetching data from Airstack twice.

First, fetch all the Lens and Farcaster followers of a user by providing the main user to the `$mainUser` variable using the [`SocialFollowers`](../../../api-references/api-reference/socialfollowers-api.md) API:

{% hint style="info" %}
For this check, you will need a backend to store the user's followers on Lens and Farcaster.

You can update the data on your end periodically to get the data most up to date.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/qOKffKdoMY" %}
Get all Lens and Farcaster followers of a given user
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery($mainUser: Identity!) {
  # Get all the user's followers
<strong>  SocialFollowers(
</strong>    input: {
      filter: {
        identity: {_eq: $mainUser}
      },
      blockchain: ALL
    }
  ) {
    Follower {
      followerAddress {
        addresses
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Variables" %}

```json
{
  "mainUser": "0xeaf55242a90bb3289dB8184772b0B98562053559"
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
          "dappName": "farcaster",
          "followerAddress": {
            "addresses": ["0x2fd0e9793f691b097e782c871ac300cf35d0c315"]
          }
        },
        {
          "dappName": "farcaster",
          "followerAddress": {
            "addresses": ["0xff77c6d3392202c84a347d17f8499c580df8ec78"]
          }
        },
        {
          "dappName": "farcaster",
          "followerAddress": {
            "addresses": ["0x52d543a222d3a499a0e88d05b3d9739b9936f66c"]
          }
        }
        // Other followers on Lens & Farcaster
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

Then, compile the list of users followers and provide it as a variable input to `$userFollowers` along with the senders' 0x address to the `$senders` variable by using the [`TokenTransfers`](../../../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/G2Jnn5qDiU" %}
Show all token transfers from user's followers on Lens or Farcaster sent to senders
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery(
  $userFollowers: [Identity!],
  $senders: [Identity!]
) {
  # Check for any Ethereum Token Transfers
<strong>  Ethereum: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_in: $userFollowers},
        to: {_in: $senders}
      },
      blockchain: ethereum
    }
  ) {
    TokenTransfer {
      to {
        addresses
      }
    }
  }
  # Check for any Polygon Token Transfers
<strong>  Polygon: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_in: $userFollowers},
        to: {_in: $senders}
      },
      blockchain: polygon
    }
  ) {
    TokenTransfer {
      to {
        addresses
      }
    }
  }
  # Check for any Base Token Transfers
<strong>  Base: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_in: $userFollowers},
        to: {_in: $senders}
      },
      blockchain: base
    }
  ) {
    TokenTransfer {
      to {
        addresses
      }
    }
  }
  # Check for any Zora Token Transfers
<strong>  Zora: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_in: $userFollowers},
        to: {_in: $senders}
      },
      blockchain: zora
    }
  ) {
    TokenTransfer {
      to {
        addresses
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Variables" %}

```json
{
  "userFollowers": [
    "0x2fd0e9793f691b097e782c871ac300cf35d0c315",
    "0xff77c6d3392202c84a347d17f8499c580df8ec78",
    "0x52d543a222d3a499a0e88d05b3d9739b9936f66c"
  ],
  "senders": [
    "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB",
    "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2",
    "0x0964256674E42d61f0fF84097E28F65311786ccb"
  ]
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "to": {
            "addresses": [
              // This sender has received token transfer
              // from one of the user's follower
<strong>              "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
</strong>            ]
          }
        },
        // Other token transfers on Ethereum
      ]
    },
    "Polygon": {
      "TokenTransfer": null
    },
    "Base": {
      "TokenTransfer": null
    },
    "Zora": {
      "TokenTransfer": null
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Check If XMTP Users Have Any ENS and Attended Any Non-Virtual POAPs

You can check if senders has any ENS and attended any non-virtual POAPs by providing the senders' 0x address to the `$senders` variable using the [`Poaps`](../../../api-references/api-reference/poaps-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/hQYsGQwUw9" %}
Show me senders' ENS domain and non-virtual POAPs attended
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery($senders: [Identity!]) {
  Domains(
    input: { filter: { owner: { _in: $senders } }, blockchain: ethereum }
  ) {
    Domain {
      name
      owner
      ownerDetails {
        poaps {
          poapEvent {
            isVirtualEvent
            eventName
            eventId
          }
        }
      }
    }
  }
}
```

{% endtab %}

{% tab title="Variables" %}

```json
{
  "senders": [
    "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB",
    "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2",
    "0x0964256674E42d61f0fF84097E28F65311786ccb"
  ]
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Domains": {
      "Domain": [
        {
          "name": "dawufi.eth",
          "owner": "0x0964256674e42d61f0ff84097e28f65311786ccb",
          "ownerDetails": {
            "poaps": [
              {
                "poapEvent": {
<strong>                  "isVirtualEvent": false, // This is non-virtual POAP
</strong>                  "eventName": "Skyteller x Consensus 2023",
                  "eventId": "124226"
                }
              },
              // Other POAPs
            ]
          }
        },
        // Other ENS domain that is owned by the sender
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding building your general inbox on your XMTP messaging app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Poaps API Reference](../../../api-references/api-reference/poaps-api.md)
- [SocialFollowers API Reference](../../../api-references/api-reference/socialfollowers-api.md)
- [Primary Inbox](primary-inbox.md)
- [Request Inbox](request-inbox.md)
