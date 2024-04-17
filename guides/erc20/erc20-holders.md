---
description: >-
  Learn how to fetch ERC20 token holders of a specific contract address(es) and
  how to use the same query to check whether a given user holds a specific ERC20
  token.
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

# 🗝️ ERC20 Holders

[Airstack](https://airstack.xyz) provides easy-to-use ERC20 token APIs for enriching Web3 applications with onchain and offchain ERC20 token data from Ethereum, Base, Zora, and other [Airstack supported chains](../overview.md#supported-chains).

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get ERC20 Token Holders](erc20-holders.md#get-erc20-token-holders)
* [Check If User Hold A Specific ERC20 Token](erc20-holders.md#check-if-user-hold-a-specific-erc20-token)

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

## Get ERC20 Token Holders

You can use [Airstack](https://airstack.xyz) to fetch ERC20 token holders of a given contract address(es) by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api.md) API and providing the token contract address(es) to `tokenAddress` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/EDX15sLWg1" %}
Show me holders of @Wrapped Ether
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: { _in: ["0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"] }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    TokenBalance {
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
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

{% tab title="Response" %}
```json
 {
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "addresses": [
              "0xdef1c0ded9bec7f1a1670819833240f027b25eff"
            ],
            "domains": [
              {
                "name": "0x.merklejerk.eth",
                "isPrimary": false
              },
              {
                "name": "zeroex.eth",
                "isPrimary": false
              }
            ],
            "socials": null,
            "xmtp": null
          }
        },
        // Other WETH holders
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJjNGNhMGVkYTc2NDdhOGFiN2MyMDYxYzJlMTE4YTE4YTkzNmYxM2QweGFhYTJkYTI1NWRmOWVlNzRjNzA3NWJjYjZkODFmOTc5NDA5MDhhNWQxNTYiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE3MDA0MTA5MTkiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
]
```
{% endtab %}
{% endtabs %}

## Check If User Hold A Specific ERC20 Token

You can use [Airstack](https://airstack.xyz) to check if a user hold a given ERC20 token by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api.md) API.

For inputs, provide the token contract address(es) to `tokenAddress` and user's 0x address, ENS domain, cb.id, Lens profile, or Farcaster fname/fid to `owner` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/kp1lhkNlLq" %}
Check if 0xdef1c0ded9bec7f1a1670819833240f027b25eff hold any Wrapped Ether
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: { _eq: "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2" }
        owner: { _eq: "0xdef1c0ded9bec7f1a1670819833240f027b25eff" }
      }
      blockchain: ethereum
    }
  ) {
    TokenBalance {
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "addresses": ["0xdef1c0ded9bec7f1a1670819833240f027b25eff"],
            "domains": [
              {
                "name": "0x.merklejerk.eth",
                "isPrimary": false
              },
              {
                "name": "zeroex.eth",
                "isPrimary": false
              }
            ],
            "socials": null,
            "xmtp": null
          }
        }
      ],
      "pageInfo": {
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

If the given user hold the specified ERC20 token, then `TokenBalances` will have non-null value as a response. Otherwise, the API will return null.

## Developer Support

If you have any questions or need help regarding fetching ERC20 holders data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Balance Snapshots Guides](../balance-snapshots.md)
* [Holder Snapshots Guides](../holder-snapshots.md)
* [ERC20 Token Balances](erc20-balances.md)
* [Combinations (Common Holders)](../combinations/)
  * [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
* [ERC20 Tokens In Common](../tokens-in-common/erc20s.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)
