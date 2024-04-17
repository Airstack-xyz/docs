---
description: Learn how to fetch NFTs in common from multiple users.
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

# ♦️ NFTs

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching dapps and integrating on-chain and off-chain data from Ethereum, Base, Zora, and other [Airstack-supported chains](../overview.md#supported-chains).&#x20;

In this tutorial, you will learn how to fetch NFTs in common from multiple users.

In this guide, you will learn how to use Airstack to fetch [NFTs In Common](nfts.md#nfts-in-common).

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

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);

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
import { init, fetchQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const { data, error } = fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```

{% endtab %}

{% tab title="Python" %}

```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = "YOUR_QUERY" # Replace with GraphQL Query

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

## NFTs In Common

You can fetch common NFTs of multiple user(s) provided the user's 0x address, Farcaster name or ID, ENS domains, or Lens profile:

### Try Demo

{% embed url="https://app.airstack.xyz/query/83iNwAcifb" %}
Show common NFTs of two users on Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _eq: "betashop.eth" }
        tokenType: { _in: [ERC721, ERC1155] }
      }
      blockchain: base
    }
  ) {
    TokenBalance {
      token {
        tokenBalances(
          input: {
            filter: {
              owner: { _eq: "tokenstaker.eth" }
              tokenType: { _in: [ERC721, ERC1155] }
            }
          }
        ) {
          id
          token {
            address
            symbol
            name
          }
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
          "token": {
            "tokenBalances": [
              {
                "id": "09adff7901e6b3ed411a037abf0be67365cb2dabe24e48921768d32abf64203a",
                "token": {
                  "address": "0x4c17ff12d9a925a0dec822a8cbf06f46c626855c",
                  "symbol": "FC-HZN",
                  "name": "Farcaster Horizon"
                }
              }
            ]
          }
        },
        {
          "token": {
            "tokenBalances": [] // Owned by betashop.eth, but not owned by tokenstaker.eth
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

If you have any questions or need help regarding fetching common NFTs of multiple users, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Nested Queries](../../api-references/overview/nested-queries.md)
- [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)
- [Tokens In Common For Lens Developers](../lens/tokens-in-common.md)
- [Tokens In Common For Farcaster Developers](../../farcaster/farcaster/tokens-in-common.md)
- [Balance Snapshots Guides](../balance-snapshots.md)
- [Holder Snapshots Guides](../holder-snapshots.md)
