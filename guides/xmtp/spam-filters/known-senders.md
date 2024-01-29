---
description: >-
  Learn how to build a primary inbox that only includes messages from known
  senders.
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

# 📔 Primary Inbox

The Primary Inbox should contain all the users that a given user certainly knows.&#x20;

Some criteria that can be checked for a user to be included in the primary inbox are:

* The sender is followed by the user on Lens
* The sender is followed by the user on Farcaster
* The user has sent the sender tokens

If the sender meets none of the listed criteria, they should be removed from the primary inbox and then checked to see if they belong in the [**General Inbox**](high-probability-of-connection.md).

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to build an XMTP primary inbox by:

* [Check If A Given User Is Following The Senders on Lens](known-senders.md#check-if-a-given-user-is-following-senders-on-lens)
* [Check If A Given User Is Following The Senders on Farcaster](known-senders.md#check-if-a-given-user-is-following-senders-on-farcaster)
* [Check If A Given User Has Sent Tokens To The Senders](known-senders.md#check-if-a-given-user-has-sent-tokens-to-senders)

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

## Best Practice

While choosing a specific criteria will significantly decrease the number of spam appearing on your user's XMTP inbox, It is best practice that you **combine** the multiple criterion given here to build your **known sender** inbox.

This is done to provide **multiple layers of filtration** that will make it nearly impossible for spammers to have their messages slide into your users' XMTP inbox.

## Check If A Given User Is Following Senders on Lens

You can check if a given user is following senders on Lens by providing an array of senders' 0x addresses to the `$senders` variable and the main user to the `$mainUser` variable using the [`Wallet`](../../wallet.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/sm0sottpSa" %}
Show me if a given user is following senders on Lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query isFollowing(
  $mainUser: Identity!,
  $senders: [Identity!]
) {
  Wallet(input: {identity: $mainUser, blockchain: ethereum}) {
    socialFollowers(
      input: {
        filter: {
          dappName: {_eq: lens},
          identity: {_in: $senders}
        }
      }
    ) {
      Follower {
        dappName
        followingAddress {
          addresses
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
    "Wallet": {
      "socialFollowers": {
        "Follower": [
          {
            "dappName": "lens",
            "followingAddress": {
              "addresses": [
                // main user is following this XMTP user on Lens
<strong>                "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>              ]
            }
          },
          {
            "dappName": "lens",
            "followingAddress": {
              "addresses": [
                // main user is following this XMTP user on Lens
<strong>                "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
</strong>              ]
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

## Check If A Given User Is Following Senders on Farcaster

You can check if a given user is following senders on Farcaster by providing an array of senders' 0x addresses to the `$senders` variable and the main user to the `$mainUser` variable using the [`Wallet`](../../wallet.md) API:

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/bW28GNZSZ7" %}
Show me if a given user is following senders on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query isFollowing(
  $mainUser: Identity!,
  $senders: [Identity!]
) {
  Wallet(input: {identity: $mainUser, blockchain: ethereum}) {
    socialFollowers(
      input: {
        filter: {
          dappName: {_eq: farcaster},
          identity: {_in: $senders}
        }
      }
    ) {
      Follower {
        dappName
        followingAddress {
          addresses
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
    "Wallet": {
      "socialFollowers": {
        "Follower": [
          {
            "dappName": "farcaster",
            "followingAddress": {
              "addresses": [
                "0x40ceb58e8f17ae4fa6124684aaad22a39c33fb8c",
                // The main user follow this XMTP user on Farcaster
<strong>                "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
</strong>              ]
            }
          },
          {
            "dappName": "farcaster",
            "followingAddress": {
              "addresses": [
                "0xe1b1e3bbf4f29bd7253d6fc1e2ddc9cacb0a546a",
                // The main user follow this XMTP user on Farcaster
<strong>                "0x0964256674e42d61f0ff84097e28f65311786ccb"
</strong>              ]
            }
          },
          {
            "dappName": "farcaster",
            "followingAddress": {
              "addresses": [
                "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
                // The main user follow this XMTP user on Farcaster
<strong>                "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
</strong>                // Other Farcaster connected addresses
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

## Check If A Given User Has Sent Tokens To Senders

You can check if a given user has sent any tokens to the senders on either Ethereum, Polygon, or Base by providing an array of senders' 0x addresses to the `$senders` variable and the main user to the `$mainUser` variable using the [`TokenTransfers`](../../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/6OQ7Is97pl" %}
Show historical token transfers from main user to senders
{% endembed %}

### Code

{% hint style="info" %}
You can use this query to filter senders **on the fly** with a maximum of 200 wallet inputs to the `$senders` variable per API call.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers(
  $mainUser: Identity!,
  $senders: [Identity!]
) {
  # Check Ethereum token transfers
<strong>  ethereum: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_eq: $mainUser},
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
  # Check Polygon token transfers
<strong>  polygon: TokenTransfers(
</strong>    input: {
      filter: {
        from: {_eq: $mainUser},
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
  # Check Base token transfers
  base: TokenTransfers(
    input: {
      filter: {
        from: {_eq: $mainUser},
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
}
</code></pre>
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
    "ethereum": {
      "TokenTransfer": [
        {
          "to": {
            "addresses": [
              "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
            ]
          }
        },
        // ...Other token transfers
        // If other XMTP users' addresses, does not appear,
        // then the main user never sent any tokens to those XMTP users
      ]
    },
    "polygon": {
      // no transfers from the main user to XMTP users on Polygon
<strong>      "TokenTransfer": null 
</strong>    },
    "base": {
      // no transfers from the main user to XMTP users on Base
<strong>      "TokenTransfer": null
</strong>    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding creating a primary inbox for your XMTP messaging app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Wallet API Reference](../../../api-references/api-reference/wallet-api.md)
* [TokenTransfers API Reference](../../../api-references/api-reference/tokentransfers-api.md)
* [Proof of Personhood](proof-of-personhood.md)
* [High Probability of Connection](high-probability-of-connection.md)
