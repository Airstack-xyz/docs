---
description: >-
  Learn how to fetch ERC20 token details data, which includes name, symbol, logo
  images (original & resized), and total supply.
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

# 📑 ERC20 Details

[Airstack](https://airstack.xyz) provides easy-to-use ERC20 token APIs for enriching Web3 applications with onchain and offchain ERC20 token data from Ethereum, Gold, Base, and Zora.

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get ERC20 Token Total Supply](erc20-details.md#get-erc20-token-total-supply)
- [Get ERC20 Token Name & Symbol](erc20-details.md#get-erc20-token-name-and-symbol)
- [Get ERC20 Token Logo Images](erc20-details.md#get-erc20-token-logo-images)

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

## Get ERC20 Token Total Supply

You can fetch an ERC20 token's total supply by using the `totalSupply` field from [`Tokens`](../../api-references/api-reference/tokens-api.md) API by providing ERC20 token contract `address` as an input:

### Try Code

{% embed url="https://app.airstack.xyz/query/JN3pfhlTBk" %}
Show me total supply of @Wrapped Ether
{% endembed %}

### Demo

{% tabs %}
{% tab title="Query" %}

```graphql
query TotalSupply {
  Tokens(
    input: {
      filter: { address: { _eq: "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2" } }
      blockchain: ethereum
    }
  ) {
    Token {
      totalSupply
      decimals
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "Tokens": {
      "Token": [
        {
          "totalSupply": "3235848667496664235958043",
          "decimals": 18
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get ERC20 Token Name & Symbol

You can fetch an ERC20 token's name and symbol by using the `name` and `symbol` fields from [`Tokens`](../../api-references/api-reference/tokens-api.md) API by providing ERC20 token contract `address` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4XZZgtB1OW" %}
Show me the name and symbol of @Wrapped Ether
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Tokens(
    input: {
      filter: { address: { _eq: "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2" } }
      blockchain: ethereum
    }
  ) {
    Token {
      name
      symbol
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "Tokens": {
      "Token": [
        {
          "name": "Wrapped Ether",
          "symbol": "WETH"
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get ERC20 Token Logo Images

You can fetch all ERC20 token logo images by using the [`Tokens`](../../api-references/api-reference/tokens-api.md) API by providing ERC20 token contract `address` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/xnkNqfV6Pt" %}
Show me the the logo image of @Wrapped Ether
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Tokens(
    input: {
      filter: { address: { _eq: "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2" } }
      blockchain: ethereum
    }
  ) {
    Token {
      projectDetails {
        imageUrl
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
    "Tokens": {
      "Token": [
        {
          "projectDetails": {
            "imageUrl": "https://assets.airstack.xyz/image/project-image/a5f3V5sauNM0Tw8EXERgdB2s98Wt1ntSoJJ/GNKh0nDl4qUGVQO6pHSKpTJY1gcL/original_image.png"
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching ERC20 details data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [ERC20 Balances](erc20-balances.md)
- [ERC20 Holders](erc20-holders.md)
- [Combinations (Common Holders)](../combinations/)
  - [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  - [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
- [ERC20 Tokens In Common](../tokens-in-common/erc20s.md)
- [Tokens API Reference](../../api-references/api-reference/tokens-api.md)
