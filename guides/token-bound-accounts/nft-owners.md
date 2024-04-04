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

[Airstack](https://airstack.xyz) provides easy-to-use APIs that index both deployed and non-deployed (optimistic) ERC6551 accounts across Ethereum and Base to enrich ERC6551 dapps with on-chain and off-chain data.

For non-deployed (optimistic) ERC6551 accounts, it will be available in the [`tokenNfts`](../../api-references/api-reference/tokennfts-api.md) nested queries and the value will be calculated through a hashing function that depends on 3 input variables:

| Variables        | Default Value                                | Description                                                                                                                                                                                              |
| ---------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `registry`       | `0x000000006551c19487814612e58FE06813775758` | <p>The registry address of the ERC6551 account. This can be used to indicate the different <a href="account-versions.md">versions</a> of ERC6551 accounts.<br><br>This defaults to registry v.0.3.1.</p> |
| `implementation` | `0x55266d75D1a14E4572138116aF39863Ed6596E7F` | <p>The implementation address of ERC6551 account.<br><br>Defaulting implementation to the official standard ERC6551 implementation address.</p>                                                          |
| `salt`           | 0                                            | The ERC6551 account's salt.                                                                                                                                                                              |

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get Token Bound Accounts By NFT Owner Address](nft-owners.md#get-token-bound-accounts-by-nft-owner-address)
* [Get The Owner Of NFT That Owns A Given Token Bound Accounts Address](nft-owners.md#get-the-owner-of-nft-that-owns-a-given-token-bound-accounts-address)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
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

{% hint style="info" %}
For non-deployed (optimistic) TBAs, it can be checked through some of the fields' value:

* `createdAtBlockNumber`: -1
* `createdAtBlockTimestamp`: `null`
* `creationTransactionHash`: `null`
{% endhint %}

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
      filter: {
        owner: { _in: "0xa75b7833c78EBA62F1C5389f811ef3A7364D44DE" }
        tokenType: { _eq: ERC721 }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    TokenBalance {
      tokenNfts {
        address
        tokenId
        erc6551Accounts {
          address {
            addresses
          }
          createdAtBlockNumber
          createdAtBlockTimestamp
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "tokenNfts": {
            "address": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
            "tokenId": "0",
            // This NFT have 2 TBAs deployed on Ethereum mainnet
<strong>            "erc6551Accounts": [
</strong>              {
                "address": {
                  "addresses": [
                    "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
                  ]
                },
                "createdAtBlockNumber": 17213826,
                "createdAtBlockTimestamp": "2023-05-08T05:53:59Z"
              },
              {
                "address": {
                  "addresses": [
                    "0xb5307cb1ae1385f64de8442d5d48ff86479f4f8c"
                  ]
                },
                "createdAtBlockNumber": 18467201,
                "createdAtBlockTimestamp": "2023-10-31T02:40:23Z"
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "address": "0xa7d8d9ef8d8ce8992df33d8b8cf4aebabd5bd270",
            "tokenId": "207000185",
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    // Non-deployed (Optimistic) TBA address
<strong>                    "0x062e978c4867df2ed10a3092234659829d44b9f1"
</strong>                  ]
                },
<strong>                "createdAtBlockNumber": -1, // This indicate that the TBA has not been deployed yet
</strong>                "createdAtBlockTimestamp": null
              }
            ]
          }
        },
        // Other ERC721 NFTs with their TBA address
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get The Owner Of NFT That Owns A Given Token Bound Accounts Address

You can find the owner of the NFT that owns a given TBA by using the `Accounts` API and providing the TBA address to the `address` fitler:

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
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)

[^1]: owner of NFT that owns the ERC6551 accounts
