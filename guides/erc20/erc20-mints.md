---
description: >-
  Learn how to use Airstack to fetch all ERC20 token mints data, from or to
  user(s), across Ethereum, Base, Zora, and other Airstack-supported chains.
---

# 👛 ERC20 Mints

All ERC20 tokens minted are essentially ERC20 token transfers from a [null address (0x00...00)](https://explorer.airstack.xyz/token-balances?address=0x0000000000000000000000000000000000000000&rawInput=%23%E2%8E%B10x0000000000000000000000000000000000000000%E2%8E%B1%280x0000000000000000000000000000000000000000+ADDRESS+ethereum+null%29&inputType=ADDRESS) to a user address that are executed by the receiving user itself. Thus, with [Airstack](https://airstack.xyz), you can use the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API to fetch all user's ERC20 token mints by specifying the input as follows:

<table><thead><tr><th width="176">Input Filter</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>operator</code></td><td>user's 0x address, ENS, cb.id, Lens, or Farcaster</td><td>Executor of the transaction.</td></tr><tr><td><code>from</code></td><td><a href="https://explorer.airstack.xyz/token-balances?address=0x0000000000000000000000000000000000000000&#x26;rawInput=%23%E2%8E%B10x0000000000000000000000000000000000000000%E2%8E%B1%280x0000000000000000000000000000000000000000+ADDRESS+ethereum+null%29&#x26;inputType=ADDRESS"><code>0x0000000000000000000000000000000000000000</code></a></td><td>Sender in the ERC20 token transfers.</td></tr><tr><td><code>to</code></td><td>user's 0x address, ENS, cb.id, Lens, or Farcaster</td><td>Receiver in the ERC20 token transfers.</td></tr></tbody></table>

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to [get ERC20 token mints by a User.](erc20-mints.md#get-erc20-token-mints-by-a-user)

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

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get ERC20 Token Mints By A User

You can fetch all ERC20 tokens minted by a user, e.g. [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS), across multiple chains, such as Ethereum, Base, Zora, and other [Airstack-supported chains](../overview.md#supported-chains), by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/lpLjUCQZmI" %}
Show me all ERC20 tokens minted by ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Get only ERC20 tokens
<strong>        tokenType: {_eq: ERC20},
</strong>      },
      blockchain: ethereum,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      tokenNft {
        metaData {
          name
        }
        contentValue {
          image {
            medium
          }
        }
      }
      tokenType
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
<strong>      "TokenTransfer": null // No ERC20 token minted on Ethereum
</strong>    },
    "Polygon": {
      "TokenTransfer": [
        {
          "blockchain": "polygon",
          "formattedAmount": 0.01697896348647009,
          "tokenAddress": "0xadbf1854e5883eb8aa7baf50705338739e558e5b",
          "tokenNft": null,
          "tokenType": "ERC20"
        },
        // Other ERC20 tokens minted by ipeciura.eth on Polygon
      ]
    },
    "Base": {
<strong>      "TokenTransfer": null // No ERC20 token minted on Base
</strong>    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching ERC20 token mints data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api.md)
- [On-Chain Graph](../onchain-graph.md)
- [TokenTransfers Guides](../token-transfers.md)
- [Token Mints Guides](../token-mints.md)
- [ERC20 Details](erc20-details.md)
- [ERC20 Balances](erc20-balances.md)
- [ERC20 Holders](erc20-holders.md)
- [Spam ERC20](spam-erc20.md)
