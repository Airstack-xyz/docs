---
description: >-
  Learn how to use Airstack to get token-bound (ERC6551) accounts by NFTs that
  own the accounts and vice versa
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

# ♦ NFTs

[Airstack](https://airstack.xyz) provides easy-to-use APIs that index both deployed and non-deployed (optimistic) ERC6551 accounts across Ethereum, Polygon, Base, and Zora to enrich ERC6551 dapps with on-chain and off-chain data.

For non-deployed (optimistic) ERC6551 accounts, it will be available in the [`tokenNfts`](../../api-references/api-reference/tokennfts-api.md) nested queries and the value will be calculated through a hashing function that depends on 3 input variables:

| Variables        | Default Value                                | Description                                                                                                                                                                                              |
| ---------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `registry`       | `0x000000006551c19487814612e58FE06813775758` | <p>The registry address of the ERC6551 account. This can be used to indicate the different <a href="account-versions.md">versions</a> of ERC6551 accounts.<br><br>This defaults to registry v.0.3.1.</p> |
| `implementation` | `0x55266d75D1a14E4572138116aF39863Ed6596E7F` | <p>The implementation address of ERC6551 account.<br><br>Defaulting implementation to the official standard ERC6551 implementation address.</p>                                                          |
| `salt`           | 0                                            | The ERC6551 account's salt.                                                                                                                                                                              |

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get Token Bound Accounts (ERC6551) By NFT Collection Address(es)](nfts.md#get-token-bound-accounts-erc6551-by-nft-collection-address-es)
- [Get All Deployed and Non-Deployed (Optimistic) Token Bound Accounts (ERC6551) By NFT Collection Address(es)](nfts.md#get-all-deployed-and-non-deployed-optimistic-token-bound-accounts-erc6551-by-nft-collection-address)
- [Get Optimistic Token Bound Accounts (ERC6551) for Custom Implementations](nfts.md#get-optimistic-custom)
- [Get Cross-Chain Token Bound Accounts (ERC6551) By NFT Collection Address(es)](nfts.md#get-cross-chain-token-bound-accounts-erc6551-by-nft-collection-address-es)
- [Get Token Bound Accounts By Specific NFT](nfts.md#get-token-bound-accounts-by-specific-nft)
- [Get Cross-Chain Token Bound Accounts By Specific NFT](nfts.md#get-cross-chain-token-bound-accounts-erc6551-by-nft-collection-address-es)
- [Get Owner NFT of a Token Bound Account](nfts.md#get-owner-nft-of-a-token-bound-account)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
- Basic knowledge of GraphQL
- Basic knowledge of [ERC6551](https://eips.ethereum.org/EIPS/eip-6551)

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

## Get Token Bound Accounts (ERC6551) By NFT Collection Address(es)

You can get all the deployed ERC6551 accounts owned by a given [NFT collection address(es)](#user-content-fn-1)[^1]:&#x20;

{% hint style="info" %}
If you would like to include NFTs with no TBA and show the associated non-deployed (optimistic) TBAs to be included in the result, then check [this query](nfts.md#get-all-deployed-and-non-deployed-optimistic-token-bound-accounts-erc6551-by-nft-collection-address) instead.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/Rezf4zOTBz" %}
Get Token Bound Accounts (ERC6551) By NFT Collection Address(es)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Accounts(
    input: {
      filter: {
        tokenAddress: { _in: ["0x26727ed4f5ba61d3772d1575bca011ae3aef5d36"] }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Account {
      address {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
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
          "address": {
            "addresses": ["0x776c56774a83a80c6ab608385418f6ffb2e088fb"],
            "domains": [
              {
                "isPrimary": true,
                "name": "skynft.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@sapienz_0",
                "profileTokenId": "117100",
                "profileTokenIdHex": "0x01c96c",
                "userId": "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5",
                "userAssociatedAddresses": [
                  "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
                ]
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

## Get All Deployed and Non-Deployed (Optimistic) Token Bound Accounts (ERC6551) By NFT Collection Address(es)

You can fetch all deployed and non-deployed (optimistic) TBAs on NFT Collection(s) by using the [`TokenNfts`](../../api-references/api-reference/tokennfts-api.md) API and providing the NFT collection address to the `address` filter:

{% hint style="info" %}
For non-deployed (optimistic) TBAs, it can be checked through some of the fields' value:

- `createdAtBlockNumber`: -1
- `createdAtBlockTimestamp`: `null`
- `creationTransactionHash`: `null`
  {% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/ZQg2Vl8HmO" %}
Show me all TBAs, both deployed and non-deployed (optimistic), on Sapienz NFT collection
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenNfts(
    input: {
      filter: {
        address: {_in: ["0x26727ed4f5ba61d3772d1575bca011ae3aef5d36"]}
      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    TokenNft {
      address
      tokenId
      erc6551Accounts(
      # set this to `true` to show all optimistic/non-deployed and deployed TBAs
      # set this to `false` to show all deployed TBA accounts
<strong>        input: {showOptimisticAddress: true}
</strong>      ) {
        createdAtBlockNumber
        createdAtBlockTimestamp
        address {
          addresses
        }
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenNfts": {
      "TokenNft": [
        {
          "tokenId": "0",
          // This NFT have 2 deployed TBAs on Ethereum
<strong>          "erc6551Accounts": [
</strong>            {
<strong>              "createdAtBlockNumber": 17213826, // Block number when the TBA was created
</strong>              "createdAtBlockTimestamp": "2023-05-08T05:53:59Z",
              "address": {
                "addresses": [
                  "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
                ]
              }
            },
            {
              "createdAtBlockNumber": 18467201,
              "createdAtBlockTimestamp": "2023-10-31T02:40:23Z",
              "address": {
                "addresses": [
                  "0xb5307cb1ae1385f64de8442d5d48ff86479f4f8c"
                ]
              }
            }
          ]
        },
        {
          "tokenId": "100",
          "erc6551Accounts": [
            {
              // This NFT have no TBA
<strong>              "createdAtBlockNumber": -1,
</strong>              "createdAtBlockTimestamp": null,
              "address": {
                "addresses": [
                  // This TBA address is optimistic
<strong>                  "0x35dc2fd127fcc7a0ed33b29d7e58e4bc01b13b2d"
</strong>                ]
              }
            }
          ]
        },
        //
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Get Optimistic Token Bound Accounts (ERC6551) for Custom Implementations <a href="#get-optimistic-custom" id="get-optimistic-custom"></a>

You can fetch non-deployed (optimistic) TBAs on NFT Collection(s) by using the [`TokenNfts`](../../api-references/api-reference/tokennfts-api.md) API and providing the NFT collection address. You can control which optimistic accounts to retrieve by changing the values of implementation, registry, and salt in the input filters.

{% hint style="info" %}
At the moment only \_eq is supported for the inputs. If you want to get multiple custom implementations in one call, you need to duplicate your query and set aliases for each if you want to pass them in one API call. You can find an example how to do that [here](https://app.airstack.xyz/query/pcCmFDJVZv).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/RR88ljW7Be" %}
Get Moonbirds NFT collection Optimistic TBAs with Custom Parameters
{% endembed %}

{% tabs %}
{% tab title="Query" %}

```graphql
query GetOptimisticWithCustomParameters {
  TokenNfts(
    input: {
      filter: { address: { _eq: "0x23581767a106ae21c074b2276D25e5C3e136a68b" } }
      blockchain: ethereum
    }
  ) {
    TokenNft {
      erc6551Accounts(
        input: {
          filter: {
            # Your custom implementation address
            implementation: {
              _eq: "0x2d25602551487c3f3354dd80d76d54383a243358"
            }
            # Your custom registry address
            registry: { _eq: "0x02101dfB77FDE026414827Fdc604ddAF224F0921" }
            # Your salt number
            salt: { _eq: "0" }
          }
          showOptimisticAddress: true
          limit: 200
        }
      ) {
        implementation
        registry
        salt
        createdAtBlockNumber
        createdAtBlockTimestamp
        creationTransactionHash
        address {
          # Optimistic TBA address
          addresses
        }
      }
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```graphql
{
  "data": {
    "TokenNfts": {
      "TokenNft": [
        {
          "erc6551Accounts": [
            {
              "implementation": "0x2d25602551487c3f3354dd80d76d54383a243358",
              "registry": "0x02101dfb77fde026414827fdc604ddaf224f0921",
              "salt": "0",
              # Block Number "-1" means it's not yet deployed
              "createdAtBlockNumber": -1,
              "createdAtBlockTimestamp": null,
              "creationTransactionHash": null,
              "address": {
                "addresses": [
                # Optimistic TBA address
                  "0xc6dd4ab4631f526fe4f8d4fc73d5136570c4d035"
                ]
              }
            }
          ]
        },
```

{% endtab %}
{% endtabs %}

## Get Cross-Chain Token Bound Accounts (ERC6551) By NFT Collection Address(es)

You can fetch the cross-chain token bound ERC6551 accounts of an NFT collection address(es), e.g. all Polygon ERC6551 accounts owned by an Ethereum NFT collection [Art Blocks](https://explorer.airstack.xyz/token-holders?activeView=&address=0xa7d8d9ef8D8Ce8992Df33D8b8CF4Aebabd5bD270&tokenType=&rawInput=%23%E2%8E%B1Art+Blocks%E2%8E%B1%280xa7d8d9ef8D8Ce8992Df33D8b8CF4Aebabd5bD270+NFT_COLLECTION+ethereum+null%29&inputType=NFT_COLLECTION&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&activeSocialInfo=&blockchain=ethereum):

### Try Demo

{% embed url="https://app.airstack.xyz/query/pGvkCRA8Fa" fullWidth="false" %}
Show all Polygon ERC6551 accounts owned by Ethereum NFT Artblocks
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Accounts(
    input: {
      blockchain: polygon
      filter: {
        tokenAddress: { _in: "0xa7d8d9ef8D8Ce8992Df33D8b8CF4Aebabd5bD270" }
      }
    }
  ) {
    Account {
      address {
        addresses
        domains {
          isPrimary
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
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
          "address": {
            "addresses": ["0x03d73cfdcb8dc66315b8c370edd7ba71d0ee658b"],
            "domains": {
              "isPrimary": false,
              "name": "cryptocitizen52.eth"
            },
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@cryptocitizen52",
                "profileTokenId": "123612",
                "profileTokenIdHex": "0x01e2dc",
                "userId": "0x03d73cfdcb8dc66315b8c370edd7ba71d0ee658b",
                "userAssociatedAddresses": [
                  "0x03d73cfdcb8dc66315b8c370edd7ba71d0ee658b"
                ]
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

## Get Token Bound Accounts By Specific NFT

You can get all the token bound accounts given by a specific NFT with contract address `tokenAddress` and token ID `tokenId`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/j4yjmfO6wq" %}
Get Token Bound Accounts By Specific NFT (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Accounts(
    input: {
      filter: {
        tokenAddress: { _eq: "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36" }
        tokenId: { _eq: "1" }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Account {
      address {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          profileName
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
          "address": {
            "addresses": ["0x718a9d173e66c411f48e41d3da2fa6f0ce8f5d3c"],
            "domains": [
              {
                "isPrimary": true,
                "name": "skynft.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@0xjst",
                "dappName": "lens"
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

## Get Cross-Chain Token Bound Accounts By Specific NFT

You can get the cross-chain token bound ERC6551 accounts of a specific NFT, e.g. all ERC6551 accounts on both Polygon that is owned by a given Ethereum [Art Blocks](https://explorer.airstack.xyz/token-holders?activeView=&address=0xa7d8d9ef8D8Ce8992Df33D8b8CF4Aebabd5bD270&tokenType=&rawInput=%23%E2%8E%B1Art+Blocks%E2%8E%B1%280xa7d8d9ef8D8Ce8992Df33D8b8CF4Aebabd5bD270+NFT_COLLECTION+ethereum+null%29&inputType=NFT_COLLECTION&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&activeSocialInfo=&blockchain=ethereum) NFT:

### Try Demo

{% embed url="https://app.airstack.xyz/query/iTTC3BM1TN" %}
Show me the Polygon ERC6551 accounts owned by Ethereum Art Blocks NFT token ID 95000052
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Accounts(
    input: {
      blockchain: polygon
      filter: {
        tokenAddress: { _eq: "0xa7d8d9ef8D8Ce8992Df33D8b8CF4Aebabd5bD270" }
        tokenId: { _eq: "95000052" }
      }
    }
  ) {
    Account {
      address {
        addresses
        domains {
          isPrimary
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
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
          "address": {
            "addresses": ["0x03d73cfdcb8dc66315b8c370edd7ba71d0ee658b"],
            "domains": {
              "isPrimary": false,
              "name": "cryptocitizen52.eth"
            },
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@cryptocitizen52",
                "profileTokenId": "123612",
                "profileTokenIdHex": "0x01e2dc",
                "userId": "0x03d73cfdcb8dc66315b8c370edd7ba71d0ee658b",
                "userAssociatedAddresses": [
                  "0x03d73cfdcb8dc66315b8c370edd7ba71d0ee658b"
                ]
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

## Get Owner NFT of a Token Bound Account

You can get the owner NFT of a specific token bound account `address`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/5EE9p28NXP" %}
Get Owner NFT of a Token Bound Account (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Accounts(
    input: {
      blockchain: ethereum
      filter: { address: { _in: "0xa1afb6a11ef500229538bfb38d5a0b8c1b61b425" } }
    }
  ) {
    Account {
      nft {
        address
        metaData {
          name
          description
          image
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
            "address": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
            "metaData": {
              "name": "Sapienz #3504",
              "description": "SAPIENZ is a collection of 15,000 networked playable characters created in partnership between STAPLEVERSE and RHYMEZLIKEDIMEZ. Imagine a reality where the marriage of art and commerce is seamless, where creativity is the currency and collaboration the fuel that powers our collective evolution. This is not merely a vision of the future, but a tangible reality within your grasp with the SAPIENZ world. The goal is to build the next 100+ years of street culture, are you in?",
              "image": "https://sapienz.imgix.net/rendered/3504.png?s=5d9251a2fe167597a1f5fbdf12e84ab3"
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

## Developer Support

If you have any questions or need help regarding fetching ERC6551 token bound accounts data by NFTs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Accounts API Reference](../../api-references/api-reference/accounts-api.md)
- [TokenNfts API Reference](../../api-references/api-reference/tokennfts-api.md)

[^1]: This is represented as `tokenAddress` parameter in the GraphQL query.
