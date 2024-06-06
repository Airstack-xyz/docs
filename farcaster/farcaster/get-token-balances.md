---
description: >-
  Learn how to get ERC20, 721, 1155, and POAPs of Farcaster User(s), including
  images and metadata, on Ethereum, Base, Degen L3, Gnosis (POAPs), and other
  Airstack-supported chains.
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

# 💰 Get Token Balances

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and for integrating onchain and offchain data with Farcaster.

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get Ethereum ERC20s Owned By Farcaster user(s)](get-token-balances.md#get-ethereum-erc20s-owned-by-farcaster-user-s)
- [Get Base ERC20s Owned By Farcaster user(s)](get-token-balances.md#get-base-erc20s-owned-by-farcaster-user-s)
- [Get All ERC20s Owned By Farcaster user(s)](get-token-balances.md#get-all-erc20s-owned-by-farcaster-user-s)
- [Get Ethereum NFTs Owned By Farcaster user(s)](get-token-balances.md#get-ethereum-nfts-owned-by-farcaster-user-s)
- [Get Base NFTs Owned By Farcaster user(s)](get-token-balances.md#get-base-nfts-owned-by-farcaster-user-s)
- [Get All NFTs Owned By Farcaster user(s)](get-token-balances.md#get-all-nfts-owned-by-farcaster-user-s)
- [Get All POAPs Owned By Farcaster user(s)](get-token-balances.md#get-all-poaps-owned-by-farcaster-user-s)

### Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL

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

## Get Ethereum ERC20s Owned By Farcaster user(s)

You can fetch all ERC20 tokens on Ethereum owned by any Farcaster user(s) using either their Farcaster names or IDs:

### Try Demo

{% embed url="https://app.airstack.xyz/query/5QXHPew792" %}
Show ERC20 tokens on Ethereum owned by Farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query ERC20sOwnedByFarcasterUser {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      token {
        name
        symbol
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
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79"
                ]
              },
              {
                "profileName": "",
                "userId": "188133",
                "userAssociatedAddresses": [
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "amount": "151869896",
          "tokenAddress": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
          "token": {
            "name": "USD Coin",
            "symbol": "USDC"
          }
        }
        // Other Ethereum ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Base ERC20s Owned By Farcaster user(s)

You can fetch all ERC20 tokens on Base owned by any Farcaster user(s) using either their Farcaster names or IDs:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OaZlBhKXQ4" %}
Show ERC20 tokens on Base owned by Farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query ERC20sOwnedByFarcasterUser {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      token {
        name
        symbol
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
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79"
                ]
              },
              {
                "profileName": "",
                "userId": "188133",
                "userAssociatedAddresses": [
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "amount": "1000000000000000000",
          "tokenAddress": "0xdce2eb77135bc3c2eeaa00306e664741e2a6bcca",
          "token": {
            "name": "TedUnlonelyToken",
            "symbol": "TED"
          }
        }
        // Other Base ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All ERC20s Owned By Farcaster user(s)

You can fetch all ERC20 tokens on Ethereum, Base, and Zora owned by any Farcaster user(s) using either their Farcaster names or IDs:

### Try Demo

{% embed url="https://app.airstack.xyz/query/hWpBlirpGn" %}
Show ERC20 tokens on Ethereum and Base owned by Farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query ERC20sOwnedByFarcasterUser {
  Ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      token {
        name
        symbol
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Base: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      amount
      tokenAddress
      token {
        name
        symbol
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
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          }
          "amount": "1000000000000000000",
          "tokenAddress": "0x502527431b8f5d3cde71569ea810d19441952b48",
          "token": {
            "name": "TedToken",
            "symbol": "TED"
          }
        },
        // Other Ethereum ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79"
                ]
              },
              {
                "profileName": "",
                "userId": "188133",
                "userAssociatedAddresses": [
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "amount": "1000000000000000000",
          "tokenAddress": "0xdce2eb77135bc3c2eeaa00306e664741e2a6bcca",
          "token": {
            "name": "TedUnlonelyToken",
            "symbol": "TED"
          }
        },
        // Other Base ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Ethereum NFTs Owned By Farcaster user(s)

You can fetch all NFTs on Ethereum owned by any Farcaster user(s) using either their Farcaster names or IDs:

### Try Demo

{% embed url="https://app.airstack.xyz/query/dRmGldKog8" %}
Show NFT on Ethereum owned by farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query NFTsOwnedByFarcasterUser {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
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
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e",
          "tokenId": "1",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/extra_small.jpg",
                "small": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/small.jpg",
                "medium": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/medium.jpg",
                "large": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/large.jpg"
              }
            }
          }
        }
        // Other Ethereum NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Base NFTs Owned By Farcaster user(s)

You can fetch all NFTs on Base owned by any Farcaster user(s) using either their Farcaster names or IDs:

### Try Demo

{% embed url="https://app.airstack.xyz/query/mV7k1lYLjJ" %}
Show NFT on Base owned by farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query NFTsOwnedByFarcasterUser {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
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
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0xf63f652daf6c328544d40b3ed146c8248687c1d1",
          "tokenId": "7",
          "tokenType": "ERC1155",
          "tokenNfts": {
            "contentValue": {
              "image": null
            }
          }
        }
        // Other Base NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All NFTs Owned By Farcaster user(s)

You can fetch all NFTs on Ethereum, Base, and Zora owned by any Farcaster user(s) using either their Farcaster names or IDs:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ImmNakuUP1" %}
Show NFT on Ethereum and Base owned by farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query NFTsOwnedByFarcasterUser {
  Ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
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
    input: {
      filter: {
        owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
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
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e",
          "tokenId": "1",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/extra_small.jpg",
                "small": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/small.jpg",
                "medium": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/medium.jpg",
                "large": "https://assets.airstack.xyz/image/nft/1/0x1538c5ddbb073638b7cd1ae41ec2d9f9a4c24a7e/1/large.jpg"
              }
            }
          }
        }
        // Other Ethereum NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0xf63f652daf6c328544d40b3ed146c8248687c1d1",
          "tokenId": "7",
          "tokenType": "ERC1155",
          "tokenNfts": {
            "contentValue": {
              "image": null
            }
          }
        }
        // Other Base NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJkODI0MGMyM2VjOThiMjFmMzhjNWQzZTFlZWE5NTdhYjgwZGE5Y2EweGQ3MDI5YmRlYTFjMTc0OTM4OTNhYWZlMjlhYWQ2OWVmODkyYjhmZjIiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2NDc1MjIyNDYiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All POAPs Owned By Farcaster user(s)

You can fetch all POAPs tokens owned by any Farcaster user(s) using either their Farcaster names or IDs:

### Try Demo

{% embed url="https://app.airstack.xyz/query/9gNjxdNWsP" %}
Show POAPs owned by Farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query POAPsOwnedByFarcasterUser {
  Poaps(
    input: {
      filter: { owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] } }
      blockchain: ALL
    }
  ) {
    Poap {
      eventId
      poapEvent {
        eventName
        eventURL
        startDate
        endDate
        country
        city
        contentValue {
          image {
            extraSmall
            large
            medium
            original
            small
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
    "Poaps": {
      "Poap": [
        {
          "eventId": "6584",
          "poapEvent": {
            "eventName": "Pudgy Penguin owner as of August 2021",
            "eventURL": "https://www.stonercats.com/",
            "startDate": "2021-08-30T00:00:00Z",
            "endDate": "2021-09-30T00:00:00Z",
            "country": "United States",
            "city": "Ethereum",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/6584/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/6584/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/6584/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/6584/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/6584/small.png"
              }
            }
          }
        },
        {
          "eventId": "14498",
          "poapEvent": {
            "eventName": "ConstitutionDAO Contributor",
            "eventURL": "https://www.constitutiondao.com/",
            "startDate": "2021-11-18T00:00:00Z",
            "endDate": "2021-11-18T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/1/14498/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/1/14498/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/1/14498/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/1/14498/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/1/14498/small.png"
              }
            }
          }
        },
        {
          "eventId": "6481",
          "poapEvent": {
            "eventName": "Fractional Early Adopter",
            "eventURL": "https://fractional.art/",
            "startDate": "2021-08-27T00:00:00Z",
            "endDate": "2021-08-27T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/1/6481/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/1/6481/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/1/6481/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/1/6481/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/1/6481/small.png"
              }
            }
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding fetching token balances of Farcaster user(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

- [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)
- [Balance Snapshots Guides](../../guides/balance-snapshots.md)
- [Holder Snapshots Guides](../../guides/holder-snapshots.md)
- [POAPs API Reference](../../api-references/api-reference/poaps-api.md)
