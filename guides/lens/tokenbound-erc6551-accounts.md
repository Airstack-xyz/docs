---
description: >-
  Learn how to get ERC6551 Accounts owned By Lens profile(s). Get Lens profile
  who own NFTs owned by ERC6551 accounts.
---

# 🪆 Tokenbound ERC6551 Accounts

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to fetch ERC6551 accounts and all assets held inside the account that is owned by a Lens profile(s).

In this guide you will learn how to use Airstack to:

* [Get ERC6551 Account Addresses of Lens profile(s)](tokenbound-erc6551-accounts.md#get-erc6551-account-addresses-of-lens-profile-s)
* [Get Assets Owned by ERC6551 Accounts of Lens profile(s)](tokenbound-erc6551-accounts.md#get-assets-owned-by-erc6551-accounts-of-lens-profile-s)
* [Get Lens profile of Owner that Owns NFT that Owns ERC6551 Accounts](tokenbound-erc6551-accounts.md#get-lens-profile-of-owner-that-owns-nft-that-owns-erc6551-accounts)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [ERC6551](https://eips.ethereum.org/EIPS/eip-6551)

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
#### React

```sh
npm install @airstack/airstack-react
```

#### Node

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
#### React

```sh
yarn add @airstack/airstack-react
```

#### Node

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
#### React

```sh
pnpm install @airstack/airstack-react
```

#### Node

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack asyncio
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

  // Render your component using the data returned by the query
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

## Get ERC6551 Account Addresses of Lens Profile(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/RN1geS8sSR" %}
Show all ERC6551 accounts owned by jayden.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_in: ["jayden.lens"]}}, blockchain: ethereum, limit: 200}
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
                  "addresses": [
                    "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": null
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Assets Owned by ERC6551 Accounts of Lens Profile(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/J14TVepK23" %}
Show all assets owned by ERC6551 accounts owned by jayden.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_in: ["jayden.lens"]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      tokenNfts {
        erc6551Accounts {
          address {
            addresses
            tokenBalances {
              tokenAddress
              tokenId
              amount
              token {
                name
                symbol
                tokenNfts {
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
                  "addresses": [
                    "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
                  ],
                  "tokenBalances": [
                    {
                      "tokenAddress": "0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1",
                      "tokenId": "9707",
                      "amount": "1",
                      "token": {
                        "name": "STAPLEVERSE - FEED CLAN",
                        "symbol": "FEED",
                        "tokenNfts": [
                          {
                            "contentValue": {
                              "image": {
                                "extraSmall": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/extra_small",
                                "large": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/large",
                                "medium": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/medium",
                                "original": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/original_image",
                                "small": "https://assets.airstack.xyz/image/nft/1/0x8ee9a60cb5c0e7db414031856cb9e0f1f05988d1/1/small"
                              }
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
            "erc6551Accounts": null
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Profile of Owner that Owns NFT that Owns ERC6551 Accounts

You can know whether an NFT is owned by an ERC6551 accounts and the NFT that own the account:

### Try Demo

{% embed url="https://app.airstack.xyz/query/E63g0vYA83" %}
Show owner of NFT that owns ERC6551 account with address 0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {filter: {address: {_in: "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"}}, blockchain: ethereum}
  ) {
    Account {
      nft {
        tokenBalances {
          owner {
            addresses
            socials(input: {filter: {dappName: {_eq: lens}}}) {
              profileName
              userId
            }
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
                  "addresses": [
                    "0xa75b7833c78eba62f1c5389f811ef3a7364d44de"
                  ],
                  "socials": [
                    {
                      "profileName": "jayden.lens",
                      "userId": "0xa75b7833c78eba62f1c5389f811ef3a7364d44de"
                    }
                  ]
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

If you have any questions or need help regarding fetching token bound ERC6551 accounts, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Tokenbound ERC6551 Guides](../token-bound-accounts/)
* [Accounts API References](../../api-references/api-reference/accounts-api/)
* [TokenBalances API References](../../api-references/api-reference/tokenbalances-api/)
