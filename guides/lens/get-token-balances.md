---
description: >-
  Learn how to get ERC20, 721, 1155, and POAPs Of Lens Profile(s), including
  images and metadata, on Ethereum, Polygon, Base, and Gnosis (POAPs).
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

## 💰 Get Token Balances

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get Ethereum ERC20s Owned By Lens Profile(s)](get-token-balances.md#get-ethereum-erc20s-owned-by-lens-profile-s)
* [Get Polygon ERC20s Owned By Lens Profile(s)](get-token-balances.md#get-polygon-erc20s-owned-by-lens-profile-s)
* [Get Base ERC20s Owned By Lens Profile(s)](get-token-balances.md#get-base-erc20s-owned-by-lens-profile-s)
* [Get All ERC20s Owned By Lens Profile(s)](get-token-balances.md#get-all-erc20s-owned-by-lens-profile-s)
* [Get Ethereum NFTs Owned By Lens Profile(s)](get-token-balances.md#get-ethereum-nfts-owned-by-lens-profile-s)
* [Get Polygon NFTs Owned By Lens Profile(s)](get-token-balances.md#get-polygon-nfts-owned-by-lens-profile-s)
* [Get Base NFTs Owned By Lens Profile(s)](get-token-balances.md#get-base-nfts-owned-by-lens-profile-s)
* [Get All NFTs Owned By Lens Profile(s)](get-token-balances.md#get-all-nfts-owned-by-lens-profile-s)
* [Get All POAPs Owned By Lens Profile(s)](get-token-balances.md#get-all-poaps-owned-by-lens-profile-s)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

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

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

### Get Ethereum ERC20s Owned By Lens Profile(s)

You can fetch all ERC20 tokens on Ethereum owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/iHvIqJvOZh" %}
Show ERC20 tokens on Ethereum owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "45934484403886362668",
          "tokenAddress": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "token": {
            "name": "Wrapped Ether",
            "symbol": "WETH"
          }
        }
        // Other Ethereum ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Polygon ERC20s Owned By Lens Profile(s)

You can fetch all ERC20 tokens on Polygon owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/C8M7BrhcDK" %}
Show ERC20 tokens on Polygon owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "44281129",
          "tokenAddress": "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
          "token": {
            "name": "(PoS) Tether USD",
            "symbol": "USDT"
          }
        }
        // Other Polygon ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Base ERC20s Owned By Lens Profile(s)

You can fetch all ERC20 tokens on Base owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/oU8wM39MWT" %}
Show ERC20 tokens on Base owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "310194000000000000000000",
          "tokenAddress": "0xac1bd2486aaf3b5c0fc3fd868558b082a531b2b4",
          "token": {
            "name": "Toshi",
            "symbol": "TOSHI"
          }
        }
        // Other Base ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get All ERC20s Owned By Lens Profile(s)

You can fetch all ERC20 tokens on Ethereum, Polygon, and Base owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/aXsVio5nvm" %}
Show ERC20 tokens on Ethereum, Polygon, and Base owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query ERC20OwnedByLensProfiles {
  Ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
  Polygon: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _eq: ERC20 }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "45934484403886362668",
          "tokenAddress": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "token": {
            "name": "Wrapped Ether",
            "symbol": "WETH"
          }
        }
        // Other Ethereum ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Polygon": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "amount": "457218374987121000000",
          "tokenAddress": "0x086373fad3447f7f86252fb59d56107e9e0faafa",
          "token": {
            "name": "Yup",
            "symbol": "YUP"
          }
        }
        // Other Polygon ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "310194000000000000000000",
          "tokenAddress": "0xac1bd2486aaf3b5c0fc3fd868558b082a531b2b4",
          "token": {
            "name": "Toshi",
            "symbol": "TOSHI"
          }
        }
        // Other Base ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Ethereum NFTs Owned By Lens Profile(s)

You can fetch all NFTs on Ethereum owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/xoAjUpSHcs" %}
Show NFT on Ethereum owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
          "tokenId": "93631715144692179688067815556165775057916676179424585455268666624027958254283",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/extra_small.svg",
                "small": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/small.svg",
                "medium": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/medium.svg",
                "large": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/large.svg"
              }
            }
          }
        }
        // Other Ethereum NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Polygon NFTs Owned By Lens Profile(s)

You can fetch all NFTs on Polygon owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/xSHo2KgP33" %}
Show NFT on Polygon owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@bradorbradley",
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x5ef718b8360ef2b82fb971b50350913e2bad4783",
          "tokenId": "2525",
          "tokenType": "ERC1155",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/large.png"
              }
            }
          }
        }
        // Other Polygon NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Base NFTs Owned By Lens Profile(s)

You can fetch all NFTs on Base owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/kfGo5b21OJ" %}
Show NFT on Base owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
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
                "large": "https://assets.airstack.xyz/image/nft/8453/KEYM3ihZQ1f7hNMrnxylaC9aymzRDEpR6k24WvE1bOeYTCP3FBEY8v0XMCaJUXyzqno4Vvx93EOQrbiH+i4aKQ==/large.jpg"
              }
            }
          }
        }
        // Other Base NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get All NFTs Owned By Lens Profile(s)

You can fetch all NFTs on Ethereum, Polygon, and Base owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/xoAjUpSHcs" %}
Show NFT on Ethereum and Polygon owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query NFTsOwnedByLensProfiles {
  Ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
  Polygon: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
        owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
          "tokenId": "93631715144692179688067815556165775057916676179424585455268666624027958254283",
          "tokenType": "ERC721",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/extra_small.svg",
                "small": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/small.svg",
                "medium": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/medium.svg",
                "large": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3ANGkOaqLt5uB0akSKQqzzwkFP2k3F+pM22yvq3atTA66A1hk52OxQkPc5GWp5cl6hkqffkEcsvP3JAWyEPPyYsKMKIbbP1VsMuvSSOA7NTW+/a2HkQPhYY/PVrG6O9Is=/large.svg"
              }
            }
          }
        }
        // Other Ethereum NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Polygon": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "lens/@bradorbradley",
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "amount": "1",
          "tokenAddress": "0x5ef718b8360ef2b82fb971b50350913e2bad4783",
          "tokenId": "2525",
          "tokenType": "ERC1155",
          "tokenNfts": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/extra_small.png",
                "small": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/small.png",
                "medium": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/medium.png",
                "large": "https://assets.airstack.xyz/image/nft/137/0x5ef718b8360ef2b82fb971b50350913e2bad4783/2525/large.png"
              }
            }
          }
        }
        // Other Polygon NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
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
                "large": "https://assets.airstack.xyz/image/nft/8453/KEYM3ihZQ1f7hNMrnxylaC9aymzRDEpR6k24WvE1bOeYTCP3FBEY8v0XMCaJUXyzqno4Vvx93EOQrbiH+i4aKQ==/large.jpg"
              }
            }
          }
        }
        // Other Base NFTs
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get All POAPs Owned By Lens Profile(s)

You can fetch all POAPs tokens owned by any Lens Profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/T4g7vpBWkX" %}
Show POAPs owned by lens/@bradorbradley and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByLensProfiles {
  Poaps(
    input: {
      filter: { owner: { _in: ["lens/@bradorbradley", "lens_id:100275"] } }
      blockchain: ALL
    }
  ) {
    Poap {
      eventId
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
      }
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
          "eventId": "80393",
          "owner": {
            "socials": [
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "poapEvent": {
            "eventName": "You met Cryptocomical at Lisbon 2022",
            "eventURL": "https://cards.io",
            "startDate": "2022-10-30T00:00:00Z",
            "endDate": "2022-11-15T00:00:00Z",
            "country": "Portugal",
            "city": "Lisbon",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/small.png"
              }
            }
          }
        },
        {
          "eventId": "47553",
          "owner": {
            "socials": [
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "poapEvent": {
            "eventName": "Proof of rAAVE - ETHCC Paris 2022",
            "eventURL": "https://aave.com",
            "startDate": "2022-07-01T00:00:00Z",
            "endDate": "2022-07-01T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/small.png"
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

If you have any questions or need help regarding fetching token balances of Lens profile(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
