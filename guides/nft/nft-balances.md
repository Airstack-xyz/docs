---
description: >-
  Learn how to fetch ERC721 and ERC1155 NFT balances of user(s) on Ethereum,
  Polygon, and Base with their variations.
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

# ⚖ NFT Balances

[Airstack](https://airstack.xyz) provides easy-to-use NFT APIs for enriching Web3 applications with onchain and offchain NFT data from Ethereum, Polygon, and Base.

### Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get NFT Balances of User(s)](nft-balances.md#get-nft-balances-of-user-s)
* [Get NFT Balances of User(s) on Specific Chain](nft-balances.md#get-nft-balances-of-user-s-on-specific-chain)
* [Get Only ERC721/1155 NFT Balances of User(s)](nft-balances.md#get-only-erc721-1155-nft-balances-of-user-s)
* [Get NFT Balances of User(s) on Specific NFT Collection(s)](nft-balances.md#get-nft-balances-of-user-s-on-specific-nft-collection-s)

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

## Get NFT Balances of User(s)

You can use [Airstack](https://airstack.xyz) to fetch NFT balance of user(s) across both Ethereum, Polygon, and Base by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API and providing an array of users' 0x address, ENS domain, cb.id, Lens profile, or Farcaster fname/fid to `owner` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/mIoE6fBEIo" %}
Show me the NFT balances of users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      owner {
        identity
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Polygon: TokenBalances(
    input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: polygon, limit: 50}
  ) {
    TokenBalance {
      owner {
        identity
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Base: TokenBalances(
    input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: base, limit: 50}
  ) {
    TokenBalance {
      owner {
        identity
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
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
    "Ethereum": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "amount": "1",
          "tokenAddress": "0xe361f10965542ee57d39043c9c3972b77841f581",
          "tokenId": "2575",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/large.png",
                "original": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/original_image.png"
              }
            }
          }
        },
        // Other Ethereum NFTs held by vitalik
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweDM0M2Y5OTllYWFjZGZhMWYyMDFmYjhlNDNlYmIzNWM5OWQ5YWUwYzEweGQ4ZGE2YmYyNjk2NGFmOWQ3ZWVkOWUwM2U1MzQxNWQzN2FhOTYwNDU1NTM4IiwiRGF0YVR5cGUiOiJzdHJpbmcifSwibGFzdFVwZGF0ZWRUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjk0NTc4MjgzIiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    },
    "Polygon": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "amount": "1",
          "tokenAddress": "0xfc4c0a71f7eaa1c8cd4044b8a4808ab1c2dca103",
          "tokenId": "14",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/large.png",
                "original": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/original_image.png"
              }
            }
          }
        },
        // Other Polygon NFTs held by vitalik
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImMzMjllODM2ZWE0MzU2ZGViZWU1ZDc3MTg5MjlhNWRiYWJkZjhjNGEwYjJmMDY2M2Y1NmQxOTU2YzlkNzViNWYiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTg5ODA2MjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "amount": "1",
          "tokenAddress": "0xab54b8bd1118d535beecf43bf9c7d163879cf967",
          "tokenId": "2",
          "tokenType": "ERC1155",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8453/KEYM3ihZQ1f7hNMrnxylaC9aymzRDEpR6k24WvE1bOeYTCP3FBEY8v0XMCaJUXyzqno4Vvx93EOQrbiH+i4aKQ==/extra_small.jpg",
                "small": "https://assets.airstack.xyz/image/nft/8453/KEYM3ihZQ1f7hNMrnxylaC9aymzRDEpR6k24WvE1bOeYTCP3FBEY8v0XMCaJUXyzqno4Vvx93EOQrbiH+i4aKQ==/small.jpg",
                "medium": "https://assets.airstack.xyz/image/nft/8453/KEYM3ihZQ1f7hNMrnxylaC9aymzRDEpR6k24WvE1bOeYTCP3FBEY8v0XMCaJUXyzqno4Vvx93EOQrbiH+i4aKQ==/medium.jpg",
                "large": "https://assets.airstack.xyz/image/nft/8453/KEYM3ihZQ1f7hNMrnxylaC9aymzRDEpR6k24WvE1bOeYTCP3FBEY8v0XMCaJUXyzqno4Vvx93EOQrbiH+i4aKQ==/large.jpg",
                "original": "https://assets.airstack.xyz/image/nft/8453/KEYM3ihZQ1f7hNMrnxylaC9aymzRDEpR6k24WvE1bOeYTCP3FBEY8v0XMCaJUXyzqno4Vvx93EOQrbiH+i4aKQ==/original_image.jpg"
              }
            }
          }
        },
        // Other Base NFTs held by vitalik
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImMzMjllODM2ZWE0MzU2ZGViZWU1ZDc3MTg5MjlhNWRiYWJkZjhjNGEwYjJmMDY2M2Y1NmQxOTU2YzlkNzViNWYiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTg5ODA2MjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get NFT Balances of User(s) on Specific Chain

You can use [Airstack](https://airstack.xyz) to fetch NFT balance of user(s) on a specific chain, e.g. Ethereum, by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API.

For the inputs, specify `blockchain` to `ethereum` and provide an array of users' 0x address, ENS domain, cb.id, Lens profile, or Farcaster fname/fid to `owner`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/fDo1Y5CJX0" %}
Show me NFT balances of users on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      owner {
        identity
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
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
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "amount": "1",
          "tokenAddress": "0xe361f10965542ee57d39043c9c3972b77841f581",
          "tokenId": "2575",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/large.png",
                "original": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/original_image.png"
              }
            }
          }
        },
        // Other Ethereum NFTs held by vitalik
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweDM0M2Y5OTllYWFjZGZhMWYyMDFmYjhlNDNlYmIzNWM5OWQ5YWUwYzEweGQ4ZGE2YmYyNjk2NGFmOWQ3ZWVkOWUwM2U1MzQxNWQzN2FhOTYwNDU1NTM4IiwiRGF0YVR5cGUiOiJzdHJpbmcifSwibGFzdFVwZGF0ZWRUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjk0NTc4MjgzIiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Only ERC721/1155 NFT Balances of User(s)

You can use [Airstack](https://airstack.xyz) to fetch only either ERC721 or ERC1155 NFT balance of user(s) by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API.

For the inputs, specify `tokenType` to `ERC721` and provide an array of users' 0x address, ENS domain, cb.id, Lens profile, or Farcaster fname/fid to `owner`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/saSFcX2XS5" %}
Show me only the ERC721 NFT balances of users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}, tokenType: {_in: [ERC721]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      owner {
        identity
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Polygon: TokenBalances(
    input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}, tokenType: {_in: [ERC721]}}, blockchain: polygon, limit: 50}
  ) {
    TokenBalance {
      owner {
        identity
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Base: TokenBalances(
    input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}, tokenType: {_in: [ERC721]}}, blockchain: base, limit: 50}
  ) {
    TokenBalance {
      owner {
        identity
      }
      amount
      tokenAddress
      tokenId
      tokenType
      tokenNfts {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
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
    "Ethereum": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "amount": "1",
          "tokenAddress": "0xe361f10965542ee57d39043c9c3972b77841f581",
          "tokenId": "2575",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/large.png",
                "original": "https://assets.airstack.xyz/image/nft/8f58Hf72fwHyRNcwwg4qm1s1lnMMqcTNPtigmujnbqExq9Wt5XW0JEqPWZQdV3hICnCYr250gkuHMB5a9H+nUw==/original_image.png"
              }
            }
          }
        },
        // Other ERC721 Ethereum NFTs held by vitalik
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweDM0M2Y5OTllYWFjZGZhMWYyMDFmYjhlNDNlYmIzNWM5OWQ5YWUwYzEweGQ4ZGE2YmYyNjk2NGFmOWQ3ZWVkOWUwM2U1MzQxNWQzN2FhOTYwNDU1NTM4IiwiRGF0YVR5cGUiOiJzdHJpbmcifSwibGFzdFVwZGF0ZWRUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjk0NTc4MjgzIiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    },
    "Polygon": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "amount": "1",
          "tokenAddress": "0xfc4c0a71f7eaa1c8cd4044b8a4808ab1c2dca103",
          "tokenId": "14",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/large.png",
                "original": "https://assets.airstack.xyz/image/nft/DXquuKVBgT0LZ59D3CvMbxW/Wgj+tOv6HxthIsOD94xJiicCTCtBPjgSY5DLpx9zdpHltoXSCqKIg9yEjZZE4Q==/original_image.png"
              }
            }
          }
        },
        // Other ERC721 Polygon NFTs held by vitalik
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImMzMjllODM2ZWE0MzU2ZGViZWU1ZDc3MTg5MjlhNWRiYWJkZjhjNGEwYjJmMDY2M2Y1NmQxOTU2YzlkNzViNWYiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTg5ODA2MjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xe3a2a53e6bc71bc5ec34313b5aa783f4733bc419"
          },
          "amount": "1",
          "tokenAddress": "0xf1788f434658347095a8e1a5db4414134d364d36",
          "tokenId": "5283",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8453/xeoepiJcK8a1aBbWMmG+g74XtIBrNqUqeB5RG51gQ1NIPUKy620oiRH9F+8JAUf5GgQ6A+cwcAaWsaRU6nXfcA==/extra_small.mp4",
                "small": "https://assets.airstack.xyz/image/nft/8453/xeoepiJcK8a1aBbWMmG+g74XtIBrNqUqeB5RG51gQ1NIPUKy620oiRH9F+8JAUf5GgQ6A+cwcAaWsaRU6nXfcA==/small.mp4",
                "medium": "https://assets.airstack.xyz/image/nft/8453/xeoepiJcK8a1aBbWMmG+g74XtIBrNqUqeB5RG51gQ1NIPUKy620oiRH9F+8JAUf5GgQ6A+cwcAaWsaRU6nXfcA==/medium.mp4",
                "large": "https://assets.airstack.xyz/image/nft/8453/xeoepiJcK8a1aBbWMmG+g74XtIBrNqUqeB5RG51gQ1NIPUKy620oiRH9F+8JAUf5GgQ6A+cwcAaWsaRU6nXfcA==/large.mp4",
                "original": "https://assets.airstack.xyz/image/nft/8453/xeoepiJcK8a1aBbWMmG+g74XtIBrNqUqeB5RG51gQ1NIPUKy620oiRH9F+8JAUf5GgQ6A+cwcAaWsaRU6nXfcA==/original_image.mp4"
              }
            }
          }
        },
        // Other ERC721 Base NFTs held by vitalik
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImMzMjllODM2ZWE0MzU2ZGViZWU1ZDc3MTg5MjlhNWRiYWJkZjhjNGEwYjJmMDY2M2Y1NmQxOTU2YzlkNzViNWYiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTg5ODA2MjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get NFT Balances of User(s) on Specific NFT Collection(s)

You can use [Airstack](https://airstack.xyz) to fetch NFT balance of user(s) on a specific NFT collection, e.g. [Ethereum Name Service](https://explorer.airstack.xyz/token-holders?activeView=\&address=0x57f1887a8BF19b14fC0dF6Fd9B2acc9Af147eA85\&tokenType=\&rawInput=%23%E2%8E%B1ENS%E2%8E%B1%280x57f1887a8BF19b14fC0dF6Fd9B2acc9Af147eA85+NFT\_COLLECTION+ethereum+null%29\&inputType=NFT\_COLLECTION\&activeTokenInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&activeSocialInfo=\&blockchain=ethereum), by using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API.

For the inputs, specify `tokenAddress` with the NFT collection address and provide an array of users' 0x address, ENS domain, cb.id, Lens profile, or Farcaster fname/fid to `owner`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/DthZQd4dl1" %}
Show me all ENS NFT held by users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x57f1887a8BF19b14fC0dF6Fd9B2acc9Af147eA85"}, owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}}, blockchain: ethereum}
  ) {
    TokenBalance {
      id
      chainId
      blockchain
      tokenAddress
      tokenId
      amount
      formattedAmount
      lastUpdatedBlock
      lastUpdatedTimestamp
      tokenType
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
          "id": "10x57f1887a8bf19b14fc0df6fd9b2acc9af147ea850xd8da6bf26964af9d7eed9e03e53415d37aa9604595546147854435590899330841447456946408949540847996520093152162339362490680613",
          "chainId": "1",
          "blockchain": "ethereum",
          "tokenAddress": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
          "tokenId": "95546147854435590899330841447456946408949540847996520093152162339362490680613",
          "amount": "1",
          "formattedAmount": 1,
          "lastUpdatedBlock": 17980604,
          "lastUpdatedTimestamp": "2023-08-23T23:10:59Z",
          "tokenType": "ERC721"
        },
        // Other ENS NFT held by vitalik
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching NFT balances data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [NFT Details](nft-details.md)
* [NFT Holders](nft-holders.md)
* [Spam NFT](spam-nft.md)
* [Combinations (Common Holders)](../combinations/)
  * [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
* [Tokens In Common](../tokens-in-common/)
  * [NFTs](../tokens-in-common/nfts.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
