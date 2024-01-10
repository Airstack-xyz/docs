---
description: >-
  Learn how to use Airstack to get token-bound (ERC6551) accounts by the owner
  of NFT that owns the accounts and vice versa.
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

# 📭 NFT Owners

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching ERC6551 dapps and integrating on-chain and off-chain data.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get Token Bound Accounts By NFT Owner Address](nft-owners.md#get-token-bound-accounts-by-nft-owner-address)
* [Get The Owner Of NFT That Owns A Given Token Bound Accounts Address](nft-owners.md#get-the-owner-of-nft-that-owns-a-given-token-bound-accounts-address)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [ERC6551](https://eips.ethereum.org/EIPS/eip-6551)

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

## Get Token Bound Accounts By NFT Owner Address

You can fetch all the token bound accounts owned by a given [NFT owner](#user-content-fn-1)[^1] address `owner`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/5GhSQXiErE" %}
Get Token Bound Accounts By NFT Owner Address (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: { owner: { _in: "0xcf94ba8779848141d685d44452c975c2ddc04945" } }
      blockchain: ethereum
      limit: 200
    }
  ) {
    TokenBalance {
      tokenNfts {
        erc6551Accounts {
          address {
            addresses
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
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": ["0x9ff8faf2c61f50d24677e9cb5aaf988c91525539"]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": ["0x5f27c4c4f66fbb9d1bddbcef60ada4731757b128"]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": ["0x5661094b8b369aff4075a9a75a1bcc51cdb5901e"]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": ["0x28d3fb76ad7e1076735a2bac3cac260c6349f45b"]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": ["0x7bc2cb8d74c2238a126fd495c7ce079ae36e6396"]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Owner Of NFT That Owns A Given Token Bound Accounts Address

### Try Demo

{% embed url="https://app.airstack.xyz/query/dgS4j5UWyj" %}
Get The Owner Of NFT That Owns A Given Token Bound Accounts Address
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {
      filter: { address: { _in: "0x9ff8faf2c61f50d24677e9cb5aaf988c91525539" } }
      blockchain: ethereum
    }
  ) {
    Account {
      nft {
        tokenBalances {
          owner {
            addresses
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
    "Accounts": {
      "Account": [
        {
          "nft": {
            "tokenBalances": [
              {
                "owner": {
                  "addresses": ["0xcf94ba8779848141d685d44452c975c2ddc04945"]
                }
              }
            ]
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

If you have any questions or need help regarding fetching ERC6551 token bound accounts data by NFT owners, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Accounts API Reference](../../api-references/api-reference/accounts-api.md)

[^1]: owner of NFT that owns the ERC6551 accounts
