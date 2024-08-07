---
description: >-
  Learn how to get NFT (ERC721/1155) balances of user(s), including images and
  metadata, on Ethereum, Base, Degen Chain, and other Airstack-supported chains.
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

# ♦️ NFT

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 applications and integrating NFT balance data from Ethereum, Base, Degen Chain, and other [Airstack-supported chains](../overview.md#supported-chains).

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to [get all NFTs owned by user(s)](nft.md#get-all-nfts-owned-by-user-s).

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL

## Get Started

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

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get All NFTs Owned By User(s)

You can fetch all NFTs owned by any user(s) across multiple chains, e.g. Ethereum, Base, Degen Chain, and other [Airstack-supported chains](../overview.md#supported-chains), using the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api.md) API:

{% hint style="info" %}
For fetching NFT balances data from multiple chains, check out [Cross-Chain Queries](../basics/cross-chain-queries.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/YY707AZGs7" %}
Show NFT on Ethereum owned by users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenBalances(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
        tokenType: { _in: [ERC1155, ERC721] }
      }
<strong>      blockchain: ethereum # This can be changed to other Airstack-supported chains
</strong>      limit: 50
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
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
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
</code></pre>
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
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "isPrimary": false
              },
              // Other ENS domains
            ]
            "socials": [
              {
                "profileName": "vitalik.eth",
                "profileTokenId": "5650",
                "profileTokenIdHex": "0x1612",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              },
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
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

## Developer Support

If you have any questions or need help regarding fetching NFT (ERC721/1155) balances of user(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Balance Snapshots Guides](broken-reference)
* [Holder Snapshots Guides](broken-reference)
* [NFT Guides](../nft/)
* [POAP Guides](../poap/)
* [NFTs In Common Guides](../tokens-in-common/nfts.md)
* [NFT Holders Guides](../nft/nft-holders.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)
* [POAPs API Reference](../../api-references/api-reference/poaps-api.md)
