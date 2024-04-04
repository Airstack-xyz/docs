---
description: >-
  Learn how to use Airstack to fetch all NFT mints data, from or to user(s),
  across Ethereum, Base, and Zora.
---

# 👛 NFT Mints

All tokens minted are essentially NFT transfers from a [null address (0x00...00)](https://explorer.airstack.xyz/token-balances?address=0x0000000000000000000000000000000000000000\&rawInput=%23%E2%8E%B10x0000000000000000000000000000000000000000%E2%8E%B1%280x0000000000000000000000000000000000000000+ADDRESS+ethereum+null%29\&inputType=ADDRESS) to a user address that are executed by the receiving user itself. Thus, with [Airstack](https://airstack.xyz), you can use the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API to fetch all user's token mints by specifying the input as follows:

<table><thead><tr><th width="176">Input Filter</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>operator</code></td><td>user's 0x address, ENS, cb.id, Lens, or Farcaster</td><td>Executor of the transaction.</td></tr><tr><td><code>from</code></td><td><a href="https://explorer.airstack.xyz/token-balances?address=0x0000000000000000000000000000000000000000&#x26;rawInput=%23%E2%8E%B10x0000000000000000000000000000000000000000%E2%8E%B1%280x0000000000000000000000000000000000000000+ADDRESS+ethereum+null%29&#x26;inputType=ADDRESS"><code>0x0000000000000000000000000000000000000000</code></a></td><td>Sender in the ERC721/1155 token transfers.</td></tr><tr><td><code>to</code></td><td>user's 0x address, ENS, cb.id, Lens, or Farcaster</td><td>Receiver in the ERC721/1155 token transfers.</td></tr></tbody></table>

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to [get NFT mints by a user](nft-mints.md#get-nft-mints-by-a-user).

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL

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

## Get NFT Mints By A User

You can fetch all NFTs minted by a user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Base, and Zora, by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/wY674s1t90" %}
Show me all NFTs minted by betashop.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Ethereum: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Get only NFTs (ERC721/1155)
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>      },
      blockchain: ethereum,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      tokenId
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
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Get only NFTs (ERC721/1155)
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>      },
      blockchain: base,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      tokenId
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
```json
{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "blockchain": "ethereum",
          "formattedAmount": 1,
          "tokenAddress": "0x0f92612c5f539c89dc379e8aa42e3ba862a34b7e",
          "tokenId": "8",
          "tokenNft": {
            "metaData": {
              "name": "Venture Club Alpha Membership NFT"
            },
            "contentValue": {
              "image": {
                "medium": "https://assets.uat.airstack.xyz/image/nft/li5d4XIGDPxtahI+EMjNmOiSymdFmCR0OsRC2p13nDaZQijmEYVpYlbV0t57GD8K/medium.jpg"
              }
            }
          },
          "tokenType": "ERC721"
        }
        // Other NFTs minted by betashop.eth on Ethereum
      ]
    },
    "Base": {
      "TokenTransfer": [
        {
          "blockchain": "base",
          "formattedAmount": 1,
          "tokenAddress": "0xba5e05cb26b78eda3a2f8e3b3814726305dcac83",
          "tokenId": "118",
          "tokenNft": {
            "metaData": {
              "name": null
            },
            "contentValue": {
              "image": null
            }
          },
          "tokenType": "ERC1155"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching NFT mints data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api.md)
* [On-Chain Graph](../onchain-graph.md)
* [TokenTransfers Guides](../token-transfers.md)
* [Token Mints Guides](../token-mints.md)
* [NFT Details](nft-details.md)
* [NFT Balances](nft-balances.md)
* [NFT Holders](nft-holders.md)
* [Spam NFT](spam-nft.md)
