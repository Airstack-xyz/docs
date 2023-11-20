---
description: >-
  Learn how to fetch NFT holders of a specific NFT and NFT collection(s) and how
  to use the same query to check whether a given user holds a specific NFT
  collection.
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

# 🗝 NFT Holders

[Airstack](https://airstack.xyz) provides easy-to-use NFT APIs for enriching Web3 applications with onchain and offchain NFT data from Ethereum and Polygon.

In this tutorial, you will learn how to fetch NFT holders of a specific NFT and NFT collection(s). You will also learn how to use the same query to check whether a given user hold a specific NFT collection, which can be a useful application for NFT-gating.

In this guide you will learn how to use Airstack to:

* [Get NFT Holder(s) of A Specific NFT](nft-holders.md#get-nft-holder-s-of-a-specific-nft)
* [Get NFT Holders of NFT Collection(s)](nft-holders.md#get-nft-holders-of-nft-collection-s)
* [Check If User Hold A Specific NFT Collection](nft-holders.md#check-if-user-hold-a-specific-nft-collection)

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

## Get NFT Holder(s) of A Specific NFT

You can use [Airstack](https://airstack.xyz) to fetch NFT holder(s) of a specifc NFT by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API and providing the NFT collection address(es) to `tokenAddress` and the token ID to `tokenId` as an input:

{% hint style="info" %}
For ERC721 NFT, there's only **1 unique holder** for each token ID.

For ERC1155 NFT, there can be **multiple holders** for a token ID.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/djL8dtRBMJ" %}
Show me holder of @BoredApeYachtClub token ID 6891
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}, tokenId: {_eq: "6891"}}, blockchain: ethereum}
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
            "addresses": [
              "0x9831bb48e27a6b74260823c10d15b577e891a37b"
            ],
            "domains": [
              {
                "name": "dhtong.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "dht",
                "userAssociatedAddresses": [
                  "0x6e2946ec7cc8b9473749319ace351c5e33f04960",
                  "0x9831bb48e27a6b74260823c10d15b577e891a37b"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
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

## Get NFT Holders of NFT Collection(s)

You can use [Airstack](https://airstack.xyz) to fetch NFT holders of a given NFT collection(s) by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API and providing the NFT collection address(es) to `tokenAddress` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/pcQj75ZyTz" %}
Show me holders of @BoredApeYachtClub
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: ["0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      tokenId
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
          "tokenId": "6891",
          "owner": {
            "addresses": [
              "0x9831bb48e27a6b74260823c10d15b577e891a37b"
            ],
            "domains": [
              {
                "name": "dhtong.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "dht",
                "userAssociatedAddresses": [
                  "0x6e2946ec7cc8b9473749319ace351c5e33f04960",
                  "0x9831bb48e27a6b74260823c10d15b577e891a37b"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
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

## Check If User Hold A Specific NFT Collection

You can use [Airstack](https://airstack.xyz) to check if a user hold a given NFT Collection by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API.

For inputs, provide the NFT collection address(es) to `tokenAddress` and user's 0x address, ENS domain, cb.id, Lens profile, or Farcaster fname/fid to `owner` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/gFu81dII6S" %}
Check if 0x9831bb48e27a6b74260823c10d15b577e891a37b hold any @BoredApeYachtClub
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}, owner: {_eq: "0x9831bb48e27a6b74260823c10d15b577e891a37b"}}, blockchain: ethereum}
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
            "addresses": [
              "0x9831bb48e27a6b74260823c10d15b577e891a37b"
            ],
            "domains": [
              {
                "name": "dhtong.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "dht",
                "userAssociatedAddresses": [
                  "0x6e2946ec7cc8b9473749319ace351c5e33f04960",
                  "0x9831bb48e27a6b74260823c10d15b577e891a37b"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
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

## Developer Support

If you have any questions or need help regarding fetching NFT details data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [NFT Details](nft-details.md)
* [NFT Balances](nft-balances.md)
* [Combinations (Common Holders)](../combinations/)
  * [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
* [Tokens In Common](../tokens-in-common/)
  * [NFTs](../tokens-in-common/nfts.md)
* [Tokens API Reference](../../api-references/api-reference/tokens-api/)
* [TokenNfts API Reference](../../api-references/api-reference/tokennfts-api/)
