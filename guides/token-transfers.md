---
description: >-
  Learn how to use Airstack to fetch all ERC20/721/1155 token transfers data,
  from or to user(s), across Ethereum, Base, Zora, and other Airstack supported
  chains.
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

# 💸 Token Transfers

[Airstack](https://airstack.xyz) provides a [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API for you to fetch all ERC20/721/1155 token transfer data from either a user or multiple users on Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains).

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get Token Transfers Sent From A User](token-transfers.md#get-token-transfers-sent-from-a-user)
- [Get Token Transfers Received By A User](token-transfers.md#get-token-transfers-received-by-a-user)
- [Get The Most Recent Token Transfers Sent From A User](token-transfers.md#get-the-most-recent-token-transfers-sent-from-a-user-s)
- [Get The Most Recent Token Transfers Received By A User](token-transfers.md#get-the-most-recent-token-transfers-received-by-a-user)

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

## Get Token Transfers Sent From A User

You can fetch all token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS), on Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains) by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
For fetching token transfers data from multiple chains, check out [Cross-Chain Queries](basics/cross-chain-queries.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/ZGtUdm88Lv" %}
Show me token transfers sent from betashop.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: ethereum,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 25,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        },
        {
          "formattedAmount": 25,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        }
        // Other token transfers from betashop.eth on Ethereum
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Token Transfers Received By A User

You can fetch all token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS), on Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains) by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
For fetching token transfers data from multiple chains, check out [Cross-Chain Queries](basics/cross-chain-queries.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/d0vekk7Ako" %}
Show me token transfers received by betashop.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: ethereum,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "ETHGlobal Pragma Lisbon"
          }
        },
        {
          "formattedAmount": 0.00005,
          "tokenType": "ERC20",
          "token": {
            "name": "Wrapped Ether"
          }
        }
        // Other Token Transfers received by betashop.eth on Ethereum
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Most Recent Token Transfers Sent From A User(s)

You can fetch all most recent token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS), across multiple chains, such as Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains), by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
For fetching most recent token transfers data from multiple chains, check out [Cross-Chain Queries](basics/cross-chain-queries.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/U8PmPyhVIS" %}
Show me the most recent token transfers sent from betashop.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: { _eq: "betashop.eth" }
</strong>      }
      blockchain: ethereum
      limit: 50
<strong>      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
</strong>    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 125,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        }
        // Other most recent token transfers sent by betashop.eth on Ethereum
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Most Recent Token Transfers Received By A User

You can fetch most recent token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS), across multiple chains, such as Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains), by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

{% hint style="info" %}
For fetching most recent token transfers data from multiple chains, check out [Cross-Chain Queries](basics/cross-chain-queries.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/UJGlgZ8T6f" %}
Show me the most recent token transfers received by betashop.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>      },
      blockchain: ethereum,
      limit: 50,
<strong>      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
</strong>    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1250,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        }
        // Other most recent token transfers received by betashop.eth on Ethereum
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching token transfers data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [TokenTransfers API Reference](../api-references/api-reference/tokentransfers-api.md)
- [On-Chain Graph](onchain-graph.md)
- [Token Mints Guides](token-mints.md)
