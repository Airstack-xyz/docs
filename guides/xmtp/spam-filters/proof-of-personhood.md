---
description: >-
  Learn how you can build and split your request inbox by using Airstack to
  determine if a user can be considered a real user or not in your XMTP
  messaging app.
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

# 🎭 Request Inbox

<div>

<figure><img src="../../../.gitbook/assets/iPhone 14 &#x26; 15 Pro - 57.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/iPhone 14 &#x26; 15 Pro - 59.png" alt=""><figcaption></figcaption></figure>

</div>

The request inbox is for users unknown to the main user and without any connections. It typically holds spam, which users should avoid.&#x20;

There could also occasionally be real users with no connections who might want to contact the main user. To manage this, we recommend dividing the request inbox even further into two parts:

* **Top Requests**: This includes likely real users from the request inbox.
* **All Requests**: This contains all users in the request inbox, without filtering.

Use the Airstack API to check if senders in the request inbox are genuine and place them in the **Top Requests Inbox**.

Some criteria that can be checked for splitting the request inbox:

* Senders Have Non-Virtual POAPs (IRL POAPs can only be minted in person, so this is a strong positive signal)
* Senders have X number of followers on Farcaster (e.g. if the user has >1000 followers you may have some confidence they are a real user)
* Senders have X number of followers on Lens
* Senders have/have not sent tokens/NFTs previously (If the user has no wallet history it's a strong negative signal)
* Senders hold some tokens/NFTs (If the user has no wallet history it's a strong negative signal)

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to create a request inbox by:

* [Check If Senders Have Non-Virtual POAPs](proof-of-personhood.md#check-if-senders-have-any-non-virtual-poaps)
* [Check If Senders Have X or More Followers on Lens](proof-of-personhood.md#check-if-senders-have-x-or-more-followers-on-lens)
* [Check If Senders Have X or More Followers on Farcaster](proof-of-personhood.md#check-if-senders-have-x-or-more-followers-on-farcaster)
* [Check If Senders Have Any Token Transfers](proof-of-personhood.md#check-if-senders-have-any-token-transfers)
* [Check If Senders Have Any Token Balances](proof-of-personhood.md#check-if-senders-have-any-token-balances)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [XMTP](https://xmtp.org)

## Get Started

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

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Best Practice

While choosing a specific criteria will significantly decrease the number of spam appearing on your user's XMTP inbox, It is best practice that you **combine** the multiple criterion given here to build and split your **Request Inbox**.

This is done to provide **multiple layers of filtration** that will make it nearly impossible for spammers to have their messages slide into your users' XMTP inbox.

## Check If Senders Have Any Token Transfers

You can check if senders have sent any token by providing an array of senders' 0x addresses to the `$senders` variable using the [`TokenTransfers`](../../../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/0OUsMMyg43" %}
Show me token transfers by senders on Ethereum, Polygon, Base, and Zora
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers($senders: [Identity!]) {
  ethereum: TokenTransfers(
    input: {filter: {from: {_in: $senders}}, blockchain: ethereum, order: {blockTimestamp: DESC}}
  ) {
    TokenTransfer {
      transactionHash
      from {
        addresses
      }
    }
  }
  polygon: TokenTransfers(
    input: {filter: {from: {_in: $senders}}, blockchain: polygon, order: {blockTimestamp: DESC}}
  ) {
    TokenTransfer {
      transactionHash
      from {
        addresses
      }
    }
  }
  base: TokenTransfers(input: {filter: {from: {_in: $senders}}, blockchain: base, order: {blockTimestamp: DESC}}) {
    TokenTransfer {
      transactionHash
      from {
        addresses
      }
    }
  }
  zora: TokenTransfers(input: {filter: {from: {_in: $senders}}, blockchain: zora, order: {blockTimestamp: DESC}}) {
    TokenTransfer {
      transactionHash
      from {
        addresses
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
    "ethereum": {
<strong>      "TokenTransfer": [ // If TokenTransfer array is not empty, then there are token transfers 
</strong>        {
          "transactionHash": "0xd2e0d4e8aae125a7edae14c7dab106c1620be136b239e7f9dbd60861034b0c25",
          "from": {
            "addresses": "0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB"
          }
        },
        // other Ethereum token transfers
      ]
    },
    "polygon": {
<strong>      "TokenTransfer": [ // If TokenTransfer array is not empty, then there are token transfers 
</strong>        {
          "transactionHash": "0x499ec2aa83944bdcdd73abd0c069a46d7fffdff77845cc079bdf5c450cae5814",
          "from": {
            "addresses": "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2"
          }
        },
        // Other Polygon token transfers
      ]
    },
    "base": {
<strong>      "TokenTransfer": [ // If TokenTransfer array is not empty, then there are token transfers 
</strong>        {
          "transactionHash": "0x5b7faf6bd2266c3344bd5ee79fdbca32416b5162b97afbd41a39bbd1516d1ea7",
          "from": {
            "addresses": "0x0964256674E42d61f0fF84097E28F65311786ccb"
          }
        },
        // Other Base token transfers
      ]
    },
    "zora": {
      "TokenTransfer": [
        {
          "transactionHash": "0x928c96854d4aa1c7852472885efb0c2506b7a2b27e81562d6fef6480f9e12a86",
          "from": {
            "addresses": [
              "0x0964256674e42d61f0ff84097e28f65311786ccb"
            ]
          }
        },
        // Other Zora token transfers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If Senders Have Any Token Balances

You can check if senders hold any token in their wallet across multiple chains, e.g. Ethereum, Polygon, Base, or Zora, by providing an array of senders' 0x addresses to the `$senders` variable using the [`TokenBalances`](../../../api-references/api-reference/tokenbalances-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/uUiadl5vB6" %}
show me senders' token balances on Ethereum, Polygon, Base, and Zora
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery($senders: [Identity!]) {
  # Ethereum token balances
<strong>  Ethereum: TokenBalances(
</strong>    input: {
      filter: { owner: { _in: $senders } }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      tokenAddress
      owner {
        addresses
      }
    }
  }
  # Polygon token balances
<strong>  Polygon: TokenBalances(
</strong>    input: {
      filter: { owner: { _in: $senders } }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      tokenAddress
      owner {
        addresses
      }
    }
  }
  # Base token balances
<strong>  Base: TokenBalances(
</strong>    input: {
      filter: { owner: { _in: $senders } }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      tokenAddress
      owner {
        addresses
      }
    }
  }
  # Zora token balances
<strong>  Zora: TokenBalances(
</strong>    input: {
      filter: { owner: { _in: $senders } }
      blockchain: zora
      limit: 50
    }
  ) {
    TokenBalance {
      tokenAddress
      owner {
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
<strong>      "TokenBalance": [ // Shows that hold some tokens in Ethereum
</strong>        {
          "tokenAddress": "0xbf5495efe5db9ce00f80364c8b423567e58d2110",
          "owner": {
            "addresses": [
              // This sender hold tokens inside its wallet on Ethereum
<strong>              "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>            ]
          }
        },
        // more Ethereum tokens
      ]
    },
    "Polygon": {
      "TokenBalance": [
        {
          "tokenAddress": "0x8a2c53c06348d4af7f3fce97a79124205a92dcf7",
          "owner": {
            "addresses": [
              // This sender hold tokens inside its wallet on Polygon
<strong>              "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>            ]
          }
        },
        // more Polygon tokens
      ]
    },
    "Base": {
      "TokenBalance": [
        {
          "tokenAddress": "0x73d8048044b24e6fba5833849c3e5c26c6523719",
          "owner": {
            "addresses": [
              // This sender hold tokens inside its wallet on Base
<strong>              "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>            ]
          }
        },
        // more Base tokens
      ]
    },
    "Zora": {
      "TokenBalance": [
        {
          "tokenAddress": "0x7b7a8caf6882d29ebd893b038b873d277344e397",
          "owner": {
            "addresses": [
              // This sender hold tokens inside its wallet on Zora
<strong>              "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>            ]
          }
        },
        // more Zora tokens
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If Senders Have Any Non-Virtual POAPs

You can check if senders have attended any non-virtual POAPs by providing an array of senders' 0x addresses to the `$senders` variable using the [`Poaps`](../../../api-references/api-reference/poaps-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

#### Try Demo

{% embed url="https://app.airstack.xyz/query/79XSqLCL5I" %}
show me all POAPs owned by senders and see if they are virtual or not
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwned($senders: [Identity!]) {
  Poaps(
    input: {
      filter: { owner: { _in: $senders } }
      blockchain: ALL
      limit: 50
    }
  ) {
    Poap {
      mintOrder
      mintHash
      poapEvent {
        isVirtualEvent
      }
      owner {
        addresses
      }
    }
    pageInfo {
      nextCursor
      prevCursor
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
    "Poaps": {
      "Poap": [
        {
          "mintOrder": 5,
          "mintHash": "0x4974ddc3ada2100b8e4cb2e17fb993324f71e0676cdd33dfff107899fca16e90",
          "poapEvent": {
<strong>            "isVirtualEvent": false // This is not virtual event
</strong>          },
          "owner": {
            "addresses": [
              // This sender has attended a non-virtual POAP event
<strong>              "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>            ]
          }
        },
        {
          "mintOrder": 12,
          "mintHash": "0xdbb3a298401c221e6596557cecd0c0c8944e0dd538b7d81b6d97449b5911ce98",
          "poapEvent": {
<strong>            "isVirtualEvent": true // This is virtual event
</strong>          },
          "owner": {
            "addresses": [
              "0x0964256674e42d61f0ff84097e28f65311786ccb"
            ]
          }
        },
        // more POAPs
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If Senders Have X or More Followers on Lens

You can check if senders have X or more Lens followers by providing an array of senders' 0x addresses to the `$senders` variable using the [`Socials`](../../../api-references/api-reference/socials-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/FkIONmlhsH" %}
Check if senders' Lens profile have more than or equal to 100 followers on Lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($xmtpUsers: Identity!) {
  Socials(
    input: {
      filter: {
        followerCount: {_gt: 100},
        dappName: {_eq: lens},
        identity: {_in: $senders}
      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Social {
      followerCount
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
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "followerCount": 231,
          "userAddress": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
        },
        {
          "followerCount": 209,
          "userAddress": "0x0964256674e42d61f0ff84097e28f65311786ccb"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If Senders Have X or More Followers on Farcaster

You can check if senders have X or more Farcaster followers by providing an array of senders' 0x addresses to the `$senders` variable using the [`Socials`](../../../api-references/api-reference/socials-api.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/Ls1K8nOeZ7" %}
Check if senders' Farcaster profile have more than or equal to 100 followers on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($senders: [Identity!]) {
  Socials(
    input: {
      filter: {
        followerCount: {_gt: 100},
        dappName: {_eq: farcaster},
        identity: {_in: $senders}
      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Social {
      followerCount
      userAddress
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
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "followerCount": 51956,
          "userAddress": "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1"
        },
        {
          "followerCount": 4320,
          "userAddress": "0xe1b1e3bbf4f29bd7253d6fc1e2ddc9cacb0a546a"
        },
        {
          "followerCount": 164,
          "userAddress": "0x40ceb58e8f17ae4fa6124684aaad22a39c33fb8c"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding creating a request inbox on your XMTP messaging app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenTransfers API Reference](../../../api-references/api-reference/tokentransfers-api.md)
* [TokenBalances API Reference](../../../api-references/api-reference/tokenbalances-api.md)
* [Poaps API Reference](../../../api-references/api-reference/poaps-api.md)
* [Domains API Reference](../../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../../api-references/api-reference/socials-api.md)
* [Primary Inbox](known-senders.md)
* [General Inbox](high-probability-of-connection.md)
