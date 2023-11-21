---
description: >-
  Learn how to use Airstack to filter out spam NFTs and check whether an NFT is
  a spam or not.
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

# 🧹 Spam NFT

[Airstack](https://airstack.xyz) provides easy-to-use NFT APIs for enriching Web3 applications with onchain and offchain NFT data from Ethereum and Polygon.

When indexing any new token, Airstack takes into consideration contract deployer address history, and other factors to determine if the token might be spam.

Those contracts are the marked spam and can be filtered out. If you think that a given contract was incorrectly labeled as spam, please reach out to us [support@airstack.xyz](mailto:support@airstack.xyz).

In this guide you will learn how to use Airstack to:

* [Check If NFT Collection(s) Are Spam Or Not](spam-nft.md#check-if-nft-collection-s-are-spam-or-not)
* [Show All Non-Spam NFTs](spam-nft.md#show-all-non-spam-nfts)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
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

## Check If NFT Collection(s) Are Spam Or Not

You can use [Airstack](https://airstack.xyz) to check if NFT collection(s) are spam or not by using [`Tokens`](../../api-references/api-reference/tokens-api/) API and providing the NFT collection address(es) to `address` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/yprF4nN1wf" %}
Check if @BoredApeYachtClub, @Ethereum Name Service, and @Pudgy Penguins are spam NFTs or not
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Tokens(
    input: {filter: {type: {_in: [ERC1155, ERC721]}, address: {_in: ["0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D", "0x57f1887a8BF19b14fC0dF6Fd9B2acc9Af147eA85", "0xBd3531dA5CF5857e7CfAA92426877b022e612cf8"]}}, blockchain: ethereum}
  ) {
    Token {
      isSpam
      address
      name
      symbol
      type
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Tokens": {
      "Token": [
        {
<strong>          "isSpam": false, // not spam NFT
</strong>          "address": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
          "name": "Ethereum Name Service",
          "symbol": "ENS",
          "type": "ERC721"
        },
        {
<strong>          "isSpam": false, // not spam NFT
</strong>          "address": "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d",
          "name": "BoredApeYachtClub",
          "symbol": "BAYC",
          "type": "ERC721"
        },
        {
<strong>          "isSpam": false, // not spam NFT
</strong>          "address": "0xbd3531da5cf5857e7cfaa92426877b022e612cf8",
          "name": "PudgyPenguins",
          "symbol": "PPG",
          "type": "ERC721"
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Show All Non-Spam NFTs

You can use [Airstack](https://airstack.xyz) to fetch all existing non-spam NFTs across Ethereum and Polygon:

### Try Demo

{% embed url="https://app.airstack.xyz/query/TsCYfuhZ5D" %}
Show all non-spam NFTs on Ethereum and Polygon
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Ethereum: Tokens(
    input: {filter: {isSpam: {_eq: false}, type: {_in: [ERC1155, ERC721]}}, blockchain: ethereum}
  ) {
    Token {
      isSpam
      address
      name
      symbol
      type
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
  Polygon: Tokens(
    input: {filter: {isSpam: {_eq: false}, type: {_in: [ERC1155, ERC721]}}, blockchain: polygon}
  ) {
    Token {
      isSpam
      address
      name
      symbol
      type
    }
    pageInfo {
      hasNextPage
      hasPrevPage
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
    "Ethereum": {
      "Token": [
        {
          "isSpam": false,
          "address": "0x8754f54074400ce745a7ceddc928fb1b7e985ed6",
          "name": "EulerBeats",
          "symbol": "eBEATS",
          "type": "ERC1155"
        },
        {
          "isSpam": false,
          "address": "0x0000000000000138bd6bd34cf4a3905576f58e25",
          "name": "OpenAvatar",
          "symbol": "AVATR",
          "type": "ERC721"
        },
        {
          "isSpam": false,
          "address": "0x00000000000009ca62911fdd88c8cf50565ab183",
          "name": "Frensville Treasury Token",
          "symbol": "FTTKN",
          "type": "ERC721"
        },
        // Other non-spam Ethereum NFTs
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweDAwMDExOTZmMzM5NjNiMWUzYTkzODM2ODg3YTgwODBkZjM4ODg4ODgiLCJEYXRhVHlwZSI6InN0cmluZyJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    },
    "Polygon": {
      "Token": [
        {
          "isSpam": false,
          "address": "0x000000000437b3cce2530936156388bff5578fc3",
          "name": "My NFT",
          "symbol": "NFT",
          "type": "ERC721"
        },
        {
          "isSpam": false,
          "address": "0x0000000009bba016bf81a230372961866ae7e6be",
          "name": "Biteye Course Ticket: Lens",
          "symbol": "BLENS",
          "type": "ERC721"
        },
        {
          "isSpam": false,
          "address": "0x000000009eab12592db703bfe50db40f3144aa91",
          "name": "LuckySea.gg",
          "symbol": "luckysea_nft",
          "type": "ERC721"
        },
        // Other non-spam Polygon NFTs
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEzNzB4MDAwMTc0YTQzOWMyYzdkYjZmNWExMzEyNWE3NmE3NjdlMDJkOTgyZiIsIkRhdGFUeXBlIjoic3RyaW5nIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching NFT details data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [NFT Details](nft-details.md)
* [NFT Balances](nft-balances.md)
* [NFT Holders](nft-holders.md)
* [Combinations (Common Holders)](../combinations/)
  * [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
* [Tokens In Common](../tokens-in-common/)
  * [NFTs](../tokens-in-common/nfts.md)
* [Tokens API Reference](../../api-references/api-reference/tokens-api/)
* [TokenNfts API Reference](../../api-references/api-reference/tokennfts-api/)
